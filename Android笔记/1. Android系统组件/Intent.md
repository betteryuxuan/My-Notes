# 使用Intent在活动中穿梭

Intent（意图）是一种重要的消息传递对象，用于在不同组件（如活动（Activity）、服务（Service）、广播接收器（BroadcastReceiver）等）之间进行通信和交互。

## 组成

Intent 主要由以下几个部分组成：

- **Action**：表示要执行的操作，比如 `ACTION_VIEW`、`ACTION_SEND` 等。
- **Data**：表示要操作的数据，通常是一个 URI。
- **Category**：表示 Intent 的类型，比如 `CATEGORY_LAUNCHER`、`CATEGORY_DEFAULT` 等。
- **Extras**：表示附加的信息，可以通过 `putExtra()` 方法添加键值对。

## 显式Intent

- 用于在应用内部启动组件（如Activity、Service、BroadcastReceiver）。显式Intent会指定要启动的组件的类名，例如：

  ```java
        button1.setOnClickListener(new View.OnClickListener() {
              @Override
              public void onClick(View v) {
                  // 创建一个从 MainActivity 到 SecondActivity 的 Intent
                  Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                  // 启动 SecondActivity
                  startActivity(intent);
              }
         });
  ```

  创建一个 `Intent` 对象，用于启动 `SecondActivity`。`Intent` 的第一个参数是当前的 `Context`（即 `MainActivity.this`），第二个参数是要启动的目标 `Activity` 类（即 `SecondActivity.class`）。

## 隐式Intent

- 用于在不指定组件名称的情况下启动组件，而是通过指定动作（Action）、数据（Data）和类型（Type）等信息，让系统去匹配合适的组件。

1. 启动`activity`

在标签中添加下面代码：

  ```xml
  <activity
      android:name=".SecondActivity"
      android:exported="true"
      android:theme="@style/Theme.AppCompat.DayNight.DialogWhenLarge">
      <intent-filter>
          <!-- 指定动作为 com.example.menutest_716.SecondActivity -->
          <action android:name="com.example.menutest_716.SecondActivity" />
          
          <!-- 添加默认分类 -->
          <category android:name="android.intent.category.DEFAULT" />
          
          <!-- 添加自定义分类 com.example.menutest_716.MY_CATEGORY -->
          <category android:name="com.example.menutest_716.MY_CATEGORY" />
      </intent-filter>
  </activity>
  ```

```java
Button button5 = findViewById(R.id.button5);
button5.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // 创建一个隐式 Intent，指定动作为 com.example.menutest_716.SecondActivity
        Intent intent = new Intent("com.example.menutest_716.SecondActivity");
        
        // 添加自定义分类 com.example.menutest_716.MY_CATEGORY
        intent.addCategory("com.example.menutest_716.MY_CATEGORY");
        
        // 启动符合条件的 Activity
        startActivity(intent);
    }
});
```

2. 打开网页

  ```java
  button6.setOnClickListener(new View.OnClickListener() {
      @Override
      public void onClick(View v) {
          // 创建一个新的 Intent，动作为 ACTION_VIEW
          Intent intent = new Intent(Intent.ACTION_VIEW);
          
          // 设置 Intent 的数据为指定的 URI，这里是打开百度网站的示例
          intent.setData(Uri.parse("http://www.baidu.com"));
          
          // 启动能够处理此 Intent 的 Activity
          startActivity(intent);
      }
  });
  ```

3. 拨打电话

```java
button8.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        // 创建一个拨号的 Intent
        Intent intent = new Intent(Intent.ACTION_DIAL);
        // 设置 Intent 的数据为要拨打的电话号码
        intent.setData(Uri.parse("tel:10086"));
        // 启动该 Intent，跳转到系统的拨号界面
        startActivity(intent);
    }
});
```

## 显式与隐式区别

- **目的**：显式Intent用于精确指定要启动的组件，通常在应用内部使用；隐式Intent用于请求系统启动能够处理指定动作或数据类型的任何组件。
- **使用方式**：显式Intent直接指定组件类名，而隐式Intent通过设置动作、数据等信息间接指定要启动的组件，系统会根据匹配规则找到合适的组件处理请求。

## 作用

1. **组件通信**：Intent 允许应用程序的不同组件（如活动、服务、广播接收器）之间进行通信。通过发送Intent，一个组件可以请求另一个组件执行特定的操作或返回结果。
2. **启动活动**：当你需要从一个活动跳转到另一个活动时，可以使用Intent来启动目标活动。这使得应用程序的导航更加灵活和动态。
3. **启动服务**：服务是后台运行的组件，通常用于执行长时间运行的操作，如下载文件或播放音乐。通过Intent，你可以启动或绑定到一个服务，并与之交互。
4. **发送广播**：广播是一种消息传递机制，允许应用程序向系统或应用程序的其他部分发送消息。Intent 可以用于发送和接收广播，从而实现应用程序之间的通信。
5. **数据传输**：Intent 可以携带数据（如字符串、URI、文件等），这使得数据可以在不同的组件之间传递。这对于实现复杂的交互和数据共享非常有用。
6. **隐式Intent**：隐式Intent不指定具体的组件，而是通过定义一个操作（action）、数据类型（data）和类别（category）来请求系统执行相应的操作。这使得应用程序可以更灵活地与其他应用程序或系统服务交互。
7. **显式Intent**：显式Intent直接指向特定的组件，如某个活动或服务。这使得开发者可以精确控制应用程序的行为和交互。

# 活动间传递数据

## 向下一个活动传递数据

有两个活动：`MainActivity` 和 `SecondActivity`，要从 `MainActivity` 向 `SecondActivity` 传递数据。

在 `MainActivity` 中：

1. 创建一个 `Intent` 对象，并使用 `putExtra` 方法添加要传递的数据。
2. 使用 `startActivity` 方法启动 `SecondActivity`。

```java
        Button button3 = findViewById(R.id.button3);
        button3.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String data  = "祝你开心永远";
                Intent intent = new Intent(MainActivity.this, SecondActivity.class);
                intent.putExtra("extra_data",data);
                startActivity(intent);
            }
        });
```

在 `SecondActivity` 中：

1. 在 `onCreate` 方法中获取传递过来的数据。

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_second);
    
    // 获取传递的数据
    Intent intent = getIntent();
    String value = intent.getStringExtra("key");
}
```

## 返回数据给上一个活动

在 Android 中，如果我们需要从一个活动返回数据给上一个活动，可以使用 `startActivityForResult` 方法启动目标活动，并在目标活动中设置结果数据。

在 `MainActivity` 中：

1. 使用 `startActivityForResult` 方法启动 `SecondActivity`。
2. 重写 `onActivityResult` 方法来接收返回的数据。

```java
// 启动 SecondActivity 并请求返回结果
Intent intent = new Intent(MainActivity.this, SecondActivity.class);
startActivityForResult(intent, 1);

// 重写 onActivityResult 方法来接收返回的数据
@Override
protected void onActivityResult(int requestCode, int resultCode, Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == 1 && resultCode == RESULT_OK) {
        // 从返回的 Intent 中获取数据
        String result = data.getStringExtra("result_key");
        // 处理返回的数据
    }
}
```

在 `SecondActivity` 中：

1. 创建一个 `Intent` 对象，并使用 `putExtra` 方法添加要返回的数据。
2. 使用 `setResult` 方法设置结果，并传入包含数据的 `Intent`。
3. 调用 `finish` 方法关闭当前活动。

```java
// 创建一个 Intent 对象并添加要返回的数据
Intent returnIntent = new Intent();
returnIntent.putExtra("result_key", "result_value");

// 设置结果并传入包含数据的 Intent
setResult(RESULT_OK, returnIntent);

// 关闭当前活动
finish();
```

---

---

==***感谢您的阅读
如有错误烦请指正***==
