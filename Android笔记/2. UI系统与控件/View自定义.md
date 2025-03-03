分为三大类，第一种是自定义 View，第二种是自定义 ViewGroup，第三种是自定义组合控
件。其中，自定义 Vew 又分为继承系统控件(比如TextView)和继承 View 两种情况。自定义
ViewGroup 也分为继承 ViewGroup 和继承系统特定的 ViewGroup(比如 RelativeLayout)两种情
况。

下面分别给出每种自定义控件的详细介绍和示例代码，帮助你更直观地理解实现原理。

------

## 1. 自定义 View

自定义 View 分为两种情况：

### 1.1 继承系统控件

例如，我们可以继承系统提供的 **TextView**，在保留原有功能的基础上，增加一些额外的绘制效果，比如在文本后绘制一个红色圆形背景。

```java
public class CustomTextView extends androidx.appcompat.widget.AppCompatTextView {
    
    public CustomTextView(Context context) {
        super(context);
    }
    
    public CustomTextView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    
    public CustomTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    
    @Override
    protected void onDraw(Canvas canvas) {
        // 绘制自定义背景，例如一个红色的圆形背景
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setColor(Color.RED);
        
        // 计算圆心和半径
        int width = getWidth();
        int height = getHeight();
        float radius = Math.min(width, height) / 2f;
        canvas.drawCircle(width / 2f, height / 2f, radius, paint);
        
        // 调用父类方法，绘制文本内容
        super.onDraw(canvas);
    }
}
```

**说明：**

- 继承自系统控件后，我们可以在 `onDraw()` 方法中先绘制自定义效果，再调用 `super.onDraw(canvas)` 绘制文本。
- 这种方式适合对现有控件进行外观扩展，而不用从零开始编写所有逻辑。

### 1.2 继承 View

如果需要从零开始构造一个全新的控件，可以直接继承 `View`。例如，实现一个简单的圆形控件。

```java
public class CircleView extends View {
    private Paint mPaint;
    
    public CircleView(Context context) {
        super(context);
        init();
    }
    
    public CircleView(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }
    
    public CircleView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }
    
    private void init() {
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mPaint.setColor(Color.BLUE);  // 设置圆形颜色
    }
    
    // 重写 onMeasure 确定控件尺寸
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int desiredSize = 200; // 默认大小 200px
        
        int width = resolveSize(desiredSize, widthMeasureSpec);
        int height = resolveSize(desiredSize, heightMeasureSpec);
        
        setMeasuredDimension(width, height);
    }
    
    // 重写 onDraw 绘制圆形
    @Override
    protected void onDraw(Canvas canvas) {
        int width = getWidth();
        int height = getHeight();
        int radius = Math.min(width, height) / 2;
        canvas.drawCircle(width / 2f, height / 2f, radius, mPaint);
    }
}
```

**说明：**

- 通过重写 `onMeasure()` 方法，可以控制控件的默认尺寸和自适应模式。
- 在 `onDraw()` 方法中，使用 `Canvas` 绘制出自定义图形。
- 适用于需要完全自定义绘制和行为的场景。

## 2. 自定义 ViewGroup

自定义 ViewGroup 的核心在于对子控件进行测量和布局，同样分为两种情况：

### 2.1 继承 ViewGroup

例如，实现一个简单的**流式布局（FlowLayout）**，将子控件按行排列，当一行放不下时换行。

```java
public class FlowLayout extends ViewGroup {

    public FlowLayout(Context context) {
        super(context);
    }
    
    public FlowLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    
    public FlowLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    
    // 测量所有子控件，并计算自身需要的宽度和高度
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        int widthSize  = MeasureSpec.getSize(widthMeasureSpec);
        int widthMode  = MeasureSpec.getMode(widthMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        
        int width = 0, height = 0;
        int lineWidth = 0, lineHeight = 0;
        
        int count = getChildCount();
        for (int i = 0; i < count; i++) {
            View child = getChildAt(i);
            // 测量子控件
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
            int childWidth  = child.getMeasuredWidth();
            int childHeight = child.getMeasuredHeight();
            
            // 判断当前行是否可以容纳该子控件
            if (lineWidth + childWidth > widthSize) {
                // 换行前先记录当前行的最大宽度和累计高度
                width = Math.max(width, lineWidth);
                height += lineHeight;
                // 重置行宽和行高
                lineWidth = childWidth;
                lineHeight = childHeight;
            } else {
                // 同一行内，累加宽度，取最高的高度
                lineWidth += childWidth;
                lineHeight = Math.max(lineHeight, childHeight);
            }
            
            // 最后一个子控件处理
            if (i == count - 1) {
                width = Math.max(width, lineWidth);
                height += lineHeight;
            }
        }
        
        // respect EXACTLY 模式
        setMeasuredDimension(
            (widthMode == MeasureSpec.EXACTLY) ? widthSize : width,
            (heightMode == MeasureSpec.EXACTLY) ? heightSize : height
        );
    }
    
    // 布局所有子控件的位置
    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        int width = getWidth();
        int x = 0, y = 0;
        int lineHeight = 0;
        int count = getChildCount();
        
        for (int i = 0; i < count; i++) {
            View child = getChildAt(i);
            int childWidth  = child.getMeasuredWidth();
            int childHeight = child.getMeasuredHeight();
            
            // 如果当前行放不下子控件，则换行
            if (x + childWidth > width) {
                x = 0;
                y += lineHeight;
                lineHeight = childHeight;
            } else {
                lineHeight = Math.max(lineHeight, childHeight);
            }
            
            // 布局子控件
            child.layout(x, y, x + childWidth, y + childHeight);
            x += childWidth;
        }
    }
}
```

**说明：**

- 在 `onMeasure()` 方法中，遍历所有子控件，根据宽度限制来判断是否换行，同时计算最终的宽高。
- 在 `onLayout()` 方法中，按照测量结果确定每个子控件的位置。
- 这种方式适合需要完全自定义布局逻辑的场景。

------

### 2.2 继承系统特定的 ViewGroup

例如，扩展 `LinearLayout`，在其基础上增加额外效果。下面示例在绘制完成后，为布局绘制一个绿色边框。

```java
public class CustomLinearLayout extends LinearLayout {

    public CustomLinearLayout(Context context) {
        super(context);
    }
    
    public CustomLinearLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
    
    public CustomLinearLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }
    
    // 重写 dispatchDraw 方法，在绘制完子控件后增加边框绘制
    @Override
    protected void dispatchDraw(Canvas canvas) {
        super.dispatchDraw(canvas);
        Paint paint = new Paint();
        paint.setColor(Color.GREEN);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(5);
        canvas.drawRect(0, 0, getWidth(), getHeight(), paint);
    }
}
```

**说明：**

- 继承系统已有的 ViewGroup（如 LinearLayout），可直接利用其布局功能，只需针对特定需求进行扩展。
- 示例中，我们在 `dispatchDraw()` 中添加额外绘制，既保留了原有布局特性，又增加了自定义效果。

------

## 3. 自定义组合控件

组合控件通常是通过组合多个已有控件（View 或 ViewGroup）来构建更复杂的 UI。常见做法是继承一个容器（例如 LinearLayout），然后在构造函数中加载一个 XML 布局，并对内部控件进行封装。

### 自定义标题栏控件

```java
public class CustomTitleBar extends LinearLayout {
    private TextView tvTitle;
    private ImageView ivIcon;
    
    public CustomTitleBar(Context context) {
        this(context, null);
    }
    
    public CustomTitleBar(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    
    public CustomTitleBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init(context, attrs);
    }
    
    private void init(Context context, AttributeSet attrs) {
        // 设置为水平排列
        setOrientation(HORIZONTAL);
        // 从 XML 加载布局，并将其添加到当前容器中
        LayoutInflater.from(context).inflate(R.layout.view_title_bar, this, true);
        
        // 初始化内部控件
        tvTitle = findViewById(R.id.tv_title);
        ivIcon = findViewById(R.id.iv_icon);
        
        // 这里可以解析 attrs 自定义属性进行相关设置（例如标题文字、图标等）
    }
    
    // 对外提供设置标题的方法
    public void setTitle(String title) {
        tvTitle.setText(title);
    }
    
    // 对外提供设置图标的方法
    public void setIcon(Drawable drawable) {
        ivIcon.setImageDrawable(drawable);
    }
}
```

#### XML 布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<!-- 使用 <merge> 标签可以避免重复的布局层级 -->
<merge xmlns:android="http://schemas.android.com/apk/res/android">
    <ImageView
        android:id="@+id/iv_icon"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:src="@drawable/ic_launcher" />
    
    <TextView
        android:id="@+id/tv_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="标题"
        android:textSize="18sp"
        android:layout_marginStart="8dp" />
</merge>
```

**说明：**

- 在构造函数中通过 `LayoutInflater` 加载布局，将 XML 中定义的子控件直接添加到组合控件中。
- 通过封装内部控件（如 `TextView`、`ImageView`），对外提供简洁的接口，便于复用和维护。
- 这种方式常用于实现复杂组件，如自定义标题栏、表单控件、搜索框等。

## 小结

- **自定义 View**：
  - **继承系统控件**：适用于在原有功能基础上扩展外观和交互，如自定义按钮、文本框。
  - **继承 View**：适用于需要完全自定义绘制和行为的控件，如图形控件、动画控件。
- **自定义 ViewGroup**：
  - **继承 ViewGroup**：需要自行实现子控件的测量与布局，适合复杂的自定义排列（例如流式布局、瀑布流布局）。
  - **继承系统特定的 ViewGroup**：在系统容器基础上扩展，利用已有布局机制，同时增加额外效果。
- **自定义组合控件**：
  - 通过组合多个已有控件封装出一个功能复合的控件，强调组件复用和接口设计，适合构建业务组件（例如自定义标题栏、登录框）。



