# 介绍

> ViewBinding 是 Android 开发中的一个功能，它简化了访问视图的过程，避免了使用 `findViewById` 的繁琐步骤。它通过生成与布局文件相对应的绑定类，使得我们能够以类型安全的方式访问布局中的视图。

# 作用

> 视图绑定功能可让您更轻松地编写与视图交互的代码。在模块中启用视图绑定后，它会为该模块中显示的每个 XML 布局文件生成一个绑定类。绑定类的实例包含对在相应布局中具有 ID 的所有视图的直接引用。
>
> 在大多数情况下，视图绑定会替代 `findViewById`。

# 用法

## 开启`ViewBinding`功能

在`bulid.gradle.kts`中启用

> 不需要包含任何额外的库来启用视图绑定。从 Android Studio 3.6 中附带的版本开始，它内置于 Android Gradle 插件中。要启用视图绑定，需要在模块级 `build.gradle` 文件中配置 `viewBinding` 。

![image-20240718211841342](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240718211841342.png)

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240718211854902.png" alt="image-20240718211854902" style="zoom: 67%;" />

```kotlin
 buildFeatures{
        viewBinding = true
    }
```

完成后点击`sync now`同步

## 自动生成绑定类

绑定类会在编译时自动生成，位于 `build/generated/data_binding_base_class_source_out` 目录下。

绑定类包含了与 `activity_main.xml` 布局文件中定义的所有视图的绑定引用

![image-20240718221829984](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240718221829984.png)

**绑定类命名规则**：

> **去掉下划线并将每个单词首字母大写（PascalCase）**：
>
> - 布局文件名：`fragment_sample_list.xml` `activity_main.xml`
> - 生成的绑定类名：`FragmentSampleListBinding` `ActivityMainBinding`

## 在Activity中使用

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719090719804.png" alt="image-20240719090719804" style="zoom: 67%;" />

1. 声明全局变量

```java
 private ActivityMainBinding binding;
```

<u>这里`ActivityMainBinding`类就是步骤2中自动生成的绑定类的名字</u>

2. 绑定对象

```java
binding = ActivityMainBinding.inflate(getLayoutInflater());
```

>**`inflate`**：
>
>`ActivityMainBinding.inflate` 方法是由 ViewBinding 功能自动生成的一个静态方法。它用于创建 `ActivityMainBinding` 实例。这个方法会解析布局文件 `activity_main.xml`，并返回一个绑定对象，通过这个对象可以访问布局中的所有视图。
>
> **`getLayoutInflater`**：
>
>`getLayoutInflater` 方法是 `Activity` 类中的一个方法，它用于获取当前 `Activity` 的 `LayoutInflater` 对象。`LayoutInflater` 是一个用于解析 XML 布局文件并将其转换为相应的视图对象的类。

>  总结：
>
> 1. 调用 `getLayoutInflater` 方法获取当前活动的 `LayoutInflater` 实例。
>
> 2. 使用这个 `LayoutInflater` 实例调用 `ActivityMainBinding.inflate` 方法，解析 `activity_main.xml` 布局文件，并创建一个 `ActivityMainBinding` 实例。
>
> 3. `ActivityMainBinding` 实例会包含对 `activity_main.xml` 布局文件中所有视图的引用。通过这个绑定对象，你可以直接访问布局文件中的视图，而无需使用 `findViewById` 方法。

3. 设置内容视图

```java
setContentView(binding.getRoot());
```

使用`setContentView`将布局文件加载到当前活动中时，通过`binding.getRoot()`获取布局资源ID

代码示例：

```java
   public class MainActivity extends AppCompatActivity {
	
    // 声明 ViewBinding 全局变量
    private ActivityMainBinding binding;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        // 使用 ViewBinding 加载布局
        binding = ActivityMainBinding.inflate(getLayoutInflater());

        // 设置当前活动的内容视图为绑定的根视图
        setContentView(binding.getRoot());
    }
}
```

## 访问视图控件

> 通过绑定对象，可以直接访问布局文件中的视图控件。

```java
 binding.tv1.setText("修改后");
```

通过 ViewBinding 直接访问 `activity_main.xml` 布局文件中的 `TextView` 控件

![image-20240719092932642](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240719092932642.png)

```java
 	    binding.tv1.setText("修改后");
        binding.btn1.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(MainActivity.this, "按钮点击", Toast.LENGTH_SHORT).show();
            }
        });
```

# 区别

**与 findViewById 的区别**：

与使用 `findViewById` 相比，视图绑定具有一些很显著的优点：

- **Null 安全**：由于视图绑定会创建对视图的直接引用，因此不存在因视图 ID 无效而引发 null 指针异常的风险。此外，当视图仅存在于布局的某些配置中时，绑定类中包含其引用的字段会标记为 `@Nullable`。
- **类型安全**：每个绑定类中的字段都具有与其在 XML 文件中引用的视图相匹配的类型。这意味着不存在发生类转换异常的风险。

这些差异意味着布局和代码不兼容，会导致 build 在编译时（而不是运行时）失败。



> 参考：
>
> 1. [Android开发解放双手的利器ViewBinding](https://www.bilibili.com/video/BV1yf42117Fr/?spm_id_from=333.788.top_right_bar_window_history.content.click&vd_source=e82cc2aea0e4c86710c97cb4273a2830)
> 2. [视图绑定  | Android Developers](https://developer.android.com/topic/libraries/view-binding?hl=zh-cn)
> 3. [使用视图绑定替代findViewById](https://medium.com/androiddevelopers/use-view-binding-to-replace-findviewbyid-c83942471fc)