#  BottomSheet

> `BottomSheet` 是一种位于屏幕底部的面板，用于显示附加内容或选项。提供了从屏幕底部向上滑动显示内容的交互方式。这种设计模式在 Material Design 中被广泛推荐，因为它可以提供一种优雅且不干扰主屏幕内容的方式来展示额外信息或操作。



具体实现主要包含：BottomSheetBeahvior 、BottomSheetDialog、BottomSheetDialogFragment

![m3-bottom-sheet](https://raw.githubusercontent.com/betteryuxuan/Image/main/m3-bottom-sheet.png)



# BottomSheetBehavior

> 用于控制布局（通常是 `CoordinatorLayout` 下的子布局）行为的类，它允许布局像 Bottom Sheet 一样从屏幕底部滑出或隐藏。
>
> 可以通过 `BottomSheetBehavior` 实现一个嵌入式 Bottom Sheet，它是屏幕内容的一部分，而不是弹出式对话框。可以控制 Bottom Sheet 的各种状态，比如展开、折叠、隐藏等。

步骤

1. 导入依赖

   ```java
       implementation 'com.google.android.material:material:1.9.0'
   ```

2. 设置布局文件

   需要在布局文件中定义一个作为 `Bottom Sheet` 的 `View`，通常是 `FrameLayout` 或其他可容纳内容的容器，并确保该容器是在 `CoordinatorLayout` 之下。

   注意要添加`app:layout_behavior="com.google.android.material.bottomsheet.BottomSheetBehavior">`

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.coordinatorlayout.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main_content"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true">


    <androidx.core.widget.NestedScrollView
        android:id="@+id/bottom_sheet"
        android:layout_width="match_parent"
        android:layout_height="200dp"
        android:background="@android:color/holo_orange_light"
        android:clipToPadding="true"
        app:layout_behavior="com.google.android.material.bottomsheet.BottomSheetBehavior">

        <TextView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:padding="16dp"
            android:text="你好"
            android:textSize="16sp" />

    </androidx.core.widget.NestedScrollView>

</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

3. 在代码中获取BottomSheetBehavior

```java
public class MainActivity extends AppCompatActivity {
    
    private BottomSheetBehavior mBottomSheetBehavior;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        View bottomSheet = findViewById(R.id.bottom_sheet);

        mBottomSheetBehavior = BottomSheetBehavior.from(bottomSheet);
        mBottomSheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);
    }
}
```

`BottomSheetBehavior` 有多种状态，你可以通过 `setState()` 方法来控制它的状态：

- `STATE_EXPANDED`：完全展开状态。
- `STATE_COLLAPSED`：折叠状态。
- `STATE_HIDDEN`：隐藏状态（需要启用）。
- `STATE_DRAGGING`：正在拖动（通常是用户手势触发）。
- `STATE_SETTLING`：松手后即将到达某个状态。

4. 设置 BottomSheetBehavior 的回调

   可以通过设置 `BottomSheetCallback` 来监听 `Bottom Sheet` 状态的变化或滑动事件：

   ```java
   bottomSheetBehavior.addBottomSheetCallback(new BottomSheetBehavior.BottomSheetCallback() {
       @Override
       public void onStateChanged(@NonNull View bottomSheet, int newState) {
           switch (newState) {
               case BottomSheetBehavior.STATE_EXPANDED:
                   Toast.makeText(MainActivity.this, "Bottom Sheet Expanded", Toast.LENGTH_SHORT).show();
                   break;
               case BottomSheetBehavior.STATE_COLLAPSED:
                   Toast.makeText(MainActivity.this, "Bottom Sheet Collapsed", Toast.LENGTH_SHORT).show();
                   break;
               case BottomSheetBehavior.STATE_HIDDEN:
                   Toast.makeText(MainActivity.this, "Bottom Sheet Hidden", Toast.LENGTH_SHORT).show();
                   break;
           }
       }
   
       @Override
       public void onSlide(@NonNull View bottomSheet, float slideOffset) {
           // 处理滑动事件
       }
   });
   ```

# BottomSheetDialog

> 一个 `Dialog` 类型的组件，用于创建一个从屏幕底部弹出的对话框。它的行为类似于 `BottomSheetBehavior`，但它独立于屏幕上的其他内容，不嵌入布局。常用于临时对话或操作，不会影响当前界面的 UI。

1. **布局文件**：创建一个你想要在 `BottomSheetDialog` 中显示的布局。

   ```xml
   <LinearLayout
       xmlns:android="http://schemas.android.com/apk/res/android"
       android:layout_width="match_parent"
       android:layout_height="wrap_content"
       android:orientation="vertical">
   
       <!-- 底部对话框的内容 -->
   
   </LinearLayout>
   ```

2. **在 Java 或 Kotlin 中创建 `BottomSheetDialog`**：

   ```java
   BottomSheetDialog bottomSheetDialog = new BottomSheetDialog(this);
   View view = getLayoutInflater().inflate(R.layout.bottom_sheet_layout, null);
   bottomSheetDialog.setContentView(view);
   bottomSheetDialog.show();
   ```

#   BottomSheetDialogFragment

> **BottomSheetDialogFragment**： `BottomSheetDialog` 的子类，继承了 `Fragment` 的生命周期管理特性，因此适合在需要更好管理生命周期的场景中使用。使用 `BottomSheetDialogFragment`可以在 Fragment 中管理 BottomSheet 弹窗，并且支持在旋转屏幕等情况下保持状态。

**步骤**：

1. **创建一个继承 `BottomSheetDialogFragment` 的类**：

   在`fragment`可以进行UI的更新

   ```java
   public class MyBottomSheetDialogFragment extends BottomSheetDialogFragment {
       @Nullable
       @Override
       public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
           return inflater.inflate(R.layout.bottom_sheet_layout, container, false);
       }
   }
   ```

2. **在布局文件中创建 `BottomSheet` 布局**： 与 `BottomSheetDialog` 类似，创建一个普通的布局文件（如 `bottom_sheet_layout.xml`）。

3. **在 Java 中调用 `BottomSheetDialogFragment`**：

   ```java
   MyBottomSheetDialogFragment bottomSheetFragment = new MyBottomSheetDialogFragment();
   bottomSheetFragment.show(getSupportFragmentManager(), bottomSheetFragment.getTag());
   ```

实现效果：![qq_pic_merged_1727012854181](https://raw.githubusercontent.com/betteryuxuan/Image/main/qq_pic_merged_1727012854181.jpg)

# 区别

- **`BottomSheet`**：适用于需要持续显示在页面底部的小控件或内容面板，常用于多任务、底部工具栏等场景。
- **`BottomSheetDialog`**：适合显示简短的、操作性的内容，如菜单选项、确认对话框等。
- **`BottomSheetDialogFragment`**：适合复杂的对话框场景，特别是需要异步加载内容、动态更新或处理复杂用户交互时。



---

---

==***感谢您的阅读
如有错误烦请指正***==

---



> 参考：
>
> 1. [BottomSheet 的使用介绍](https://blog.csdn.net/m0_73986294/article/details/134366315)
> 2. [探索BottomSheet的背后秘密Bottom Sheet 在Android Design Support Libra - 掘金 (juejin.cn)](https://juejin.cn/post/7156874737740677133)