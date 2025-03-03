

步骤1：添加Retrofit库的依赖

步骤2：创建 接收服务器返回数据 的类

步骤3：创建 用于描述网络请求 的接口

步骤4：创建 Retrofit 实例

步骤5：创建 网络请求接口实例 并 配置网络请求参数

步骤6：发送网络请求（异步 / 同步）

步骤7：处理服务器返回的数据

# 示例

## 天气

### 1. 导入依赖

```kotlin
	implementation("com.squareup.retrofit2:retrofit:2.0.2")
    implementation("com.squareup.okhttp3:okhttp:3.1.2")
    //gson
    implementation("com.squareup.retrofit2:converter-gson:2.0.2")
```

### 2. 添加网络权限

```java
    <uses-permission android:name="android.permission.INTERNET" />
```

### 3. 创建 接收服务器返回数据 的类

`City`

天气返回的json数据格式：

```json
{
  "code": "200",
  "updateTime": "2020-06-30T22:00+08:00",
  "fxLink": "http://hfx.link/2ax1",
  "now": {
    "obsTime": "2020-06-30T21:40+08:00",
    "temp": "24",
    "feelsLike": "26",
    "icon": "101",
    "text": "多云",
    "wind360": "123",
    "windDir": "东南风",
    "windScale": "1",
    "windSpeed": "3",
    "humidity": "72",
    "precip": "0.0",
    "pressure": "1003",
    "vis": "16",
    "cloud": "10",
    "dew": "21"
  },
  "refer": {
    "sources": [
      "QWeather",
      "NMC",
      "ECMWF"
    ],
    "license": [
      "QWeather Developers License"
    ]
  }
}
```

数据类

```java
package com.example.retrofitpractice;

public class City {
    private String code;
    private String updateTime;

    private now now;

    private static class now {
        private String temp;
        private String text;
        private String windDir;

        @Override
        public String toString() {
            return "temp='" + temp + '\'' +
                    ", text='" + text + '\'' +
                    ", windDir='" + windDir ;
        }
    }

    @Override
    public String toString() {
        return "City{" +
                "code='" + code + '\'' +
                ", updateTime='" + updateTime + '\'' +
                ", now=" + now.toString() +
                '}';
    }
}
```

### 4. 创建 用于描述网络请求 的接口

`GetRequest_Interface`

```java
package com.example.retrofitpractice;

import retrofit2.Call;
import retrofit2.http.GET;

public interface GetRequest_Interface {

    @GET("v7/weather/now?location=101010100&key=8d10337174fe4491894596b13e2155bb")
    Call<City> getCall();
    // @GET注解的作用:采用Get方法发送网络请求
    // getCall() = 接收网络请求数据的方法
    // 其中返回类型为Call<*>，*是接收数据的类（即上面定义的City类）
}
```

### 5.  开始请求处理数据

```java
public class MainActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
 

        request();
    }

    private void request() {

        // 创建Retrofit对象
        Retrofit retrofit = new Retrofit.Builder()
                .baseUrl("https://api.qweather.com/")
                .addConverterFactory(GsonConverterFactory.create())
                .build();
        // 创建 网络请求接口 的实例
        GetRequest_Interface request = retrofit.create(GetRequest_Interface.class);

        //对 发送请求 进行封装
        Call<City> call = request.getCall();

        // 发送网络请求(异步)
        call.enqueue(new Callback<City>() {
            @Override
            public void onResponse(Call<City> call, Response<City> response) {
                String string = response.body().toString();
                Log.d("MainTag", string);
            }

            @Override
            public void onFailure(Call<City> call, Throwable t) {
                Log.e("MainTag", "连接失败");
            }
        });
    }
}
```

