

# 场景

![1731560214120](https://raw.githubusercontent.com/betteryuxuan/Image/main/1731560214120.jpg)

**外部滑动方向和内部不同**：

左右滑动让父容器的View拦截点击事件来处理，上下滑动让内部的View拦截点击事件来处理

可以根据滑动的距离差，滑动速度或者角度来进行判断

# 解决套路

## 外部拦截法

> **重写父容器的事件拦截方法**

如果此时想要横向滑动，父容器拦截该事件，在onTouchEvent方法处理;想要竖向滑动，父容器就不拦截，交给子元素处理

```java
public boolean onInterceptTouchEvent(MotionEvent event) {
    boolean intercepted = false;
    int x = (int) event.getX();
    int y = (int) event.getY();
    switch (event.getAction()) {
        case MotionEvent.ACTION_DOWN: {
            intercepted = false;
            break;
        }
        case MotionEvent.ACTION_MOVE: {
            if (父容器需要此类点击事件) {
                intercepted = true;
            } else {
                intercepted = false;
            }
            break;
        }
        case MotionEvent.ACTION_UP: {
            intercepted = false;
            break;
        }
        default:
            break;
    }
    mLastXIntercept = x;
    mLastYIntercept = y;
    return intercepted;
}
```

> 注意：
>
> 1. down不要拦截，一旦拦截后续都会交给父容器处理，那样如果不符合就无法交给子元素处理了

## 内部拦截法

> **重写子容器的事件分发方法和父容器的事件拦截方法**

父容器不再拦截任何事件，都交给子元素处理。那么在子元素的`dispatchTouchEvent`方法中，如何当前想要竖向滑动就直接讲此事件消耗掉。如果是横向滑动，就调用`requestDisallowInterceptTouchEvent`方法，

下面是子元素的`dispatchTouchEvent`方法

```java

public boolean dispatchTouchEvent(MotionEvent event) {
    int x = (int) event.getX();
    int y = (int) event.getY();

    switch (event.getAction()) {
        case MotionEvent.ACTION_DOWN: {
            // 请求父控件不拦截触摸事件
            parent.requestDisallowInterceptTouchEvent(true);
            break;
        }
        case MotionEvent.ACTION_MOVE: {
            int deltaX = x - mLastX;
            int deltaY = y - mLastY;
            if (父容器需要此类点击事件) {
            // 父控件可以拦截触摸事件
                parent.requestDisallowInterceptTouchEvent(false);
            }
            break;
        }
        case MotionEvent.ACTION_UP: {
            break;
        }
        default:
            break;
    }

    mLastX = x;
    mLastY = y;
    return super.dispatchTouchEvent(event);
}
```

同时父View需要重写onInterceptTouchEvent方法：

```java
public boolean onInterceptTouchEvent(MotionEvent event) {
    int action = event.getAction(); 
    if (action == MotionEvent.ACTION_DOWN) {
        return false;
    } else {
        return true;
    }
}
```

> 注意：**为何ACTION_DOWN要返回false？**

```java
@Override
public void requestDisallowInterceptTouchEvent(boolean disallowIntercept) {

    if (disallowIntercept == ((mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0)) {
        return;
    }

    // 传入true,设置该标志，否则清楚该标志
    if (disallowIntercept) {
        mGroupFlags |= FLAG_DISALLOW_INTERCEPT;
    } else {
        mGroupFlags &= ~FLAG_DISALLOW_INTERCEPT;
    }

    // 然后调用父视图的该方法
    if (mParent != null) {
        mParent.requestDisallowInterceptTouchEvent(disallowIntercept);
    }
}
```

在ViewGroup的dispatchTouchEvent方法中

```java
if (actionMasked == MotionEvent.ACTION_DOWN
    || mFirstTouchTarget != null) {
    // requestDisallowInterceptTouchEvent设置的true，导致这里的disallowIntercept为false
    final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
    if (!disallowIntercept) {
        // 本来设置的true，不让父容器拦截，但这里却进入了
        intercepted = onInterceptTouchEvent(ev);
        // 所以我们重写onInterceptTouchEvent方法让他返回false，其他为false
        ev.setAction(action);
    } else {
        intercepted = false;
    }
}
```







# 示例

HorizontalScrollViewEx父容器，ListViewEx子元素

## 外部拦截

```java
public class HorizontalScrollViewEx extends ViewGroup {
    private static final String TAG = "HorizontalScrollViewEx";

    private int mChildrenSize;  // 记录子元素的数量
    private int mChildWidth;  // 记录每个子元素的宽度
    private int mChildIndex;  // 记录当前显示的子元素索引

    // 分别记录上次触摸事件的坐标
    private int mLastX = 0;
    private int mLastY = 0;

    // 分别记录拦截触摸事件时上次触摸事件的坐标
    private int mLastXIntercept = 0;
    private int mLastYIntercept = 0;

    private Scroller mScroller;  // 用于实现平滑滚动
    private VelocityTracker mVelocityTracker;  // 用于跟踪触摸事件的速度

    private void init() {
        mScroller = new Scroller(getContext());  // 初始化Scroller对象，用于平滑滚动
        mVelocityTracker = VelocityTracker.obtain();  // 初始化VelocityTracker对象，用于跟踪触摸速度
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        boolean intercepted = false;  // 是否拦截事件的标志
        int x = (int) event.getX();  // 获取当前触摸点的X坐标
        int y = (int) event.getY();  // 获取当前触摸点的Y坐标

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: {
                intercepted = false;
                // 如果Scroller未完成滚动，则取消滚动并拦截事件
                if (!mScroller.isFinished()) {
                    mScroller.abortAnimation();
                    intercepted = true;
                }
                break;
            }
            case MotionEvent.ACTION_MOVE: {
                int deltaX = x - mLastXIntercept;  // 计算X轴的滑动距离
                int deltaY = y - mLastYIntercept;  // 计算Y轴的滑动距离
                // 如果X轴滑动距离大于Y轴，表示是横向滑动，拦截事件
                if (Math.abs(deltaX) > Math.abs(deltaY)) {
                    intercepted = true;
                } else {
                    intercepted = false;
                }
                break;
            }
            case MotionEvent.ACTION_UP: {
                intercepted = false;  // 不拦截UP事件
                break;
            }
            default:
                break;
        }

        // 打印拦截结果
        Log.d(TAG, "intercepted=" + intercepted);

        // 更新上次触摸的坐标
        mLastX = x;
        mLastY = y;
        mLastXIntercept = x;
        mLastYIntercept = y;

        return intercepted;  // 返回是否拦截事件
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        mVelocityTracker.addMovement(event);  // 记录触摸事件，计算速度
        int x = (int) event.getX();  // 获取当前触摸点的X坐标
        int y = (int) event.getY();  // 获取当前触摸点的Y坐标

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: {
                // 如果滚动没有完成，终止滚动
                if (!mScroller.isFinished()) {
                    mScroller.abortAnimation();
                }
                break;
            }
            case MotionEvent.ACTION_MOVE: {
                int deltaX = x - mLastX;  // 计算X轴的滑动距离
                int deltaY = y - mLastY;  // 计算Y轴的滑动距离
                // 根据滑动的X轴距离，进行水平滚动
                scrollBy(-deltaX, 0);  
                break;
            }
            case MotionEvent.ACTION_UP: {
                // 计算当前滚动的X坐标
                int scrollX = getScrollX();
                // 根据滚动的X坐标计算当前显示的子元素索引
                int scrollToChildIndex = scrollX / mChildWidth;

                // 计算当前滑动的速度
                mVelocityTracker.computeCurrentVelocity(1000);
                float xVelocity = mVelocityTracker.getXVelocity();

                // 如果速度大于一定阈值（50），则按速度方向滚动到下一个子元素
                if (Math.abs(xVelocity) >= 50) {
                    mChildIndex = xVelocity > 0 ? mChildIndex - 1 : mChildIndex + 1;
                } else {
                    // 否则根据滚动位置决定滚动到哪个子元素
                    mChildIndex = (scrollX + mChildWidth / 2) / mChildWidth;
                }

                // 限制子元素的索引范围
                mChildIndex = Math.max(0, Math.min(mChildIndex, mChildrenSize - 1));

                // 计算目标子元素位置的X坐标
                int dx = mChildIndex * mChildWidth - scrollX;
                // 执行平滑滚动到目标子元素
                smoothScrollBy(dx, 0);

                // 清空VelocityTracker
                mVelocityTracker.clear();
                break;
            }
            default:
                break;
        }

        // 更新上次触摸的坐标
        mLastX = x;
        mLastY = y;
        return true;  // 返回true表示消费了触摸事件
    }

    // 平滑滚动方法，使用Scroller来进行滚动
    private void smoothScrollBy(int dx, int dy) {
        mScroller.startScroll(getScrollX(), 0, dx, 0, 500);  // 启动滚动，滚动持续500ms
        invalidate();  // 请求重新绘制视图
    }

    @Override
    public void computeScroll() {
        // 计算Scroller的滚动偏移量
        if (mScroller.computeScrollOffset()) {
            // 更新视图位置
            scrollTo(mScroller.getCurrX(), mScroller.getCurrY());
            // 请求重新绘制
            postInvalidate();
        }
    }
}

```

主要在这里，根据x，y滑动距离判断当前的意图，如果是要横向滑动就调用`onTouchEvent`方法，进行滑动逻辑

```java
case MotionEvent.ACTION_MOVE: {
    int deltaX = x - mLastXIntercept;  // 计算X轴的滑动距离
    int deltaY = y - mLastYIntercept;  // 计算Y轴的滑动距离
    // 如果X轴滑动距离大于Y轴，表示是横向滑动，拦截事件
    if (Math.abs(deltaX) > Math.abs(deltaY)) {
        intercepted = true;
    } else {
        intercepted = false;
    }
    break;
}
```

## 内部拦截

```java
public class ListViewEx extends ListView {
    private static final String TAG = "ListViewEx";
    private HorizontalScrollViewEx2 mHorizontalScrollViewEx2;  // 自定义的水平滚动视图
    // 分别记录上次触摸的坐标
    private int mLastX = 0;
    private int mLastY = 0;

    // 重写 dispatchTouchEvent 方法来处理触摸事件
    @Override
    public boolean dispatchTouchEvent(MotionEvent event) {
        int x = (int) event.getX();  // 获取当前触摸点的X坐标
        int y = (int) event.getY();  // 获取当前触摸点的Y坐标

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: {
                // 在按下事件时，告诉 HorizontalScrollViewEx2 不要拦截触摸事件
                mHorizontalScrollViewEx2.requestDisallowInterceptTouchEvent(true);
                break;
            }
            case MotionEvent.ACTION_MOVE: {
                // 计算X轴和Y轴的滑动距离
                int deltaX = x - mLastX;
                int deltaY = y - mLastY;
                // 如果滑动的X轴距离大于Y轴，说明是水平滑动，通知 HorizontalScrollViewEx2 不要拦截事件
                if (Math.abs(deltaX) > Math.abs(deltaY)) {
                    mHorizontalScrollViewEx2.requestDisallowInterceptTouchEvent(false);
                }
                break;
            }
            case MotionEvent.ACTION_UP: {
                break;
            }
            default:
                break;
        }

        // 更新上次触摸的坐标
        mLastX = x;
        mLastY = y;

        // 调用父类的 dispatchTouchEvent 方法继续传递触摸事件
        return super.dispatchTouchEvent(event);
    }
}
```
整体框架还是和上面内部拦截法给出的一样，在ACTION_MOVE处理水平增量和竖直增量，水平滑动就交给父容器处理，父容器调用onTouchEvent处理。竖直滑动就调用自己的onTouchEvent处理

```java
public boolean onInterceptTouchEvent(MotionEvent event) {
    int x = (int) event.getX();  // 获取当前触摸事件的X坐标
    int y = (int) event.getY();  // 获取当前触摸事件的Y坐标
    int action = event.getAction();  

    if (action == MotionEvent.ACTION_DOWN) {
        mLastX = x; 
        mLastY = y; 

        // 如果Scroller的动画尚未完成（例如，滑动过程中有动画），则终止动画，并拦截当前触摸事件
        if (!mScroller.isFinished()) {
            mScroller.abortAnimation();  // 终止Scroller的滚动动画
            return true;  // 返回true表示拦截触摸事件，不传递给其他视图
        }

        // 如果Scroller动画已完成，不拦截当前触摸事件，返回false
        return false;
    } else {
        // 对于其他事件（如ACTION_MOVE, ACTION_UP），直接返回true表示拦截触摸事件
        return true;
    }
}
```



# ViewPager处理

ViewPager内部处理了滑动冲突

我们看看的`onInterceptTouchEvent`方法源码，ViewPager主要关心横向界面的切换，如果当前意图是横向切换，就响应用户操作并拦截。采用的是外部拦截法

```java
@Override
public boolean onInterceptTouchEvent(MotionEvent ev) {
    // 获取当前触摸事件的动作类型
    final int action = ev.getAction() & MotionEvent.ACTION_MASK;

    // 触摸取消或触摸结束不拦截该事件
    if (action == MotionEvent.ACTION_CANCEL || action == MotionEvent.ACTION_UP) {
        if (DEBUG) Log.v(TAG, "Intercept done!");
        resetTouch();  // 重置触摸状态
        return false; 
    }

    // 如果已经确定是否正在拖动，且当前不是按下事件（ACTION_DOWN），无需进一步处理。
    if (action != MotionEvent.ACTION_DOWN) {
        if (mIsBeingDragged) {  // 如果正在拖动，拦截
            if (DEBUG) Log.v(TAG, "Intercept returning true!");
            return true;
        }
         // 如果设置为不允许拖动，就不拦截事件
        if (mIsUnableToDrag) { 
            if (DEBUG) Log.v(TAG, "Intercept returning false!");
            return false;
        }
    }

    switch (action) {
        case MotionEvent.ACTION_MOVE: {
            /*
             * 在进入此部分时，mIsBeingDragged 必定为 false，说明尚未开始拖动。
             * 我们检查用户是否已经从原始按下位置移动了足够的距离，决定是否开始拖动。
             */

            final int activePointerId = mActivePointerId;
            if (activePointerId == INVALID_POINTER) { 
                break;
            }

            // 获取当前触摸点的索引
            final int pointerIndex = ev.findPointerIndex(activePointerId);
            // 获取当前触摸点的 x 和 y 坐标
            final float x = ev.getX(pointerIndex);
            final float dx = x - mLastMotionX;  // 水平位移
            final float xDiff = Math.abs(dx);  // 计算水平移动的绝对值
            final float y = ev.getY(pointerIndex);
            final float yDiff = Math.abs(y - mInitialMotionY);  // 垂直方向的移动差值

            if (DEBUG) Log.v(TAG, "Moved x to " + x + "," + y + " diff=" + xDiff + "," + yDiff);

            // dx 不为零，不在两个页面之间的间隙区域，页面可以滑动，则不拦截事件，让嵌套的滚动视图处理
            if (dx != 0 && !isGutterDrag(mLastMotionX, dx) && canScroll(this, false, (int) dx, (int) x, (int) y)) {
                mLastMotionX = x;  // 更新最后触摸位置
                mLastMotionY = y;
                mIsUnableToDrag = true;  // 设置为无法拖动，表示事件不会被vp拦截。
                return false;  // 返回 false，交给嵌套视图处理。
            }

            // 如果水平方向的移动差值超过触摸滑动阈值，并且斜率小于0.5，表示在水平方向上的拖动
            if (xDiff > mTouchSlop && xDiff * 0.5f > yDiff) {
                if (DEBUG) Log.v(TAG, "Starting drag!");
                mIsBeingDragged = true;  // 开始拖动
                requestParentDisallowInterceptTouchEvent(true);  // 请求父视图不要拦截触摸事件
                setScrollState(SCROLL_STATE_DRAGGING);  // 设置滚动状态为拖动中
                mLastMotionX = dx > 0 ? mInitialMotionX + mTouchSlop : mInitialMotionX - mTouchSlop;  // 更新最后触摸坐标
                mLastMotionY = y;
                setScrollingCacheEnabled(true);  // 启用滚动缓存
            } else if (yDiff > mTouchSlop) {  // 如果垂直方向的滑动超过阈值，标记为无法拖动
                if (DEBUG) Log.v(TAG, "Starting unable to drag!");
                mIsUnableToDrag = true;
            }

            // 如果已经开始拖动，则调用 performDrag 来处理实际的拖动
            if (mIsBeingDragged) {
                if (performDrag(x)) {  // 执行拖动操作
                    ViewCompat.postInvalidateOnAnimation(this);  // 请求重绘
                }
            }
            break;
        }

        case MotionEvent.ACTION_DOWN: {
            /*
             * 记录按下时的触摸位置。
             * ACTION_DOWN 总是指第一个触摸点（指针索引 0）。
             */
            mLastMotionX = mInitialMotionX = ev.getX();  // 记录按下时的 x 坐标
            mLastMotionY = mInitialMotionY = ev.getY();  // 记录按下时的 y 坐标
            mActivePointerId = ev.getPointerId(0);  // 获取当前触摸点的指针 ID
            mIsUnableToDrag = false;  // 默认可以拖动

            mIsScrollStarted = true;  // 表示滑动已经开始
            mScroller.computeScrollOffset();  // 计算滚动偏移
            if (mScrollState == SCROLL_STATE_SETTLING && Math.abs(mScroller.getFinalX() - mScroller.getCurrX()) > mCloseEnough) {
                // 如果滚动状态是结算中且偏移量足够大，允许用户捕捉滑动。
                mScroller.abortAnimation();  // 终止动画
                mPopulatePending = false;  
                populate(); 
                mIsBeingDragged = true;  // 开始拖动
                requestParentDisallowInterceptTouchEvent(true);  // 请求父视图不要拦截触摸事件
                setScrollState(SCROLL_STATE_DRAGGING);  // 设置滚动状态为拖动中
            } else {
                completeScroll(false); 
                mIsBeingDragged = false;
            }

            if (DEBUG) {
                Log.v(TAG, "Down at " + mLastMotionX + "," + mLastMotionY
                      + " mIsBeingDragged=" + mIsBeingDragged
                      + "mIsUnableToDrag=" + mIsUnableToDrag);
            }
            break;
        }

        case MotionEvent.ACTION_POINTER_UP:
            onSecondaryPointerUp(ev);  
            break;
    }

    // 初始化或更新 VelocityTracker，用于记录滑动速度
    if (mVelocityTracker == null) {
        mVelocityTracker = VelocityTracker.obtain();  
    }
    mVelocityTracker.addMovement(ev); 

    /*
     * 只有在拖动模式下，我们才会拦截触摸事件。
     */
    return mIsBeingDragged;  // 如果正在拖动，返回 true，表示拦截事件；否则返回 false。
}
```









> 参考：
>
> 1. [一文解决Android View滑动冲突_android 横向竖向滑动冲突-CSDN博客](https://blog.csdn.net/u010302764/article/details/72778732?ops_request_misc=%7B%22request%5Fid%22%3A%220E83387B-047E-4E52-BF03-BD80D1B1ABD0%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=0E83387B-047E-4E52-BF03-BD80D1B1ABD0&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-72778732-null-null.142^v100^pc_search_result_base4&utm_term=滑动冲突&spm=1018.2226.3001.4187)
> 2. 《开发艺术探索》