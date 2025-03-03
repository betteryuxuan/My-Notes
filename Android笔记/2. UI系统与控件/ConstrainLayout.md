# 介绍

> 约束布局`ConstraintLayout`是 Android Studio 2.3 起创建布局后的默认布局
>
> 主要是为了解决布局多层嵌套问题，以灵活的方式定位和调整小部件。
>
> ConstraintLayout可以按照比例约束控件位置和尺寸，能够更好地适配屏幕大小不同的机型。

借助 `ConstraintLayout`，您可以创建具有扁平视图层次结构（没有嵌套视图组）的大型复杂布局。它与 `RelativeLayout`类似，所有视图都是根据同级视图与父布局之间的关系进行布局的，但它比 `RelativeLayout` 更灵活，并且更易于与 Android Studio 的布局编辑器一起使用。

布局编辑器的可视化工具中直接提供了 `ConstraintLayout` 的所有功能，因为布局 API 和布局编辑器是专为彼此构建的。可以完全使用 `ConstraintLayout` 通过拖动（而非修改 XML）来构建布局。

# 约束条件

> 定义视图的位置时需要为该视图添加至少一个水平约束条件和一个垂直约束条件。
>
> 每个约束条件都表示与其他视图、父布局或不可见引导线的连接或对齐方式。
>
> 每个约束条件均定义视图沿纵轴或横轴的位置。每个视图的每个轴都必须至少有一个约束条件

![image-20240719152207087](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719152207087.png)

从左上角按钮处拖动到页面后，此时未添加任何约束条件，会在位置 [0,0]（左上角）处绘制该视图。

结果：

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-19-15-26-48-905_com.example.co.jpg" alt="Screenshot_2024-07-19-15-26-48-905_com.example.co" style="zoom:25%;" />

缺少横轴的约束条件，就会在左边界绘制；缺少纵轴的约束条件，就会在上边界绘制。

所以我们要确保都必须至少有一个约束条件。

## 添加方式

拖动每个边中间的圆形约束手柄到另一个视图的边缘、布局的边缘或引导线，即可建立约束

![image-20240719160326643](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719160326643.png)

## 外边距设置

方形框：设置边距

滑动部分：该控件在页面中的比例位置

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719160932807.png" alt="image-20240719160932807" style="zoom: 67%;" />

## 删除方式

1. 按住 Ctrl 键的同时点击某个约束定位点。该约束条件将变为红色，表示可以点击将其删除

2. 在 **Attributes** 窗口的 **Layout** 部分中，点击一个约束定位点

   ![image-20240719161400246](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719161400246.png)

3. 鼠标悬浮在约束线上，右键菜单栏中删除

举例：

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719153535993.png" alt="image-20240719153535993" style="zoom: 50%;" />

对于这样的四个按钮，`button1,3`都设置了约束，但是`button2`没有垂直约束，因此显示在顶端

<img src="C:/Users/HP/AppData/Roaming/Typora/typora-user-images/image-20240719153858022.png" alt="image-20240719153858022" style="zoom: 67%;" />

# 添加约束条件

## 父级位置

将视图的一侧约束到布局的相应边缘

![image-20240719170642461](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719170642461.png)

## 对齐方式

将一个视图的边缘与另一视图的同一边对齐。

可以水平对齐，也可以偏移对齐

![image-20240719165702768](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719165702768.png)

## 基线对齐

将一个视图的文本基线与另一视图的文本基线对齐。

方式：

右键该控件，选择`Baseline`，点击文本基线并将其拖动到另一条基线上。

![image-20240719163113293](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719163113293.png)

## 引导线约束

添加垂直或水平引导线约束视图，并且对应用用户不可见。

可以根据 dp 单位或相对于布局边缘的百分比，在布局中放置引导线。

![image-20240719163227054](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719163227054.png)

利用引导线令两个按钮中心对称

![image-20240719172558925](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719172558925.png)

## 屏障约束

用于在动态布局中对齐视图。

屏障可以自动调整其大小以适应其所包含视图的尺寸，常用于当你需要对齐一组视图的边缘（如顶部、底部、开始或结束边缘）时。

1. 点击工具栏中的 `Guidelines`，然后点击 `Add Vertical Barrier` 或 `Add Horizontal Barrier`。
2. 在 Component Tree 窗口中，选择要放入屏障内的视图，并将其拖动到屏障组件中。

举例：

使用了`Horizontal Barrier`，并且将这两个按钮加入该组。屏障始终保持两个控件位置最高的地方，实现将一组视图的顶边或底边对齐到最高或最低的视图边缘。

![image-20240719174706532](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719174706532.png)

![image-20240719173952716](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719173952716.png)

并且可以在这里进行修改对齐的方向

![image-20240719175049886](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719175049886.png)

# 约束偏差

> 约束偏差（Constraint Bias）是ConstraintLayout中的一个概念，用于在视图被约束在两个边界（如父布局的左右边界）之间时，决定其在这些边界之间的位置。通过设置约束偏差，可以控制视图在两个约束之间的位置偏向。

更改高度和宽度的计算方式。这些符号代表尺码模式，如下所示。点击该符号即可在这些设置之间切换：

1. **Fixed**（固定大小）

- 通过在文本框中指定特定尺寸，或者在编辑器中直接调整视图大小来设置固定的宽度和高度。
- 这种模式下，视图的大小是硬编码的，不会根据内容或屏幕尺寸变化。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719191955594.png" alt="image-20240719191955594" style="zoom: 67%;" />

2. **Wrap Content**（内容包裹）

- 视图会根据其内容的大小来确定自身的宽度和高度。
- 如果视图的内容变化，视图的尺寸也会相应变化。
- 默认情况下，设置为`WRAP_CONTENT`的widget不受约束条件的限制。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719192019252.png" alt="image-20240719192019252" style="zoom:67%;" />

3. **Match Constraints**（匹配约束）

- 视图会尽可能扩展以满足每侧的约束条件，同时考虑外边距。
- 在这种模式下，可以使用一些额外的属性来调整视图的最小和最大尺寸。
- **注意**：在ConstraintLayout中不能使用`match_parent`，应改用`match constraints`（0dp）。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719192008186.png" alt="image-20240719192008186" style="zoom:67%;" />

# 使用链控制线性组

> 在Android的ConstraintLayout中，链（Chains）是一个强大的特性，它允许你以灵活的方式组织视图，使其能够水平或垂直地相互关联。

* 创建链的步骤

1. **选择视图**：首先，你需要选择想要包含在链中的所有视图。这些视图可以是你布局中的任何控件，如按钮、文本框等。
2. **设置约束**：确保每个视图都至少与链中的另一个视图有水平或垂直的约束。对于水平链，这通常意味着视图的左侧或右侧与链中的另一个视图的右侧或左侧相连。对于垂直链，则是上方或下方。
3. **定义链的样式**：链的样式可以通过XML属性或直接在布局编辑器中设置。例如，在XML中，你可以通过为链的第一个视图设置`app:layout_constraintHorizontal_chainStyle`（水平链）或`app:layout_constraintVertical_chainStyle`（垂直链）来定义链的样式。在布局编辑器中，你可以通过选择链中的任何视图并使用工具栏上的链按钮来更改链的样式。
4. 调整权重：如果你想要链中的某些视图比其他视图占据更多空间，你可以使用`layout_constraintHorizontal_weight`和`layout_constraintVertical_weight`属性为它们分配权重。这与LinearLayout中的`layout_weight`属性类似。

![image-20240720092912572](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720092912572.png)

![image-20240720101030107](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720101030107.png)

* 链的样式

  ![image-20240720093336585](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720093336585.png)

1. **Spread**：均分剩余空间

![image-20240720093503662](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720093503662.png)

2. **Spread inside**：第一个和最后一个视图固定在链两端的约束条件上，其余视图均匀分布。

![image-20240720093803831](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720093803831.png)

3. **packed**：所有视图的边界内紧密排列视图

![image-20240720094224272](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720094224272.png)

4. **Weighted**：当链设置为 **spread** 或 **spread inside** 时，可以通过将一个或多个视图设置为`match constraints` (`0dp`) 来填充剩余空间。

   使用 `layout_constraintHorizontal_weight` 和 `layout_constraintVertical_weight` 属性为每个视图分配重要性权重。

```java
app:layout_constraintHorizontal_weight="1"
```

* 链的对齐

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240720100354369.png" alt="image-20240720100354369" style="zoom:80%;" />

注意：

1. 视图可以同时属于水平链和垂直链
2. 只有当链的每一端都被约束到同一轴上的另一个对象时，链才能正常工作
3. 尽管链的方向是垂直或水平的，但使用其中一个方向不会沿该方向对齐视图。为链中的每个视图实现适当的位置，还添加其他约束条件，

---

---

==***感谢您的阅读
如有错误烦请指正***==

---

>  参考：
>
> 1. [使用 ConstraintLayout 构建自适应界面  | Views  | Android Developers](https://developer.android.com/develop/ui/views/layout/constraint-layout?hl=zh-cn#add-a-constraint)
> 2. [【约束布局】ConstraintLayout 约束布局 ( 简介 | 引入依赖 | 基本操作 | 垂直定位约束 | 角度定位约束 | 基线约束 )-CSDN博客](https://3mw.cn/0fpue)
> 3. [基础布局之ConstraintLayout约束布局-CSDN博客](https://blog.csdn.net/qq_45649553/article/details/137204824)
