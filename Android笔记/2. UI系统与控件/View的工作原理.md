# 工作过程

> **View的绘制其实是从Activity的onResume()之后才开始的.**

在Activity的启动过程中,ActivityThread中的handleLaunchActivity，performLaunchActivity，handleResumeActivity,这3个主要的方法完成了Activity的创建到启动工作.

## handleLaunchActivity()

```java
private void handleLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    ...

    if (localLOGV) Slog.v(
        TAG, "Handling launch of " + r);
    
    //分析1 : 这里是创建Activity,并调用了Activity的onCreate()和onStart()
    Activity a = performLaunchActivity(r, customIntent);

    if (a != null) {
        r.createdConfig = new Configuration(mConfiguration);
        Bundle oldState = r.state;
        //分析2 : 这里调用Activity的onResume()
        handleResumeActivity(r.token, false, r.isForward,
                !r.activity.mFinished && !r.startsNotResumed);
    }
    ....
}
```

handleLaunchActivity()主要是为了调用performLaunchActivity()和handleResumeActivity()

## performLaunchActivity()

```java
private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    ......
    //分析1 : 这里底层是通过反射来创建的Activity实例
    java.lang.ClassLoader cl = r.packageInfo.getClassLoader();
    activity = mInstrumentation.newActivity(cl, component.getClassName(), r.intent);

    //底层也是通过反射构建Application,如果已经构建则不会重复构建,毕竟一个进程只能有一个Application
    Application app = r.packageInfo.makeApplication(false, mInstrumentation);

    if (activity != null) {
        Context appContext = createBaseContextForActivity(r, activity);
        CharSequence title = r.activityInfo.loadLabel(appContext.getPackageManager());
        Configuration config = new Configuration(mCompatConfiguration);
        if (DEBUG_CONFIGURATION) Slog.v(TAG, "Launching activity "
                + r.activityInfo.name + " with config " + config);
        //分析2 : 在这里实例化了PhoneWindow,并将该Activity设置为PhoneWindow的Callback回调,还初始化了WindowManager
        activity.attach(appContext, this, getInstrumentation(), r.token,
                r.ident, app, r.intent, r.activityInfo, title, r.parent,
                r.embeddedID, r.lastNonConfigurationInstances, config);

        //分析3 : 间接调用了Activity的performCreate方法,间接调用了Activity的onCreate方法.
        mInstrumentation.callActivityOnCreate(activity, r.state);
        
        //分析4: 这里和上面onCreate过程差不多,调用Activity的onStart方法
        if (!r.activity.mFinished) {
            activity.performStart();
            r.stopped = false;
        }
        ....
    }
}
```

主要过程:

1. 通过反射来创建的Activity实例
2. 在这里实例化了PhoneWindow,并将该Activity设置为PhoneWindow的Callback回调.建立起Activity与PhoneWindow之间的联系.
3. 调用了Activity的onCreate方法
4. 调用Activity的onStart方法

对于分析1中的代码:

```java
public Activity newActivity(ClassLoader cl, String className,
        Intent intent)
        throws InstantiationException, IllegalAccessException,
        ClassNotFoundException {
    //反射->实例化
    return (Activity)cl.loadClass(className).newInstance();
}
```

对于分析2中的代码:

```java
final void attach(Context context, ActivityThread aThread,
        Instrumentation instr, IBinder token, int ident,
        Application application, Intent intent, ActivityInfo info,
        CharSequence title, Activity parent, String id,
        NonConfigurationInstances lastNonConfigurationInstances,
        Configuration config, String referrer, IVoiceInteractor voiceInteractor,
        Window window, ActivityConfigCallback activityConfigCallback) {
    ......

    //实例化PhoneWindow
    mWindow = new PhoneWindow(this, window, activityConfigCallback);
    mWindow.setWindowControllerCallback(this);
    mWindow.setCallback(this);
    mWindow.setOnWindowDismissedCallback(this);
    
    .....
    //还有一些其他的配置代码

    mWindow.setWindowManager(
            (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
            mToken, mComponent.flattenToString(),
            (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);

    mWindowManager = mWindow.getWindowManager();
    ....
}
```

对于分析3中的代码:

```java
//首先是来到Instrumentation的callActivityOnCreate方法
public void callActivityOnCreate(Activity activity, Bundle icicle) {
    prePerformCreate(activity);
    activity.performCreate(icicle);
    postPerformCreate(activity);
}

//然后就来到Activity的performCreate方法
final void performCreate(Bundle icicle) {
    performCreate(icicle, null);
}

final void performCreate(Bundle icicle, PersistableBundle persistentState) {
    ....
    if (persistentState != null) {
        onCreate(icicle, persistentState);
    } else {
        onCreate(icicle);
    }
    ......
}

```

对于分析4中的代码:

```java
//Activity
final void performStart() {
    ......
    mInstrumentation.callActivityOnStart(this);
    ......
}

//Instrumentation
public void callActivityOnStart(Activity activity) {
    activity.onStart();
}
```

## handleResumeActivity()

```java
final void handleResumeActivity(IBinder token,
        boolean clearHide, boolean isForward, boolean reallyResume, int seq, String reason) {
    .....
    //分析1 : 在其内部调用Activity的onResume方法
    r = performResumeActivity(token, clearHide, reason);

    .....
    r.window = r.activity.getWindow();
    View decor = r.window.getDecorView();
    decor.setVisibility(View.INVISIBLE);
    //获取WindowManager
    ViewManager wm = a.getWindowManager();
    WindowManager.LayoutParams l = r.window.getAttributes();
    a.mDecor = decor;

    if (a.mVisibleFromClient) {
        .....
        //分析2 : WindowManager添加DecorView
        wm.addView(decor, l);
        ...
    }
    .....

}
```

细看分析2中的逻辑:

```java
@Override
public void addView(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
    applyDefaultToken(params);
    mGlobal.addView(view, params, mContext.getDisplay(), mParentWindow);
}
```

我们看到,方法里面将逻辑交给了mGlobal,mGlobal是WindowManagerGlobal,WindowManagerGlobal是全局单例.WindowManagerImpl的方法都是由WindowManagerGlobal完成的.我们跟着来到了WindowManagerGlobal的addView方法.

```java
public void addView(View view, ViewGroup.LayoutParams params,
        Display display, Window parentWindow) {
    ....

    ViewRootImpl root;

    synchronized (mLock) {
        root = new ViewRootImpl(view.getContext(), display);
        view.setLayoutParams(wparams);

        .....

        root.setView(view, wparams, panelParentView);
    }
}
```

在这里我们看到了,实例化ViewRootImpl,然后建立ViewRootImpl与View的联系.跟着进入setView方法

```java
public void setView(View view, WindowManager.LayoutParams attrs, View panelParentView) {
    .....
    requestLayout();
    .....
}

@Override
public void requestLayout() {
    if (!mHandlingLayoutInLayoutRequest) {
        //检查线程合法性
        checkThread();
        mLayoutRequested = true;
        scheduleTraversals();
    }
}

void scheduleTraversals() {
    if (!mTraversalScheduled) {
        mTraversalScheduled = true;
        mTraversalBarrier = mHandler.getLooper().getQueue().postSyncBarrier();
        mChoreographer.postCallback(
                Choreographer.CALLBACK_TRAVERSAL, mTraversalRunnable, null);
        ......
    }
}

final class TraversalRunnable implements Runnable {
    @Override
    public void run() {
        doTraversal();
    }
}
final TraversalRunnable mTraversalRunnable = new TraversalRunnable();


void doTraversal() {
    if (mTraversalScheduled) {
        ....
        performTraversals();
        ....
    }
}

```

一路下来,我们来到了熟悉的方法面前:performTraversals方法.

## performTraversals()

performTraversals()方法相信大家都已经非常熟悉啦,它是整个View绘制的核心,从measure到layout,再从layout到draw,全部在这个方法里面完成了,所以这个方法里面的代码非常长,这是肯定的.由于本人水平有限,就不每句代码逐行分析了,我们需要学习的是一个主要的流程.

下面的performTraversals()方法的超精简代码,里面的代码真的超级超级多,下面是主要流程,也是今天的主角

```java
private void performTraversals() {
    //分析1 : 这里面会调用performMeasure开始测量流程
    measureHierarchy(host, lp, res,
                    desiredWindowWidth, desiredWindowHeight);
    //分析2 : 开始布局流程
    performLayout(lp, mWidth, mHeight);
    //分析3 : 开始绘画流程
    performDraw();
}
```

首先,来个主要的流程图,这个是performTraversals()的大致流程.

![](https://raw.githubusercontent.com/xfhy/Android-Notes/master/Images/performTraversals%E7%9A%84%E5%A4%A7%E8%87%B4%E6%B5%81%E7%A8%8B.png)

如上面的代码所示,performTraversals()会依次调用performMeasure、performLayout、performDraw方法,而这三个方法是View的绘制流程的核心所在.

- performMeasure : 在performMeasure里面会调用measure方法,然后measure会调用onMeasure方法,而在onMeasure方法中则会对所有的子元素进行measure过程.这相当于完成了一次从父元素到子元素的measure传递过程,如果子元素是一个ViewGroup,那么继续向下传递,直到所有的View都已测量完成.测量完成之后,我们可以根据getMeasureHeight和getMeasureWidth方法获取该View的高度和宽度.
- performLayout : performLayout的原理其实是和performMeasure差不多,在performLayout里面调用了layout方法,然后在layout方法会调用onLayout方法,onLayout又会对所有子元素进行layout过程.由父元素向子元素传递,最终完成所有View的layout过程.确定View的4个点: left+top+right+bottom,layout完成之后可以通过getWidth和getHeight获取View的最终宽高.
- performDraw : 也是和performMeasure差不多,从父元素从子元素传递.在performDraw里面会调用draw方法,draw方法再调用drawSoftware方法,drawSoftware方法里面回调用View的draw方法,然后再通过dispatchDraw方法分发,遍历所有子元素的draw方法,draw事件就这样一层层地传递下去.

# View测量流程

## MeasureSpec

`MeasureSpec` 是View的内部类，它通过封装父容器对子 View 的测量要求（模式和大小），为 View 的测量过程提供标准化的支持。

![image-20241201194335030](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241201194335030.png)

> `MeasureSpec` 是一个 32 位的 `int` 类型，它将测量模式（`SpecMode`）和测量大小（`SpecSize`）封装在一个单一的整数中。系统通过位运算来从中提取这两部分，并且也提供了方法来将它们重新组合成一个完整的 `MeasureSpec``

### 组成

`MeasureSpec` 是由一个 32 位整数组成的，其中：

- **高 2 位** 表示测量模式（Mode）。
- **低 30 位** 表示测量的大小（Size）。

### 测量模式

`MeasureSpec` 提供了三种测量模式：

1. **`UNSPECIFIED` (0x0)**：
   - 父容器没有对 View 限制任何尺寸，View 可以是任意大小。
   - 通常用于内部计算，父容器不会主动使用。
2. **`EXACTLY` (0x1)**：
   - 父容器已经确定了 View 的具体大小，View 的最终大小就是SpecSize
   - 对应于布局中的 `match_parent` 或固定大小（如 `100dp`）。
3. **`AT_MOST` (0x2)**：
   - 父容器指定了 View 的可用大小为SpecSize，View 不能超过这个大小。
   - 对应于布局中的 `wrap_content`。

### 源码

```java
public static class MeasureSpec {
    // 用于控制模式位的位置
    private static final int MODE_SHIFT = 30;
    private static final int MODE_MASK  = 0x3 << MODE_SHIFT; // 用于提取高 2 位模式的掩码

    // 常量定义，代表不同的测量模式
    public static final int UNSPECIFIED = 0 << MODE_SHIFT; // 未指定模式
    public static final int EXACTLY     = 1 << MODE_SHIFT; // 精确模式
    public static final int AT_MOST     = 2 << MODE_SHIFT; // 最大模式

    /**
     * 创建一个 MeasureSpec 值，结合测量大小和模式
     * 
     * @param size 测量大小，范围是 0 到 (2^30 - 1)
     * @param mode 测量模式（UNSPECIFIED, EXACTLY, AT_MOST）
     * @return 返回组合后的 MeasureSpec 值
     */
    public static int makeMeasureSpec(@IntRange(from = 0, to = (1 << MeasureSpec.MODE_SHIFT) - 1) int size,
                                      @MeasureSpecMode int mode) {
        // 判断是否使用兼容模式
        if (sUseBrokenMakeMeasureSpec) {
            // 如果兼容模式开启，直接将 size 和 mode 相加
            return size + mode;
        } else {
            // 使用位运算清除模式位，再设置模式位
            return (size & ~MODE_MASK) | (mode & MODE_MASK);
        }
    }

    // 获取 MeasureSpec 中的模式部分
    @MeasureSpecMode
    public static int getMode(int measureSpec) {
        // 通过与 MODE_MASK 的按位与操作获取高 2 位，返回测量模式
        return (measureSpec & MODE_MASK);
    }

    // 获取 MeasureSpec 中的大小部分
    public static int getSize(int measureSpec) {
        // 通过与 ~MODE_MASK 的按位与操作清除高 2 位，只保留低 30 位的大小，返回测量大小
        return (measureSpec & ~MODE_MASK);
    }
}

```

### 顶级View过程

> 对于顶级View（DecorView）来说：其MeasureSpec由窗口尺寸和自身LayoutParams共同确定

* 与LayoutParams的关系

在 `ViewRootImpl` 类中，`measureHierarchy` 和 `getRootMeasureSpec` 方法用于测量根视图（即应用的窗口或顶层视图）的尺寸，并为子视图创建相应的 `MeasureSpec`。

**`measureHierarchy` 方法**

```java
private boolean measureHierarchy(final View host, final WindowManager.LayoutParams lp,
                                 final Resources res, final int desiredWindowWidth, final int desiredWindowHeight,
                                 boolean forRootSizeOnly) {
    int childWidthMeasureSpec;
    int childHeightMeasureSpec;

    if (!goodMeasure) {
        // 获取根视图的测量规范
        childWidthMeasureSpec = getRootMeasureSpec(desiredWindowWidth, lp.width,
                                                   lp.privateFlags);
        childHeightMeasureSpec = getRootMeasureSpec(desiredWindowHeight, lp.height,
                                                    lp.privateFlags);

        // 执行测量，传递测量规范给根视图
        performMeasure(childWidthMeasureSpec, childHeightMeasurSpec);
    }
}
```

**`getRootMeasureSpec` 方法**

```java
private static int getRootMeasureSpec(int windowSize, int measurement, int privateFlags) {
    int measureSpec;
    // 判断是否有一个特殊标志，是否存在 "cutout"（凹口区域）扩展布局
    final int rootDimension = (privateFlags & PRIVATE_FLAG_LAYOUT_SIZE_EXTENDED_BY_CUTOUT) != 0
        ? MATCH_PARENT : measurement; // 如果有切割扩展标志，使用 MATCH_PARENT，否则使用原始尺寸

    switch (rootDimension) {
        case ViewGroup.LayoutParams.MATCH_PARENT:
            // MATCH_PARENT：精确模式，根视图的尺寸应与窗口大小一致，不可调整
            measureSpec = MeasureSpec.makeMeasureSpec(windowSize, MeasureSpec.EXACTLY);
            break;
        case ViewGroup.LayoutParams.WRAP_CONTENT:
            // WRAP_CONTENT：最大模式，根视图的尺寸可以调整，最大尺寸为窗口大小
            measureSpec = MeasureSpec.makeMeasureSpec(windowSize, MeasureSpec.AT_MOST);
            break;
        default:
            // 固定大小：精确模式
            measureSpec = MeasureSpec.makeMeasureSpec(rootDimension, MeasureSpec.EXACTLY);
            break;
    }
    return measureSpec;
}
```

### 普通View过程

> 对于普通View来说：其MeasureSpec由父容器和自身LayoutParams共同确定

```java
// ViewGroup.java
protected void measureChildWithMargins(View child,
        int parentWidthMeasureSpec, int widthUsed,
        int parentHeightMeasureSpec, int heightUsed) {
    final MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();

    final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight + lp.leftMargin + lp.rightMargin
                    + widthUsed, lp.width);
    final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom + lp.topMargin + lp.bottomMargin
                    + heightUsed, lp.height);

    child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}
```

对子元素进行 `child.measure`，测量之前先通过`getChildMeasureSpec`方法获取到子元素的`MeasureSpec`。

参数：

* spec – 父容器的`MeasureSpec`

* padding – 父视图实际提供给子视图的有效宽度

* childDimension – 子视图在布局中的宽度属性

```java
public static int getChildMeasureSpec(int spec, int padding, int childDimension) {
    int specMode = MeasureSpec.getMode(spec);  // 解包，获取父视图的测量模式
    int specSize = MeasureSpec.getSize(spec);  // 解包，获取父视图的测量大小

    // 计算子视图可用大小（父容器的尺寸减去父容器已占用的空间大小）
    int size = Math.max(0, specSize - padding);

    // 结果的尺寸和模式
    int resultSize = 0;
    int resultMode = 0;

    // 根据父视图的测量模式进行处理
    switch (specMode) {
            // 1. 父视图强制给子视图一个精确的尺寸（EXACTLY）
        case MeasureSpec.EXACTLY:
            if (childDimension >= 0) {
                // 1.1 如果子视图有指定的尺寸，则使用该尺寸（EXACTLY）
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // 1.2 如果子视图要求 `MATCH_PARENT`，即宽度应与父视图相同
                resultSize = size;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // 1.3 如果子视图要求 `WRAP_CONTENT`，即宽度根据内容来决定，最大不能超出父视图
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

            // 2. 父视图强制给子视图一个最大尺寸（AT_MOST）
        case MeasureSpec.AT_MOST:
            if (childDimension >= 0) {
                // 2.1 如果子视图有指定的尺寸，则直接使用该尺寸（EXACTLY）
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // 2.2 如果子视图要求 `MATCH_PARENT`，即宽度应与父视图相同，但父视图尺寸是最大值
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // 2.2 如果子视图要求 `WRAP_CONTENT`，即宽度根据内容来决定，最大不能超出父视图
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

            // 3. 父视图未指定具体大小，宽度可以自由伸缩（UNSPECIFIED）
        case MeasureSpec.UNSPECIFIED:
            if (childDimension >= 0) {
                // 3.1 如果子视图有指定的尺寸，则直接使用该尺寸（EXACTLY）
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // 3.2 如果子视图要求 `MATCH_PARENT`，即宽度应与父视图相同，但父视图宽度不明确
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // 3.3 如果子视图要求 `WRAP_CONTENT`，即宽度根据内容来决定，最大不能超出父视图
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            }
            break;
    }

    // 返回计算后的子视图测量规格，使用 `MeasureSpec.makeMeasureSpec` 结合结果的尺寸和模式
    return MeasureSpec.makeMeasureSpec(resultSize, resultMode);
}
```

总结：

View固定宽高，MeasureSpec为精确模式

![getChildMeasureSpec](https://raw.githubusercontent.com/betteryuxuan/Image/main/getChildMeasureSpec.png)

# View工作过程

View的工作流程主要是指 `measure`、`layout`、`draw`这三大流程，即测量、布局和绘制

其中 measure 确定View的测量宽/高，layout确定View的最终宽/高和四个顶点的位置，而draw 则将View绘制到屏幕上。

## measure过程

measure 过程要分情况来看

* 如果是一个原始的View，那么通过 measure 方法就完成了其测量过程

* 如果是一个 ViewGroup，除了完成自己的测量过程外，还会遍历去调用所有子元素的 measure 方法，各个子元素再递归去执行这个流程，下面针对这两种情况分别讨论。

### View 的 measure 过程

View的measure方法调用onMeasure

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(),widthMeasureSpec),
                     getDefaultSize(getSuggestedMinimumHeight(),heightMeasureSpec));
}

public static int getDefaultSize(int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec.getMode(measureSpec);
    int specSize = MeasureSpec.getSize(measureSpec);

    switch (specMode) {
        case MeasureSpec.UNSPECIFIED:
            result = size;
            break;
        case MeasureSpec.AT_MOST:
        case MeasureSpec.AT_MOST:
            result = specSize;
            break;
    }
    return result;
}
```

在AT_MOST和AT_MOST情况下：

`getDefaultSize`方法可以理解为返回measureSpec的specSize，这个就是View测量后的大小

在UNSPECIFIED情况下：

`result = size;`：`size`的值为传入的参数，`getSuggestedMinimumWidth()`和`getSuggestedMinimumHeight()`的返回值

> 返回视图应使用的建议最小宽度。这将返回视图的最小宽度和背景的最小宽度的最大值

```java
protected int getSuggestedMinimumWidth() {
    return (mBackground == null) ? mMinWidth : max(mMinWidth, mBackground.getMinimumWidth());
}

protected int getSuggestedMinimumHeight() {
    return (mBackground == null) ? mMinHeight : max(mMinHeight, mBackground.getMinimumHeight());
}

public int getMinimumHeight() {
    final int intrinsicHeight = getIntrinsicHeight();
    return intrinsicHeight > 0 ? intrinsicHeight : 0;
}
```

* **当没有背景（`mBackground == null`）时**，返回视图的最小宽度 `mMinWidth`。

* **当有背景时**，返回视图的最小宽度 `mMinWidth` 和背景的最小宽度之间的较大值，确保视图的宽度至少能容纳背景的最小宽度。

其中`mMinWidth`为指定的`android:minWidth`的值，不指定则默认为0

`mBackground.getMinimumHeight()`返回Drawable原始宽度，无原始宽度则返回0（ShapeDrawable无原始宽高返回0，BitmapDrawable有原始宽高）

### ViewGroup的**measure**过程

对于ViewGroup来说，除了完成自己的`measure`过程以外，还会遍历去调用所有子元素的`measure`方法，各个子元素再递归去执行这个过程。

和View不同，ViewGroup是一个抽象类，因此它没有重写View的`onMeasure`方法，但是它提供了一个`measureChildren`的方法

```java
// ViewGroup.java
/**
* 要求该视图的所有子视图自行测量，同时考虑该视图的 MeasureSpec 要求及其填充。
* 跳过处于 GONE 状态的子项
* 形参：
* widthMeasureSpec – 此视图的宽度要求
* heightMeasureSpec – 该视图的高度要求
*/
protected void measureChildren(int widthMeasureSpec, int heightMeasureSpec) {
    final int size = mChildrenCount;
    final View[] children = mChildren;
    for (int i = 0; i < size; ++i) {
        final View child = children[i];
        if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
        }
    }
}

protected void measureChild(View child, int parentWidthMeasureSpec,
        int parentHeightMeasureSpec) {
    final LayoutParams lp = child.getLayoutParams();

    // 取出子元素的MeasureSpec
    final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
            mPaddingLeft + mPaddingRight, lp.width);
    final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
            mPaddingTop + mPaddingBottom, lp.height);

    // 交给view测量
    child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}
```

### Linearlayout示例

```java
public class LinearLayout extends ViewGroup {
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        if (mOrientation == VERTICAL) {
            measureVertical(widthMeasureSpec, heightMeasureSpec);
        } else {
            measureHorizontal(widthMeasureSpec, heightMeasureSpec);
        }
    }
}
```

```java
void measureVertical(int widthMeasureSpec, int heightMeasureSpec) {
    ...
        // 遍历所有子视图并进行测量
        for (int i = 0; i < count; ++i) {
            final View child = getVirtualChildAt(i);  // 获取当前子视图
            ...
                final LayoutParams lp = (LayoutParams) child.getLayoutParams();  // 获取子视图的布局参数

            // 测量子视图的大小，如果有权重，视图将使用所有可用空间
            final int usedHeight = totalWeight == 0 ? mTotalLength : 0;  // 如果没有权重，则使用已分配的空间
            measureChildBeforeLayout(child, i, widthMeasureSpec, 0,
                                     heightMeasureSpec, usedHeight);  // 测量子视图

            final int childHeight = child.getMeasuredHeight();  // 获取子视图的测量高度
            final int totalLength = mTotalLength;
            mTotalLength = Math.max(totalLength, totalLength + childHeight + lp.topMargin + lp.bottomMargin + getNextLocationOffset(child));
        }
}
```

`mTotalLength`存储`LinearLayout`竖直方向的初步高度，表示累积的所有子视图占用空间的总长度

每测量一个子元素，`mTotalLength`就会增加。

* `totalLength + childHeight + lp.topMargin + lp.bottomMargin + getNextLocationOffset(child)`

增加的包括：子视图的测量高度，子视图的顶部、底部边距，额外偏移量。

```java
void measureVertical(int widthMeasureSpec, int heightMeasureSpec) {
 	...
    // 加上内边距
    mTotalLength += mPaddingTop + mPaddingBottom;

    int heightSize = mTotalLength;

    // 检查是否符合最小高度要求
    heightSize = Math.max(heightSize, getSuggestedMinimumHeight());

    // 根据测量模式和计算的高度，计算最终的高度
    int heightSizeAndState = resolveSizeAndState(heightSize, heightMeasureSpec, 0);
    heightSize = heightSizeAndState & MEASURED_SIZE_MASK;

    ...
        
    // 设置测量后的宽度和高度
    setMeasuredDimension(resolveSizeAndState(maxWidth, widthMeasureSpec, childState),
            heightSizeAndState);
}
```

当`height` 为 `match_parent` 或具体数值

- 像普通的 `View` 一样测量其高度`LinearLayout` 的高度就是 `specSize`（父容器指定的固定值）

当 `LinearLayout` 设置为 `wrap_content` 时

* 测量并汇总所有子视图的高度，同时考虑 `margin` 和父容器的剩余空间限制

```java
public static int resolveSizeAndState(int size, int measureSpec, int childMeasuredState) {
    final int specMode = MeasureSpec.getMode(measureSpec);  // 获取测量模式
    final int specSize = MeasureSpec.getSize(measureSpec);  // 获取期望的尺寸
    final int result;
    
    switch (specMode) {
        case MeasureSpec.AT_MOST:
            // 如果父容器允许子视图的最大尺寸，但不强制要求
            if (specSize < size) {
                // 如果计算出来的尺寸大于父容器允许的最大尺寸，返回最大尺寸并标记状态为太小
                result = specSize | MEASURED_STATE_TOO_SMALL;
            } else {
                // 否则，返回计算出的尺寸
                result = size;
            }
            break;
        case MeasureSpec.EXACTLY:
            // 如果父容器给出了一个确切的尺寸要求
            result = specSize;
            break;
        case MeasureSpec.UNSPECIFIED:
        default:
            // 如果父容器没有指定任何约束，视图可以是任意大小
            result = size;
    }
    
    // 返回最终的尺寸，并将子视图的测量状态合并到结果中
    return result | (childMeasuredState & MEASURED_STATE_MASK);
}

```

## layout过程



```java
public void layout(int l, int t, int r, int b) {
    int oldL = mLeft;
    int oldT = mTop;
    int oldB = mBottom;
    int oldR = mRight;

    // 判断父视图是否使用光学框架（optical frame），如果是则使用光学框架，否则使用普通框架
    boolean changed = isLayoutModeOptical(mParent) ? 
            setOpticalFrame(l, t, r, b) : setFrame(l, t, r, b);

    // 如果位置发生变化或需要布局
    if (changed || (mPrivateFlags & PFLAG_LAYOUT_REQUIRED) == PFLAG_LAYOUT_REQUIRED) {
        // 执行布局操作
        onLayout(changed, l, t, r, b);

        // 清除布局要求标记
        mPrivateFlags &= ~PFLAG_LAYOUT_REQUIRED;

        // 如果有布局变化监听器，通知它们
        ListenerInfo li = mListenerInfo;
        if (li != null && li.mOnLayoutChangeListeners != null) {
            ArrayList<OnLayoutChangeListener> listenersCopy = 
                    (ArrayList<OnLayoutChangeListener>)li.mOnLayoutChangeListeners.clone();
            int numListeners = listenersCopy.size();
            for (int i = 0; i < numListeners; ++i) {
                listenersCopy.get(i).onLayoutChange(this, l, t, r, b, oldL, oldT, oldR, oldB);
            }
        }
    }

    // 清除强制布局标志
    mPrivateFlags &= ~PFLAG_FORCE_LAYOUT;
    // 标记视图已布局
    mPrivateFlags3 |= PFLAG3_IS_LAID_OUT;
}
```

首先会通过setFrame方法来设定View的四个顶点的位置，即初始化mLeft、mRight、mTop和mBottom这四个值，View的四个顶点一旦确定，那么View在父容器中的位置也就确定了；

接着会调用onLayout方法，这个方法的用途是父容器确定子元素的位置，和onMeasure方法类似，onLayout的具体实现同样和具体的布局有关，所以View和ViewGroup均没有真正实现onLayout方法。

```java
// LinearLayout.java
@Override
protected void onLayout(boolean changed, int l, int t, int r, int b) {
    if (mOrientation == VERTICAL) {
        layoutVertical(l, t, r, b);
    } else {
        layoutHorizontal(l, t, r, b);
    }
}

void layoutVertical(int left, int top, int right, int bottom) {
    final int count = getVirtualChildCount();  // 获取子视图的数量

    for (int i = 0; i < count; i++) {
        // 获取第i个子视图
        final View child = getVirtualChildAt(i);
        if (child == null) {
            // 如果子视图为空，则调用measureNullChild进行处理
            childTop += measureNullChild(i);  
        } 
        // 当视图可见时
        else if (child.getVisibility() != GONE) {  
            // 获得子元素的测量宽高
            final int childWidth = child.getMeasuredWidth();
            final int childHeight = child.getMeasuredHeight();

            // 获取子视图的布局参数
            final LinearLayout.LayoutParams lp =
                (LinearLayout.LayoutParams) child.getLayoutParams();  

            // 如果当前子视图前面有分割线,加上分割线的高度
            if (hasDividerBeforeChildAt(i)) {  
                childTop += mDividerHeight;
            }

            // 加上子视图的上边距
            childTop += lp.topMargin;  
            // *计算并设置子视图的四个顶点的位置
            setChildFrame(child, childLeft, childTop + getLocationOffset(child),
                          childWidth, childHeight); 
            // 更新childTop为下一个视图的位置
            childTop += childHeight + lp.bottomMargin + getNextLocationOffset(child);

            // 如果当前子视图有跨度，跳过一定数量的子视图
            i += getChildrenSkipCount(child, i);  
        }
    }
}

// 参数width和height通过child.getMeasuredWidth()获得
private void setChildFrame(View child, int left, int top, int width, int height) {
    child.layout(left, top, left + width, top + height);
}
```

此方法会遍历所有子元素并调用setChildFrame方法来为子元素指定对应的位置，其中childTop会逐渐增大，这就意味着后面的子元素会被放置在靠下的 位 置 ， 这 刚 好 符 合 竖 直 方 向 的 LinearLayout 的 特 性 。 至 于setChildFrame，它仅仅是调用子元素的layout方法而已，这样父元素在layout方法中完成自己的定位以后，就通过onLayout方法去调用子元素的layout方法，子元素又会通过自己的layout方法来确定自己的位置，这 样 一 层 一 层 地 传 递 下 去 就 完 成 了 整 个 View 树 的 layout 过 程 

## draw过程

`performTraversals()` 方法

绘制流程的入口，负责启动整个视图的绘制流程。它会依次调用不同的绘制方法：

```java
private void performTraversals() {
    //开始绘画流程
    performDraw();
}
```

```java
private void performDraw() {
    ...... 
    draw(fullRedrawNeeded);
    ......
}
```

`draw(boolean fullRedrawNeeded)` 方法

这个方法是视图绘制的主要逻辑之一。它通过传递一个 `boolean` 标志（`fullRedrawNeeded`）来判断是否需要重新绘制视图，并在需要时执行绘制操作。最终，会调用 `drawSoftware()` 方法将视图绘制到屏幕上。

```java
private void draw(boolean fullRedrawNeeded) {
    ......
    drawSoftware(surface, mAttachInfo, xOffset, yOffset, scalingRequired, dirty);
    ......
}
```

`drawSoftware()` 方法

这个方法最终调用了 `mView.draw(canvas)`，实际上就是每个 `View` 的绘制操作。`drawSoftware()` 会将绘制内容输出到 `Canvas` 上，`Canvas` 是一个抽象的绘图上下文，负责将所有绘制的内容（比如背景、内容、子视图等）呈现到屏幕上。

```java
private boolean drawSoftware(Surface surface, AttachInfo attachInfo, int xoff, int yoff,
                             boolean scalingRequired, Rect dirty) {
    ......
    mView.draw(canvas);
    ......
}
```

`draw()` 方法详细步骤

1. **绘制背景：** 如果 `dirtyOpaque` 为 `false`，就会调用 `drawBackground()` 绘制视图的背景。
2. **保存 Canvas 层：** 如果需要处理渐变效果或者其他层次的效果，Canvas 会保存其状态。
3. **绘制控件内容：** 通过 `onDraw(canvas)` 绘制控件自己的内容。这是开发者自定义绘制逻辑的地方。
4. **绘制子控件：** `dispatchDraw(canvas)` 会被调用，`ViewGroup` 会在其中绘制它的所有子视图。这里会递归地调用 `drawChild()`，如果子控件是 `ViewGroup`，则会继续绘制子控件。如果是View的话，这个方法是空实现。
5. **绘制渐变边缘：** 如果需要，绘制渐变边缘。
6. **绘制装饰：** 包括滚动条、前景等。
7. **绘制焦点高亮：** 如果控件有焦点，会绘制焦点的高亮显示。

```java
@CallSuper
public void draw(@NonNull Canvas canvas) {
    // 获取当前视图的私有标志，并更新视图为已绘制状态
    final int privateFlags = mPrivateFlags;
    mPrivateFlags = (privateFlags & ~PFLAG_DIRTY_MASK) | PFLAG_DRAWN;

    /* 绘制过程包含多个步骤，必须按适当的顺序执行：
     *
     *      1. 绘制背景
     *      2. 如果需要，保存 Canvas 层以准备渐变效果
     *      3. 绘制视图的内容
     *      4. 绘制子控件
     *      5. 如果需要，绘制渐变边缘并恢复图层
     *      6. 绘制装饰（例如滚动条等）
     *      7. 如果需要，绘制默认焦点高亮
     */

    // Step 1: 绘制背景
    drawBackground(canvas);

    // 如果没有渐变边缘，则跳过步骤2和5（常见的情况）
    final int viewFlags = mViewFlags;
    boolean horizontalEdges = (viewFlags & FADING_EDGE_HORIZONTAL) != 0;
    boolean verticalEdges = (viewFlags & FADING_EDGE_VERTICAL) != 0;
    if (!verticalEdges && !horizontalEdges) {
        // Step 3: 绘制视图的内容
        onDraw(canvas);

        // Step 4: 绘制子控件
        dispatchDraw(canvas);

        // 绘制自动填充的高亮显示（如果有）
        drawAutofilledHighlight(canvas);

        // 如果有覆盖视图（Overlay），则绘制覆盖视图
        if (mOverlay != null && !mOverlay.isEmpty()) {
            mOverlay.getOverlayView().dispatchDraw(canvas);
        }

        // Step 6: 绘制装饰（前景、滚动条等）
        onDrawForeground(canvas);

        // Step 7: 绘制默认焦点高亮
        drawDefaultFocusHighlight(canvas);

        // 如果启用了布局边界调试，绘制焦点调试框
        if (isShowingLayoutBounds()) {
            debugDrawFocus(canvas);
        }

        // 完成绘制
        return;
    }
}
```

# 自定义View

