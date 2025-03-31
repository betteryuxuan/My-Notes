# webView

> `WebView` 是 Android 中用于显示网页内容的组件。它允许你在应用内嵌入一个浏览器，加载并显示 HTML、JavaScript、CSS 等网页内容。以下是使用 `WebView` 的步骤和一些示例代码。

## 使用步骤

1. **在布局文件中添加 `WebView`**：
   - 在 XML 布局文件中定义一个 `WebView` 控件。

2. **在 Activity 或 Fragment 中初始化 `WebView`**：
   - 获取布局中的 `WebView` 实例，并配置其设置。

3. **加载网页内容**：
   - 使用 `loadUrl()` 方法加载网页 URL，或使用 `loadData()` 方法加载 HTML 字符串。

4. **配置设置（可选）**：
   - 配置 JavaScript 支持、缓存设置等。

5. **处理 `WebView` 的事件（可选）**：
   - 设置 `WebViewClient` 和 `WebChromeClient` 来处理网页加载过程中的事件和 JavaScript 对话框。

6. **权限**

   在 `AndroidManifest.xml` 中添加互联网权限：

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

## 示例

1. 布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <WebView
        android:id="@+id/webview"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</RelativeLayout>
```

2. 在 Activity 中使用 `WebView`

在你的 `MainActivity.java` 中初始化并使用 `WebView`：

```java
import android.os.Bundle;
import android.webkit.WebChromeClient;
import android.webkit.WebResourceRequest;
import android.webkit.WebView;
import android.webkit.WebViewClient;

import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {

    private WebView webView;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        webView = findViewById(R.id.webview);

        // 启用 JavaScript 支持
        webView.getSettings().setJavaScriptEnabled(true);

        // 设置 WebViewClient 以处理网页加载事件
        webView.setWebViewClient(new WebViewClient());

        // 加载网页内容
        webView.loadUrl("https://www.baidu.com");
    }
}
```

**配置 WebView 设置**

- **JavaScript 支持**：启用 JavaScript 支持以处理动态网页内容。
  ```java
  webView.getSettings().setJavaScriptEnabled(true);
  ```

- **缓存设置**：可以设置缓存模式来优化加载速度。
  ```java
  webView.getSettings().setCacheMode(WebSettings.LOAD_DEFAULT);
  ```

- **自定义用户代理**：可以设置用户代理字符串。
  ```java
  webView.getSettings().setUserAgentString("Custom User Agent");
  ```

**处理 WebView 事件**

- **`WebViewClient`**：用于处理网页加载过程中的事件。
  - `onPageStarted(WebView view, String url, Bitmap favicon)`：当页面开始加载时调用。
  - `onPageFinished(WebView view, String url)`：当页面加载完成时调用。
  - `shouldOverrideUrlLoading(WebView view, WebResourceRequest request)`：处理 URL 跳转请求。

**权限**

在 `AndroidManifest.xml` 中添加互联网权限：

```xml
<uses-permission android:name="android.permission.INTERNET"/>
```

# HttpURLConnection

> `HttpURLConnection` 是 Android 中用于处理 HTTP 请求的类。它提供了一种在应用中发送和接收 HTTP 请求和响应的方法。

## 使用步骤

**1. 创建 `URL` 对象**：

- 使用指定的 URL 创建 `URL` 对象。

**2. 打开连接**：

- 使用 `URL.openConnection()` 打开连接，并将其转换为 `HttpURLConnection`。

**3. 配置连接参数**：

- 设置请求方法（如 GET 或 POST）。
- 配置连接和读取超时时间等参数。

**4. 发送请求**：

- 获取输入流并读取服务器响应。

**5. 处理响应**：

- 读取输入流中的数据，并进行处理。

**6. 关闭连接**：

- 关闭输入流和断开连接。

```java
// 创建一个URL对象，指定请求的URL地址
URL url = new URL("http://www.baidu.com");
// 打开一个到该URL的HTTP连接，并将其转换为HttpURLConnection
HttpURLConnection connection = (HttpURLConnection) url.openConnection();

// 设置请求方法为GET
connection.setRequestMethod("GET");
// 设置连接超时时间为8秒
connection.setConnectTimeout(8000);
// 设置读取超时时间为8秒
connection.setReadTimeout(8000);

// 获取服务器响应的输入流
InputStream inputStream = connection.getInputStream();
// 字节流->字符流->字符缓冲流
BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));
StringBuilder sb = new StringBuilder();
String line;
while ((line = reader.readLine()) != null) {
   sb.append(line);
}
return sb.toString();
// 断开与服务器的连接
connection.disconnect();
```

## 示例

获取百度网页源码

> Android 9.0（API level 28）及更高版本中，对所有的网络请求必须使用HTTPS

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    // 显示服务器响应的文本视图
    private TextView tv;
    // 发送请求的按钮
    private Button button;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        button = findViewById(R.id.btn_send);
        tv = findViewById(R.id.tv_response);
        button.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // 在新线程中执行网络请求
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 初始化HTTP连接和读取器
                HttpURLConnection connection = null;
                BufferedReader reader = null;
                try {
                    // 创建URL对象，指定请求的URL
                    URL url = new URL("https://www.baidu.com");
                    // 打开连接
                    connection = (HttpURLConnection) url.openConnection();
                    // 设置请求方法为GET
                    connection.setRequestMethod("GET");
                    // 设置连接超时和读取超时时间
                    connection.setConnectTimeout(8000);
                    connection.setReadTimeout(8000);
                    // 获取输入流，用于读取服务器响应
                    InputStream inputStream = connection.getInputStream();
                    // 将输入流包装为缓冲读取器
                    reader = new BufferedReader(new InputStreamReader(inputStream));
                    StringBuilder sb = new StringBuilder();
                    String line;
                    while ((line = reader.readLine()) != null) {
                        sb.append(line);
                    }
                    // 在UI线程中更新界面
                    showResponse(sb.toString());
                } catch (IOException e) {
                    throw new RuntimeException(e);
                } finally {
                    if (reader != null) {
                        try {
                            reader.close();
                        } catch (IOException e) {
                            throw new RuntimeException(e);
                        }
                    }
                    if (connection != null)
                        connection.disconnect();
                }
            }
        }).start();

    }

    // 在UI线程中更新响应文本
    private void showResponse(final String string) {
        runOnUiThread(new Runnable() {
            @Override
            public void run() {
                tv.setText(string);
            }
        });
    }
}
```

申请权限

```java
<uses-permission android:name="android.permission.INTERNET"/>
```

## GET请求

主要目的:从服务端获取符合条件的数据。

![image-20240806201410364](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240806201410364.png)

## POST请求

主要目的:向服务端提交数据。

场景举例:用户注册的时候，填写表格里一堆的信息(姓名、性别、年龄、电话..),这些都是要提交给服务端的数据

表单

![image-20240806201938856](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240806201938856.png)

![image-20240806203014879](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240806203014879.png)

# okHttp

在你的代码中，你使用了 OkHttp 来进行网络请求。以下是使用 OkHttp 的一般步骤和你代码中涉及到的各个部分的讲解：

## 使用步骤

## 1. 添加依赖

在你的 `build.gradle` 文件中添加 OkHttp 的依赖。如果你还没添加，可以在 `dependencies` 中加入以下行：

```gradle
implementation 'com.squareup.okhttp3:okhttp:4.10.0'
```
确保使用的是最新的版本。

## 2. 创建OkHttpClient实例

```java
OkHttpClient client = new OkHttpClient();
```
这是进行网络请求的客户端，配置了连接池、超时设置等参数。

## 3. 创建Request对象构建请求

```java
Request request = new Request.Builder()
        .url("https://www.baidu.com")
        .build();
```
使用 `Request.Builder` 来构建 HTTP 请求，包括设置 URL 和请求方法（GET、POST 等）。

## 4. 发送请求

调用`execute`方法发送请求并获取服务器返回数据

```java
Response response = client.newCall(request).execute();
```
`newCall(request).execute()` 会同步地执行请求，并返回一个 `Response` 对象。异步执行可以使用 `enqueue` 方法。

## 5. 获取响应

```java
final String responseData = response.body().string();
```
从 `Response` 对象中获取响应体，并将其转为字符串。响应体只能读取一次，所以多次使用它，可能需要将其保存到其他地方。

**更新 UI**：

```java
runOnUiThread(new Runnable() {
    @Override
    public void run() {
        tv.setText(responseData);
    }
});
```
由于网络请求是在子线程中执行的，任何更新 UI 的操作都必须在主线程中进行。使用 `runOnUiThread` 来确保 UI 更新在主线程中执行。

**示例**

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private TextView tv;      
    private Button button;  

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);  

        button = findViewById(R.id.btn_send);
        tv = findViewById(R.id.tv_response);

        button.setOnClickListener(this);
    }

    @Override
    public void onClick(View v) {
        // 在子线程中进行网络请求
        new Thread(new Runnable() {
            @Override
            public void run() {
                // 创建 OkHttpClient 实例
                OkHttpClient client = new OkHttpClient();

                // 构建一个 GET 请求，指定请求的 URL
                Request request = new Request.Builder()
                        .url("https://www.baidu.com")
                        .build();

                try {
                    // 执行网络请求，并获取响应
                    Response response = client.newCall(request).execute();

                    // 获取响应体的字符串形式
                    final String responseData = response.body().string();

                    // 使用 runOnUiThread() 确保 UI 更新在主线程中进行
                    runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            // 更新 TextView 的内容为网络请求的响应数据
                            tv.setText(responseData);
                        }
                    });
                } catch (IOException e) {
                    // 捕获并打印可能发生的 IOException
                    e.printStackTrace();
                }
            }
        }).start();  // 启动子线程进行网络请求
    }
}
```

# Pull解析方式

> Pull解析器是一种基于事件的解析方式，当解析器读取到XML文档中的开始标签、结束标签或文本内容时，会触发相应的事件。它是Android中处理XML的推荐方法之一，因其速度快且内存占用低。

## 1. 准备XML数据

假设我们有如下的XML数据：

```xml
<users>
    <user>
        <name>John Doe</name>
        <age>30</age>
        <married>true</married>
    </user>
    <user>
        <name>Jane Doe</name>
        <age>25</age>
        <married>false</married>
    </user>
</users>
```

## 2. 创建数据类

我们需要创建一个数据类来表示解析后的数据：

```java
public class User {
    private String name;
    private int age;
    private boolean married;

    // ...
}
```

## 3. 使用Pull解析器解析XML

在你的Activity或Fragment中使用Pull解析器解析XML数据：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // XML字符串
        String xmlData = "<users><user><name>John Doe</name><age>30</age><married>true</married></user><user><name>Jane Doe</name><age>25</age><married>false</married></user></users>";

        try {
            // 解析XML
            List<User> users = parseXML(xmlData);
            // 遍历解析得到的用户列表并打印日志
            for (User user : users) {
                Log.d("MainActivity", "User Name: " + user.getName());
                Log.d("MainActivity", "User Age: " + user.getAge());
                Log.d("MainActivity", "User Married: " + user.isMarried());
            }
        } catch (XmlPullParserException | IOException e) {
            e.printStackTrace();
        }
    }

    public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // XML字符串
        String xmlData = "<users><user><name>John Doe</name><age>30</age><married>true</married></user><user><name>Jane Doe</name><age>25</age><married>false</married></user></users>";

        try {
            // 解析XML
            List<User> users = parseXML(xmlData);
            // 遍历解析得到的用户列表并打印日志
            for (User user : users) {
                Log.d("MainActivity", "User Name: " + user.getName());
                Log.d("MainActivity", "User Age: " + user.getAge());
                Log.d("MainActivity", "User Married: " + user.isMarried());
            }
        } catch (XmlPullParserException | IOException e) {
            e.printStackTrace();
        }
    }

    // 解析XML数据
    private List<User> parseXML(String xmlData) throws XmlPullParserException, IOException {
        List<User> users = null; // 用于存储用户对象的列表
        User currentUser = null; // 当前正在解析的用户对象
        boolean insideUser = false; // 标记当前是否在<user>标签内

        // 创建XmlPullParserFactory和XmlPullParser实例
        XmlPullParserFactory factory = XmlPullParserFactory.newInstance();
        XmlPullParser parser = factory.newPullParser();
        parser.setInput(new StringReader(xmlData)); // 设置解析器的输入为XML字符串

        int eventType = parser.getEventType(); // 获取事件类型
        while (eventType != XmlPullParser.END_DOCUMENT) { // 遍历整个文档
            String tagName;
            switch (eventType) {
                case XmlPullParser.START_DOCUMENT:
                    users = new ArrayList<>(); // 初始化用户列表
                    break;
                case XmlPullParser.START_TAG:
                    tagName = parser.getName(); // 获取标签名
                    if (tagName.equals("user")) { // 如果是<user>标签
                        insideUser = true; // 标记进入<user>标签
                        currentUser = new User(); // 创建一个新的User对象
                    } else if (currentUser != null) {
                        // 仅当在<user>标签内部时，才解析这些标签
                        if (tagName.equals("name") && insideUser) {
                            currentUser.setName(parser.nextText());
                        } else if (tagName.equals("age") && insideUser) {
                            currentUser.setAge(Integer.parseInt(parser.nextText()));
                        } else if (tagName.equals("married") && insideUser) {
                            currentUser.setMarried(Boolean.parseBoolean(parser.nextText()));
                        }
                    }
                    break;
                case XmlPullParser.END_TAG:
                    tagName = parser.getName(); // 获取结束标签的名称
                    if (tagName.equals("user") && currentUser != null) {
                        users.add(currentUser); // 将解析完毕的用户对象添加到列表中
                        insideUser = false; // 标记离开<user>标签
                    }
                    break;
            }
            eventType = parser.next(); // 获取下一个事件
        }
        return users; // 返回解析得到的用户列表
    }
}
```

- 创建`XmlPullParserFactory`和`XmlPullParser`实例。
- 设置解析器的输入为XML字符串。
- 使用`while`循环遍历整个XML文档，根据事件类型（开始标签、结束标签、文本等）进行相应处理。
- 在遇到`<user>`标签时，创建新的`User`对象并开始填充数据。
- 在遇到`</user>`标签时，将填充完毕的`User`对象添加到`List<User>`中。
- 解析完成后，返回包含所有用户信息的列表。

# JSON

> JSON（JavaScript Object Notation）是一种轻量级的数据交换格式，易于人类阅读和编写，同时也易于机器解析和生成。

## 数据格式

### 基本结构

1. **对象**：由花括号`{}`包围，包含键值对。键是字符串，值可以是字符串、数字、对象、数组、布尔值或null。
   ```json
   {
       "name": "John",
       "age": 30,
       "married": true,
       "children": ["Anna", "Billy"],
       "address": {
           "street": "123 Main St",
           "city": "New York"
       }
   }
   ```

2. **数组**：由方括号`[]`包围，包含一个有序的值列表。
   ```json
   [
       "apple",
       "banana",
       "cherry"
   ]
   ```

3. **值**：可以是字符串、数字、对象、数组、布尔值或null。

### 数据类型

- **字符串**：必须用双引号包围。
  ```json
  "name": "John"
  ```

- **数字**：可以是整数或浮点数，不需要引号。
  ```json
  "age": 30,
  "height": 1.75
  ```

- **对象**：一个无序的键值对集合，键必须是字符串，值可以是任何合法的JSON数据。
  ```json
  "address": {
      "street": "123 Main St",
      "city": "New York"
  }
  ```

- **数组**：一个有序的值列表，值可以是任何合法的JSON数据。
  ```json
  "children": ["Anna", "Billy"]
  ```

- **布尔值**：`true`或`false`。
  ```json
  "married": true
  ```

- **null**：表示空值。
  ```json
  "middle_name": null
  ```

**示例**：

```json
{
    "name": "John Doe",
    "age": 30,
    "married": true,
    "children": [
        {
            "name": "Anna",
            "age": 10
        },
        {
            "name": "Billy",
            "age": 5
        }
    ],
    "address": {
        "street": "123 Main St",
        "city": "New York",
        "zip": "10001"
    },
    "phone_numbers": ["123-456-7890", "987-654-3210"],
    "email": "john.doe@example.com",
    "website": null
}
```

## 数据解析

在Java中，`JSONObject`类用于表示一个JSON对象，并提供了多种方法来获取其键对应的值。

### 获取值的方法

1. **opt(String key)**：
   - 根据键获取值，如果键不存在，则返回`null`。
   - 推荐使用，因为不会抛出异常。
   ```java
   JSONObject jsonObject = new JSONObject("{\"name\":\"John\", \"age\":30}");
   Object value = jsonObject.opt("name"); // 返回 "John"
   Object missingValue = jsonObject.opt("nonexistent"); // 返回 null
   ```

2. **get(String key)**：
   - 根据键获取值，如果键不存在，则抛出`JSONException`。
   ```java
   JSONObject jsonObject = new JSONObject("{\"name\":\"John\", \"age\":30}");
   Object value = jsonObject.get("name"); // 返回 "John"
   Object missingValue = jsonObject.get("nonexistent"); // 抛出 JSONException
   ```

### 根据值的类型获取

1. **optString(String key)**：
   - 根据键获取字符串值，如果键不存在或值不是字符串，则返回空字符串或null。
   ```java
   String name = jsonObject.optString("name"); // 返回 "John"
   String missingName = jsonObject.optString("nonexistent"); // 返回 ""
   ```

2. **optInt(String key)**：
   - 根据键获取整数值，如果键不存在或值不是整数，则返回0或指定的默认值。
   ```java
   int age = jsonObject.optInt("age"); // 返回 30
   int missingAge = jsonObject.optInt("nonexistent", -1); // 返回 -1
   ```

3. **optBoolean(String key)**：
   - 根据键获取布尔值，如果键不存在或值不是布尔值，则返回false或指定的默认值。
   ```java
   boolean isMarried = jsonObject.optBoolean("married"); // 返回 true 或 false
   boolean missingMarried = jsonObject.optBoolean("nonexistent", true); // 返回 true
   ```

4. **optJSONObject(String key)**：
   
   - 根据键获取`JSONObject`对象，如果键不存在或值不是`JSONObject`，则返回null。
   ```java
   JSONObject address = jsonObject.optJSONObject("address"); // 返回一个 JSONObject 对象或 null
   ```
   
5. **optJSONArray(String key)**：
   - 根据键获取`JSONArray`对象，如果键不存在或值不是`JSONArray`，则返回null。
   ```java
   JSONArray children = jsonObject.optJSONArray("children"); // 返回一个 JSONArray 对象或 null
   ```

### 完整示例

# GSON

## 1. 添加Gson依赖

在`build.gradle`文件中添加Gson依赖：

```gradle
dependencies {
    implementation 'com.google.code.gson:gson:2.8.8'
}
```

## 2. 创建数据类

假设我们有一个代表用户的JSON数据，我们首先需要创建一个对应的数据类：

```java
public class User {
    private String name;
    private int age;
    private boolean isMarried;

    //...
}
```

## 3. 使用Gson

在你的Activity或Fragment中使用Gson进行JSON序列化和反序列化。

### 序列化

**对象转JSON**

> `String userJson = gson.toJson(user);`

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 创建User对象
        User user = new User("John Doe", 30, true);

        // 创建Gson对象
        Gson gson = new Gson();

        // 将User对象序列化为JSON
        String userJson = gson.toJson(user);
        Log.d("MainActivity", "User JSON: " + userJson); // 输出: {"name":"John Doe","age":30,"isMarried":true}
    }
}
```

### 反序列化

**JSON转对象**

> ` Gson gson = new Gson();`
>
> `User user = gson.fromJson(userJson, User.class);`

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // JSON字符串
        String userJson = "{\"name\":\"John Doe\",\"age\":30,\"isMarried\":true}";

        // 创建Gson对象
        Gson gson = new Gson();

        // 将JSON字符串反序列化为User对象
        User user = gson.fromJson(userJson, User.class);
        Log.d("MainActivity", "User Name: " + user.getName()); // 输出: John Doe
        Log.d("MainActivity", "User Age: " + user.getAge()); // 输出: 30
        Log.d("MainActivity", "User Married: " + user.isMarried()); // 输出: true
    }
}
```

## 4. 其他问题

### 处理嵌套对象

创建相应的数据类，并使用Gson进行处理。

```java
public class Address {
    private String street;
    private String city;

    //...
}

public class User {
    private String name;
    private int age;
    private boolean isMarried;
    private Address address;

    //...
}
```

然后在Activity中使用Gson进行序列化和反序列化：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 创建嵌套的User对象
        Address address = new Address("123 Main St", "New York");
        User user = new User("John Doe", 30, true, address);

        // 创建Gson对象
        Gson gson = new Gson();

        // 将User对象序列化为JSON
        String userJson = gson.toJson(user);
        Log.d("MainActivity", "User JSON: " + userJson); // 输出嵌套的JSON字符串

        // 将嵌套的JSON字符串反序列化为User对象
        User userFromJson = gson.fromJson(userJson, User.class);
        Log.d("MainActivity", "User Name: " + userFromJson.getName()); // 输出: John Doe
        Log.d("MainActivity", "User Address: " + userFromJson.getAddress().getStreet()); // 输出: 123 Main St
    }
}
```

### 字段名不一致

在使用Gson进行JSON序列化和反序列化时，字段名不一致的问题可以通过`@SerializedName`注解来解决。`@SerializedName`注解允许你指定JSON中的字段名称与Java对象中的字段名称之间的映射。

1. 假设我们有一个JSON字符串：

```json
{
    "full_name": "John Doe",
    "user_age": 30,
    "is_married": true
}
```

2. 我们可以创建一个对应的数据类，并使用`@SerializedName`注解来处理字段名不一致的问题：

```java
public class User {
    @SerializedName("full_name")
    private String name;

    @SerializedName("user_age")
    private int age;

    @SerializedName("is_married")
    private boolean isMarried;
    
    //...
}
```

---

---

==***感谢您的阅读
如有错误烦请指正***==

---

>  参考：
>
>  1. [31.2-Android网络请求的Get与Post方法(1)_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1Cv411J7pQ/?spm_id_from=333.788&vd_source=b59429d4ff39742daea8fe7f3aac55a4)
>  2. [31.4-Android网络请求之JSON数据解析_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1tf4y1j7S9/?spm_id_from=333.788&vd_source=b59429d4ff39742daea8fe7f3aac55a4)
