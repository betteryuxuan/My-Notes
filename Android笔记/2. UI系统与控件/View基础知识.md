# View与ViewGroup

**`View`** 是 Android 中的 UI 组件的基类，代表了用户界面上的一个可视元素。所有的 UI 组件（例如 `Button`、`TextView`、`ImageView` 等）都直接或间接继承自 `View` 类。

**`ViewGroup`**是`View`的一个子类，作为一个容器，能够包含多个子View或其他ViewGroup。它定义了子视图的布局规则和排列方式。

- ViewGroup常见子类：`LinearLayout`、`RelativeLayout`、`FrameLayout`、`ConstraintLayout`等，都是`ViewGroup`的子类，提供了不同的布局策略。
- `ViewGroup`是一个容器，可以嵌套子`View`和其他`ViewGroup`

![image-20241027224413737](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241027224413737.png)

# View位置参数

![image-20241027153742783](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241027153742783.png)

- **`getX()` 和 `getY()`**  
  
  - **`getX()` ** ：获得点击事件距离控件左边的距离
  -  **`getY()`** ：获得点击事件距离控件顶边的距离

- **`getRawX()` 和 `getRawY()`**
  
  **`getRawX()`**：返回点击事件在屏幕上的 X 坐标值。
  
  **`getRawY()`**：返回点击事件在屏幕上的 Y 坐标值。
  
- **`getLeft()`、`getTop()`、`getRight()`、`getBottom()`**  
  - `getLeft()`：返回 `View` 左边缘相对于父布局的距离。
  - `getTop()`：返回 `View` 上边缘相对于父布局的距离。
  - `getRight()`：返回 `View` 右边缘相对于父布局的距离。
  - `getBottom()`：返回 `View` 下边缘相对于父布局的距离。
  - 可以用来计算 View 的宽高：
    ```java
    int width = view.getRight() - view.getLeft();
    int height = view.getBottom() - view.getTop();
    ```

# View的滑动

第一种：通过`View`本身提供的`scrollTo/scrollBy`方法来实现滑动

第二种：通过动画给`View`施加平移效果来实现滑动

第三种：通过改变`View`的`LayoutParams`使得`View`重新布局从而实现滑动

## 1. scrollTo与scrollBy

![image-20241027161439784](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241027161439784.png)

* 这两个方法用于View内容的滚动，改变<u>`View`内容</u>的相对位置
* `scrollTo(x,y)`表示移动到一个具体的坐标点， `scrollBy(dx,dy`)则表示移动的增量为 `dx`、`dy`
* 在 ViewGroup 中使用，则是移动其所有的子 View。



- **mScrollX**：记录的是`View`左边缘与`View`内容左边缘在水平方向的距离。

- 视图的水平偏移量。

  当 `mScrollX` 为 **正值** 时，表示`View`的内容向左滚动，内容的左边缘比`View`的左边缘要靠右。

- **mScrollY**：记录的是`View`上边缘与`View`内容上边缘在竖直方向的距离。
  
- 视图的垂直偏移量。
  
  当 `mScrollY` 为 **正值** 时，表示`View`的内容向上滚动，内容的上边缘比`View`的上边缘要更低。
  
  

 `scrollTo(x, y)` ：`mScrollX` 和 `mScrollY` 会被设置为 `(x, y)`，表示视图内容直接滚动到指定的位置。

 `scrollBy(dx, dy)`：会在现有的 `mScrollX` 和 `mScrollY` 基础上增加或减少 `dx` 和 `dy`，表示基于当前的位置进行相对滚动。

> 比如把一个按钮移动到当前位置右下角：
>
> scrollBy(-20, -20);

## 2. 属性动画

通过动画来实现 `View` 的平移滑动效果，是一种较为灵活且常用的方式。这种方法不仅可以控制滑动的距离和方向，还能够调整滑动的速度、加速度等效果，让动画更流畅和自然。

###  ObjectAnimator
`ObjectAnimator` 可以对 `View` 的 `translationX` 和 `translationY` 属性进行动画设置，从而实现水平方向和垂直方向的平移效果。`translationX` 和 `translationY` 是相对于 `View` 的初始位置的偏移量，单位是像素。

```java
// 水平方向平移动画，向右平移100像素，动画时长500毫秒
ObjectAnimator animatorX = ObjectAnimator.ofFloat(view, "translationX", 0f, 100f).setDuration(500);; 
animatorX.start();

// 垂直方向平移动画，向下平移100像素
ObjectAnimator animatorY = ObjectAnimator.ofFloat(view, "translationY", 0f, 100f).setDuration(500); 
animatorY.start();
```

### ViewPropertyAnimator
```java
view.animate()
    .translationX(100f) // 向右平移100像素
    .translationY(100f) // 向下平移100像素
    .setDuration(500)   // 动画时长500毫秒
    .start();
```

## 3. LayoutParams（布局参数）

LayoutParams用于保存一个view的布局参数，我们可以通过改变view的布局参数来改变位置

> 步骤：
>
> 1. 获取当前 View 的 LayoutParams：
>
>     `getLayoutParams()` 方法获取当前 View 的布局参数。
>
> 2. 修改 LayoutParams：
>
>     根据需要改变布局参数的属性，比如位置、宽高等。
>     
> 3. 请求重新布局：
>     
>    修改完布局参数后，调用 `requestLayout()` 方法，重新布局该 View。
>

```java
// 假设这是一个 Button，我们要让它向右滑动
Button myButton = findViewById(R.id.my_button);
ViewGroup.MarginLayoutParams layoutParams = (ViewGroup.MarginLayoutParams) myButton.getLayoutParams();

// 修改左边距，向右滑动
layoutParams.leftMargin += 50; // 每次滑动 50 像素
myButton.setLayoutParams(layoutParams); // 设置新的 LayoutParams
myButton.requestLayout(); // 请求重新布局
```

父控件是 `LinearLayout`，使用`LinearLayout.LayoutParams`

父控件是`RelativeLayout`，使用 `RelativeLayout.LayoutParams`

除了使用布局的 `LayoutParams` 外还可以用 `ViewGroup.MarginLayoutParams` :

## layout方法

实现一个可以随触摸移动的view

```java
public class CustomeView extends View {
    private int lastX; // 上一个 X 坐标
    private int lastY; // 上一个 Y 坐标

    public CustomeView(Context context) {
        super(context);
    }

    public CustomeView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public CustomeView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }

    public CustomeView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        int x = (int) event.getX(); // 获取当前触摸点的 X 坐标
        int y = (int) event.getY(); // 获取当前触摸点的 Y 坐标

        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN: // 触摸开始
                lastX = x; // 记录当前 X 坐标
                lastY = y; // 记录当前 Y 坐标
                break;
            case MotionEvent.ACTION_MOVE: // 触摸移动
                // 计算移动的偏移量
                int offsetX = x - lastX; 
                int offsetY = y - lastY; 
                // 更新视图的位置
                layout(getLeft() + offsetX, getTop() + offsetY, getRight() + offsetX, getBottom() + offsetY);
                break;
        }
        return true; // 返回 true 表示事件已被处理
    }
}
```

## offsetLeftAndRight

也能实现4的效果

```java
case MotionEvent.ACTION_MOVE:
	int offsetX = x - lastX;
	int offsetY = y - lastY;
	offsetLeftAndRight(offsetX);
	offsetTopAndBottom(offsetY);
	break;
```

# View的弹性滑动

## 1. Scroller 类
> `Scroller` 是 Android 提供的一个辅助类，用于实现 View 的平滑滑动。`Scroller` 并不直接负责滑动，而是通过计算出一系列中间值（如位置）来协助 View 实现平滑的滑动效果。

1. **初始化 Scroller**：
   ```java
   private Scroller scroller;
   
   public MyView(Context context) {
       super(context);
       scroller = new Scroller(context);
   }
   ```

2. **调用 startScroll() 方法**：用于开始一个平滑滚动。
   ```java
   public void smoothScrollTo(int destX, int destY) {
       int scrollX = getScrollX();
       int delta = destX - scrollX;
       scroller.startScroll(scrollX, 0, delta, 0, 1000); // 1000ms 内滑动
       invalidate(); // 触发重绘
   }
   ```

3. **重写 `computeScroll()` 方法**：在绘制期间不断调用 `computeScroll()` 来更新 View 的位置。
   ```java
   @Override
   public void computeScroll() {
       if (scroller.computeScrollOffset()) { // 计算滑动偏移量
           scrollTo(scroller.getCurrX(), scroller.getCurrY());
           postInvalidate(); // 再次触发重绘
       }
   }
   ```

4. 在`activity`或者`fragment`调用

```java
myView.smoothScrollTo(100, 100); // 平滑移动到 (100, 100) 的位置
```

## 2. 动画（ObjectAnimator）

`ObjectAnimator` 可以直接对 View 的属性进行动画操作，是另一种实现弹性滑动效果的方式。

示例代码：
```java
ObjectAnimator animator = ObjectAnimator.ofFloat(view, "translationX", 0f, 100f);
animator.setDuration(1000); // 设置动画持续时间
animator.start();
```

## 3. 延时
有时需要延迟滑动以增强用户体验，比如在手指抬起后稍作延迟再开始滑动。使用 `Handler` 或 `postDelayed()` 方法来实现延时：

```java
view.postDelayed(new Runnable() {
    @Override
    public void run() {
        smoothScrollTo(100, 0); 
    }
}, 300);
```

---

---

==***感谢您的阅读
如有错误烦请指正***==



> 参考：
>
> 1. 《Android开发艺术探索》
> 2. 《Android进阶之光》
