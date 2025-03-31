# 进程和线程

1. **进程（Process）**：

> **定义**：每个运行的Android应用程序（App）都在自己的进程中运行。进程是独立的，它有自己的内存空间和资源。

**跨应用数据传递**：不同应用之间通过`ContentProvider`、`Broadcast`等方式进行数据交换。

2. **线程（Thread）**：

> **定义**：进程内部的执行单元。每个应用默认有一个主线程（UI线程），用于处理UI更新和用户交互。

- **资源**：线程共享进程的内存空间，线程之间可以访问共享资源。

# 多进程模式

在 Android 中，进程是一个独立的执行环境，每个应用默认在其专属的主进程中运行。通过 `android:process` 属性可以在 `AndroidManifest.xml` 中指定组件所属的进程。

## 进程的私有和共有

- **私有进程**：以应用包名开头的进程为私有进程，例如 `android:process=":myProcess"`。这种进程对应用自身可见，不会与其他应用共享。私有进程的命名以“:”（冒号）开头，并且不包括包名。
  
  ```xml
  <service android:name=".MyService"
           android:process=":myProcess" />
  ```

  以上示例定义了一个名为 `myProcess` 的私有进程，它仅对当前应用可见。

- **共有进程**：如果需要在不同应用间共享进程，则可以将进程名设为全局进程名，即不以“:”开头的名称。例如，`android:process="com.example.sharedProcess"`。这种进程可以在不同应用之间共享，但需要确保应用间的签名相同（每个应用使用相同的 `.keystore` 文件签名）（签名相同的应用可以共享内存和数据）。

  ```xml
  <service android:name=".SharedService"
           android:process="com.example.sharedProcess" />
  ```

**私有进程与共有进程的用途**

- **私有进程**：适合不需要跨应用的内部功能，例如需要隔离资源或单独处理任务的情况。
- **共有进程**：适合同一公司或同一开发者的多个应用间共享数据或服务的需求，比如应用套件中的多个应用共享后台服务。

## 问题

1. 静态成员和单例模式完全失效。
2. 线程同步机制完全失效。
3. SharedPreferences的可靠性下降。
4. Application会多次创建。

## 本质

* **独立虚拟机**：每个进程有各自的虚拟机，因此无法直接共享内存数据。即使在一个应用中，不同进程的对象实例和内存状态也是相互独立的。
* **独立的 `Application` 实例**：多进程模式下，每个进程都会初始化自己独立的 `Application` 实例，因此无法直接共享 `Application` 中的单例对象或者成员变量。这就类似于两个应用独立运行，彼此无法直接访问对方的数据。
* **独立的内存空间**：每个进程有独立的内存管理，这意味着无法通过内存来直接共享对象数据，即便是在同一个应用的多进程间。因此，在多进程模式下，使用 `SharedPreferences`、文件存储、数据库等方式来跨进程共享数据更为常见。

> 在 Android 中，如果两个应用共享同一个 `sharedUserId`，并且签名相同，它们将拥有访问彼此数据的权限
>
> 多进程模式下的同一个应用，不同进程的组件通过 `SharedUID` 的方式访问资源和数据的过程，和两个应用共享 `SharedUID` 的原理类似。

# IPC基础概念

## Serializable接口

> `Serializable` 接口是 Java 中用于将对象序列化的接口，它的作用是将一个对象的状态转换为字节流，以便保存到文件或通过网络传输，然后在需要时恢复成原始对象。

### 用途

1. **持久化存储**：可以将对象序列化后保存到文件或数据库中，以便在应用重启时恢复对象的状态。
2. **网络传输**：序列化的对象可以通过网络在不同的系统或应用之间传输，尤其在分布式系统中非常有用。
3. **跨进程通信**：Android 中的 `Intent`、`Bundle` 等需要传递对象时，可以通过 `Serializable` 或 `Parcelable` 接口实现对象的跨进程传输。

### 使用方法
使用 `Serializable` 非常简单，只需要让一个类实现 `Serializable` 接口即可，不需要实现任何方法，因为它是一个标记接口（没有方法的接口）。例如：

```java
import java.io.Serializable;

public class Person implements Serializable {
    private static final long serialVersionUID = 1L; // 序列化版本号

    private String name;
    private int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    // Getter 和 Setter 方法
}
```

### 注意事项
1. **serialVersionUID**：`serialVersionUID` 是一个唯一标识符，用于验证序列化和反序列化的版本一致性。如果类结构发生变化（如添加或移除字段），没有指定 `serialVersionUID` 会导致反序列化失败。定义固定的 `serialVersionUID` 可以确保在类结构变更时仍然可以反序列化旧版本的对象。
  
   ```java
   private static final long serialVersionUID = 1L;
   ```

2. **transient 关键字**：在序列化过程中，某些字段可能不需要被序列化（比如敏感信息），可以使用 `transient` 关键字标记这些字段，序列化时会忽略这些字段。

   ```java
   private transient String password;
   ```

### 示例
```java
import java.io.*;

public class SerializationExample {
    public static void main(String[] args) {
        Person person = new Person("Alice", 25);

        // 序列化对象
        try (ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("person.ser"))) {
            oos.writeObject(person);
            oos.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

        // 反序列化对象
        try (ObjectInputStream ois = new ObjectInputStream(new FileInputStream("person.ser"))) {
            Person deserializedPerson = (Person) ois.readObject();
            ois.close();
        } catch (IOException | ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

## ParceLable接口

> `Parcelable`接口用于在不同组件（如Activity或Service）之间传递对象的数据，尤其是在Intent、Bundle、或IPC（进程间通信）时。相比于`Serializable`，`Parcelable`更加高效，因为它避免了过多的反射开销，并在Android系统底层进行了优化。

### 使用方法

1. **实现`Parcelable`接口**：需要在自定义类中实现`Parcelable`接口。
2. **重写`writeToParcel()`方法**：此方法用于将类的字段写入`Parcel`对象中，以便后续能够恢复该对象。通常使用`Parcel`提供的`writeXxx()`方法来写入每个字段。
3. **定义一个静态字段`CREATOR`**：这是一个`Parcelable.Creator<T>`类型的常量，它负责从`Parcel`中读取数据并返回该类的实例。`CREATOR`对象包含两个主要方法：
   - `createFromParcel(Parcel in)`：根据给定的`Parcel`对象创建一个新的实例。
   - `newArray(int size)`：创建指定大小的数组。
4. **重写`describeContents()`方法**：通常返回0，用于描述特殊的内容类型。

### 示例

```java
public class User implements Parcelable {
    private String phoneNum;   
    private String password;   

    public User(String password, String phoneNum) {
        this.password = password;
        this.phoneNum = phoneNum;
    }

    // 从 Parcel 中读取数据的构造函数，用于反序列化
    protected User(Parcel in) {
        phoneNum = in.readString();   // 从 Parcel 中读取电话号码
        password = in.readString();   // 从 Parcel 中读取密码
    }

    // describeContents 方法，一般返回 0
    @Override
    public int describeContents() {
        return 0;
    }

    // writeToParcel 方法，将数据写入 Parcel，以便序列化
    @Override
    public void writeToParcel(@NonNull Parcel dest, int flags) {
        dest.writeString(phoneNum);   // 将电话号码写入 Parcel
        dest.writeString(password);   // 将密码写入 Parcel
    }

    // CREATOR 对象，负责从 Parcel 中创建 User 实例
    public static final Creator<User> CREATOR = new Creator<User>() {
        // 从 Parcel 中读取数据并创建 User 对象
        @Override
        public User createFromParcel(Parcel source) {
            return new User(source);   // 调用 User(Parcel in) 构造函数
        }

        @Override
        public User[] newArray(int size) {
            return new User[size];
        }
    };
}
```

```java
User user = new User("fyx", "04");
Intent intent = new Intent(this, SecondActivity.class);
intent.putExtra("User", user);
startActivity(intent);
```

```java
Intent intent = getIntent();
User user = intent.getParcelableExtra("User");
if (user != null) {
    Log.d("SecondActivityTag", user.toString());
}
```

### 对比

> **Serializable**：实现简单，但性能较差，适用于对象较小、传输不频繁的场景，或者需要持久化存储（如文件、数据库）。
>
> **Parcelable**：实现稍复杂，但性能更高，适用于Android中对象传递，尤其是通过`Intent`、`Bundle`等组件传递数据时。

## Binder

机制:Binder是一种进程间通信机制:
驱动:Binder是一个虚拟物理设备驱动;
应用层:Binder是一个能发起通信Java类:
Framework/Native:Binder连接了Client、Server、
Service Manager和Binder驱动程序，形成一套C/s的通信架构。

![image-20241114101620831](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241114101620831.png)

AIDL对BInder的封装

每个进程都有binder

进程创建

# IPC方式

## Bundle

Bundle 是 Android 提供的一种轻量级的数据容器，继承自 `BaseBundle`，实现了 `Parcelable` 接口。

由于实现了 `Parcelable`，Bundle 可用于进程间通信，通过序列化和反序列化实现数据传递。

1. **Activity 之间的数据传递**

- 使用 `Intent` 携带 `Bundle`，实现 Activity 之间的通信。

```java
Intent intent = new Intent(this, SecondActivity.class);
Bundle bundle = new Bundle();
bundle.putString("key", "value");
intent.putExtras(bundle);
startActivity(intent);
```

2. **Service 和 Activity 的通信**

- 在使用 `Messenger` 时，通过 `Message` 对象的 `data` 属性传递 `Bundle` 数据。

```java
Message msg = Message.obtain();
Bundle data = new Bundle();
data.putString("msg", "Hello, Service!");
msg.setData(data);
messenger.send(msg);
```

3. **BroadcastReceiver 数据传递**

- `BroadcastReceiver` 中的广播也可以通过 `Bundle` 携带数据。

```java
Intent intent = new Intent("com.example.MY_BROADCAST");
intent.putExtra("key", "value");
sendBroadcast(intent);
```

4. **Fragment 与 Activity 或其他 Fragment 通信**

- 通过 `Bundle` 设置 `Fragment` 的参数。

```java
Fragment fragment = new MyFragment();
Bundle args = new Bundle();
args.putInt("id", 123);
fragment.setArguments(args);
```

## 文件共享

Parcelable不能直接用于文件的读写操作

User2使用ParceLable序列化

这里在MainActivity中存入对象，在SecondActivity中读取

```java
//MainActivity
@Override
protected void onResume() {
    super.onResume();

    User2 user = new User2("Alice", 25);

    // 文件路径
    String filePath = getExternalFilesDir(null) + "/user_data";

    // 序列化对象到文件
    try (FileOutputStream fos = new FileOutputStream(filePath);
         ObjectOutputStream oos = new ObjectOutputStream(fos)) {

        oos.writeObject(user);
        oos.flush();
        Log.d("ipcTag", "User 对象已序列化到文件：" + filePath);

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

```java
//SecondActivity
@Override
protected void onResume() {
    super.onResume();

    String filePath = getExternalFilesDir(null) + "/user_data";

    try (FileInputStream fis = new FileInputStream(filePath);
         ObjectInputStream ois = new ObjectInputStream(fis)) {

        User2 user = (User2) ois.readObject();
        Log.d("ipcTag", "反序列化的 User 对象：" + user);

    } catch (Exception e) {
        e.printStackTrace();
    }
}
```

## Messenger

> **`Messenger`** 是一种用于进程间通信（IPC）的机制，它允许你通过消息传递的方式在不同进程中的 `Handler` 之间传递数据。`Messenger` 是通过 `Message` 和 `Handler` 来传递数据的，可以在不同进程之间传递消息，并且避免了传统的接口调用，降低了复杂性。

### 客户端向服务端

客户端

```java
public class ThirdActivity extends AppCompatActivity {
    private Messenger mService;

    private ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            mService = new Messenger(service);

            Bundle data = new Bundle();
            data.putString("name", "feng");

            Message msg = Message.obtain();
            msg.what = 0;
            msg.setData(data);

            try {
                mService.send(msg);
            } catch (RemoteException e) {
                throw new RuntimeException(e);
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {

        }
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = new Intent(this, MessengerService.class);
        bindService(intent, connection, 1);
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        unbindService(connection);
    }
}
```

服务端

```java
public class MessengerService extends Service {
    private final Messenger messenger = new Messenger(new MessengerHandler());

    public MessengerService() {
    }

    @Override
    public IBinder onBind(Intent intent) {
        return messenger.getBinder();
    }

    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(@NonNull Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 0:
                    Bundle data = msg.getData();
                    String name = data.getString("name");
                    Log.d("MessengerService", "receive " + name + " from client");
                default:
                    super.handleMessage(msg);
            }
        }
    }
}
```

### 服务端回复 

```java
public class MessengerService extends Service {
    private final Messenger messenger = new Messenger(new MessengerHandler());
...
    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(@NonNull Message msg) {
            super.handleMessage(msg);
            switch (msg.what) {
                case 0:
                    Bundle data = msg.getData();
                    User2 user2 = (User2) data.getSerializable("user");
                    Log.d("MessengerService", "receive " + user2 + " from client");
 
                    // 从消息中提取 replyTo，用于向客户端回复消息
                    Messenger replyTo = msg.replyTo;
                    
                    Message replyMessage = Message.obtain();
                    replyMessage.what = 0;
                    Bundle bundle = new Bundle();
                    bundle.putString("reply", "123456");
                    replyMessage.setData(bundle);
                    
                    try {
                        // 使用 replyTo 发送回复消息
                        replyTo.send(replyMessage);
                    } catch (RemoteException e) {
                        throw new RuntimeException(e);
                    }
                default:
                    super.handleMessage(msg);
            }
        }
    }
}
```

客户端

```java
public class ThirdActivity extends AppCompatActivity {
    private Messenger mService;

    private ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            mService = new Messenger(service);

            Bundle data = new Bundle();
            User2 user = new User2("feng", 20);
            data.putSerializable("user", user);

            Message msg = Message.obtain();
            msg.what = 0;
            msg.setData(data);
            // 设置回信的信使
            msg.replyTo = mGetReplyMessenger;

            try {
                mService.send(msg);
            } catch (RemoteException e) {
                throw new RuntimeException(e);
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {

        }
    };

    // 接收服务返回消息的信使，使用 MessengerHandler 处理返回的消息
    private Messenger mGetReplyMessenger = new Messenger(new MessengerHandler());

    // 静态内部类，处理从服务返回的消息
    private static class MessengerHandler extends Handler {
        @Override
        public void handleMessage(@NonNull Message msg) {
            switch (msg.what) {
                case 0:
                    Bundle data = msg.getData();
                    String reply = data.getString("reply");
                    Log.d("MessengerService", "ThirdActivity: " + reply);
            }
        }
    }
    ...
}
```

