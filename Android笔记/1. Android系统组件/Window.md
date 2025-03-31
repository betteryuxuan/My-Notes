

# Window

> ```java
> public interface WindowManager extends ViewManager 
> ```
>
> **WindowManager 接口**：继承自 ViewManager，扩展了窗口管理相关的功能

> ```java
> package android.view;
> 
> public interface ViewManager {
>     public void addView(View view, ViewGroup.LayoutParams params);
>     public void updateViewLayout(View view, ViewGroup.LayoutParams params);
>     public void removeView(View view);
> }
> ```
>
> **ViewManager 接口**：声明了三个基本操作 —— 添加、更新和移除 View，为视图管理器提供了统一的操作规范。

> 三大操作具体实现：
>
> ```java
> public final class WindowManagerImpl implements WindowManager {
>     private final WindowManagerGlobal mGlobal = WindowManagerGlobal.getInstance();
> 
>     @Override
>     public void addView(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
>         applyTokens(params);
>         mGlobal.addView(view, params, mContext.getDisplayNoVerify(), mParentWindow,
>                         mContext.getUserId());
>     }
> 
>     @Override
>     public void updateViewLayout(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
>         applyTokens(params);
>         mGlobal.updateViewLayout(view, params);
>     }
> 
>     @Override
>     public void removeView(View view) {
>         mGlobal.removeView(view, false);
>     }
> }
> ```
>
> **WindowManagerImpl**：这是 `WindowManager`接口的一个最终实现类，在项目中通过实现 WindowManager 接口，提供具体的窗口管理操作。使用它的三个方法（`addView`、`updateViewLayout`、`removeView`）



> ```java
> public final class WindowManagerGlobal
> ```
>
>  桥接模式，WindowManagerImpl把接口实现的具体操作交给WindowManagerGlobal的单例进行操作。



## addView

> **addView**（`WindowManagerGlobal`） 对输入参数进行验证、预处理以及将 view、ViewRootImpl、布局参数登记到内部列表中；它充当了窗口添加过程的入口，负责准备好所有必要的状态信息。
>
> **setView**（`ViewRootImpl`） 实际将 view 绑定到窗口系统，触发布局刷新和最终通过 IPC 向 WindowManagerService 发送窗口创建请求。

```java
public void addView(View view, ViewGroup.LayoutParams params,
        Display display, Window parentWindow, int userId) {
    // 1. 参数合法性检查：view 和 display 不能为空，params 必须是 WindowManager.LayoutParams 类型
    if (view == null) {
        throw new IllegalArgumentException("view must not be null");
    }
    if (display == null) {
        throw new IllegalArgumentException("display must not be null");
    }
    if (!(params instanceof WindowManager.LayoutParams)) {
        throw new IllegalArgumentException("Params must be WindowManager.LayoutParams");
    }

    // 将传入的参数转换为 WindowManager.LayoutParams 类型
    final WindowManager.LayoutParams wparams = (WindowManager.LayoutParams) params;
    
    // ...

    // 2. 创建 ViewRootImpl 并将view 添加到三个集合里
    // panelParentView 用于保存子窗口依附的父窗口 View（如果存在）
    ViewRootImpl root;
    View panelParentView = null;

    // 同步代码块：保证整个添加过程线程安全
    synchronized (mLock) {
        // ...

        if (windowlessSession == null) {
            // 普通情况下，直接创建一个新的 ViewRootImpl
            root = new ViewRootImpl(view.getContext(), display);
        } else {
            root = new ViewRootImpl(view.getContext(), display, windowlessSession, new WindowlessWindowLayout());
        }

        // 设置 view 的布局参数
        view.setLayoutParams(wparams);

        // 将 view、root 和布局参数分别添加到内部维护的列表中，
        // 这三个列表用于统一管理同一进程中所有窗口的信息
        mViews.add(view);
        mRoots.add(root);
        mParams.add(wparams);

        // 3. 通过 ViewRootImpl 的 setView 方法完成窗口添加
        try {
            // setView作用：
            // 将传入的 View 与对应的 WindowManager.LayoutParams 绑定
            // 完成ViewRootImpl的初始化，再调用requestLayout()方法触发整 View树的重新测量、布局和绘制流程
            // 进而通过 IWindowSession 的 addToDisplay（或 addToDisplayAsUser）方法向 WindowManagerService 发送窗口的添加请求
            // 创建实际的窗口
            root.setView(view, wparams, panelParentView, userId);
        } catch (RuntimeException e) {
            final int viewIndex = (index >= 0) ? index : (mViews.size() - 1);
            if (viewIndex >= 0) {
                removeViewLocked(viewIndex, true);
            }
            throw e;
        }
    }
}
```

```java
// 已添加窗口的根视图
private final ArrayList<View> mViews = new ArrayList<View>();
// 每个根视图对应的ViewRootImpl实例
private final ArrayList<ViewRootImpl> mRoots = new ArrayList<ViewRootImpl>();
// 每个窗口的布局参数
private final ArrayList<WindowManager.LayoutParams> mParams = new ArrayList<WindowManager.LayoutParams>();
// 临时存储待移除的根视图
private final ArraySet<View> mDyingViews = new ArraySet<View>();
```



## removeView

> `removeView` 方法在进行参数校验、线程同步和视图查找后，调用 `removeViewLocked` 来处理实际的移除过程。该过程不仅移除了视图，还处理了与输入法相关的通知、视图的父控件解绑以及根据是否需要延迟销毁来决定是否加入待清理列表。

```java
public void removeView(View view, boolean immediate) {
    if (view == null) {
        throw new IllegalArgumentException("view must not be null");
    }

    synchronized (mLock) {
        // 在内部视图集合中查找传入 view 的索引
        int index = findViewLocked(view, true);
        // 根据索引获取当前与 view 关联的视图
        View curView = mRoots.get(index).getView();
        // 调用内部方法执行实际的视图移除操作
        removeViewLocked(index, immediate);
        if (curView == view) {
            return;
        }
        throw new IllegalStateException("Calling with view " + view
                + " but the ViewAncestor is attached to " + curView);
    }
}

private void removeViewLocked(int index, boolean immediate) {
    // 根据索引从 mRoots 集合中获取对应的 ViewRootImpl 实例，该实例管理一个视图的根节点
    ViewRootImpl root = mRoots.get(index);
    // 从 root 中获取对应的视图
    View view = root.getView();

    // 如果 root 不为空，则通知输入法焦点控制器窗口已关闭
    if (root != null) {
        root.getImeFocusController().onWindowDismissed();
    }
    // 调用 die 方法销毁该视图，immediate 决定同步异步
    boolean deferred = root.die(immediate);
    if (view != null) {
        view.assignParent(null);
        // 如果异步，则将该视图添加到 mDyingViews 列表中，以便后续清理
        if (deferred) {
            mDyingViews.add(view);
        }
    }
}

```

> ```java
> die() -> doDie() -> dispatchDetachedFromWindow()
> ```
>
>  如果异步则发送消息`mHandler.sendEmptyMessage(MSG_DIE);`，Handler处理该消息并调用`doDie()`



## updateViewLayout

> 更新`View`的`LayoutParams`和`ViewRootImpl`的`LayoutParams`

```java
public void updateViewLayout(View view, ViewGroup.LayoutParams params) {
    if (view == null) {
        throw new IllegalArgumentException("view must not be null");
    }
    
    if (!(params instanceof WindowManager.LayoutParams)) {
        throw new IllegalArgumentException("Params must be WindowManager.LayoutParams");
    }

    final WindowManager.LayoutParams wparams = (WindowManager.LayoutParams) params;

    // 设置 view 的布局参数，内部会更新 view 的布局属性
    view.setLayoutParams(wparams);

    synchronized (mLock) {
        // 查找该 view 的索引位置
        int index = findViewLocked(view, true);
        // 根据索引从 mRoots 集合中获取对应的 ViewRootImpl 对象
        ViewRootImpl root = mRoots.get(index);
        // 在 mParams 集合中，移除原有的布局参数
        mParams.remove(index);
        // 将新的布局参数添加到相同的位置
        mParams.add(index, wparams);
        // 通知 ViewRootImpl 更新布局参数，scheduleTraversals()进行测量布局重绘，更新Window的视图
        root.setLayoutParams(wparams, false);
    }
}
```

