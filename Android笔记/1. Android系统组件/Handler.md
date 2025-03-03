`Handler` 是用于在不同线程之间进行消息传递的机制。它与`Looper`和`MessageQueue`一起工作，帮助在不同线程间传递和处理消息，特别是在主线程与子线程之间进行通信。

# 用处

1. **子线程通过 `Handler` 更新 UI**
2. **用于线程间通信**

# 基本用法

##  用法一：使用sendMessage和handleMessage方法

1. **创建Handler**：通常在主线程中创建一个`Handler`，用于处理从其他线程发送的消息。

   ```java
   Handler handler = new Handler();
   ```

2. **发送消息**：在子线程中，通常使用`handler.sendMessage()`来向主线程发送消息。

   ```java
   new Thread(new Runnable() {
       @Override
       public void run() {
           Message message = Message.obtain();
           message.what = 1;
           handler.sendMessage(message);
       }
   }).start();
   ```

3. **处理消息**：主线程中的`Handler`会调用`handleMessage()`方法来处理接收到的消息。

   ```java
   Handler handler = new Handler(Looper.getMainLooper()) {
       @Override
       public void handleMessage(Message msg) {
           switch (msg.what) {
               case 1:
                   // 处理消息
                   break;
           }
       }
   };
   ```


## 用法二：使用post方法

1. **创建 `Handler`**：

   ```java
   Handler handler = new Handler(Looper.getMainLooper());
   ```

2. **使用 `post` 方法**：可以直接在 `Handler` 中执行需要在主线程上运行的代码块。

   ```java
   handler.post(new Runnable() {
       @Override
       public void run() {
           // 执行主线程任务
       }
   });
   ```

   > 区别：
   >
   > **`sendMessage`**: 适用于更复杂的消息处理和任务调度，特别是需要在多个线程之间通信时。它依赖于 `Handler` 类，通过消息机制来传递和处理任务。
   >
   > **`post`**: 更加简洁和直接，主要用于在主线程中执行任务，尤其是需要更新 UI 的情况。适合用于在视图中直接执行代码而不涉及复杂的线程通信。

# 法一工作原理

主要依靠以下

1. **Looper**：负责不断从 `MessageQueue` 中取出消息并分发给相应的 `Handler` 进行处理。

2. **Handler**：`Handler` 是一个用于发送和处理消息的类。它可以用来将 `Message` 或 `Runnable` 对象发送到消息队列中，并在对应的线程中处理这些消息。

3. **Message**：用于在 `Handler` 和 `Looper` 之间传递消息。

4. **MessageQueue**：`MessageQueue` 是消息的容器，负责存储和管理待处理的消息。维护了一个消息队列，按顺序处理消息。

举个例子：

> 天气预报：子线程获取到了天气数据，然后使用`Message`将数据包装，通过`Handler`对象的`sendMessage`方法将`Message`对象插入到消息队列（`MessageQueue`）中，主线程通过`Looper.loop`方法死循环检查`MessageQueue`中是否有消息，主线程的 `Handler` 会通过 `handleMessage()` 方法接收这个 `Message`

## Handler的sendMessage

 `handler.sendMessage(message);`

![img](https://raw.githubusercontent.com/betteryuxuan/Image/main/u%3D2207774827%2C2053726473%26fm%3D253%26fmt%3Dauto%26app%3D138%26f%3DJPEG)

`sendMessageDelayed` ，`sendEmptyMessageAtTime`，`sendEmptyMessageDelayed`，`sendEmptyMessage`

这些`Handler`类中发送消息的方法最终调用的方法都是`sendMessageAtTime`。

`sendMessageAtTime`方法中再调用`enqueueMessage`插入消息到消息队列

![image-20240911201151006](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240911201151006.png)

## Message

> `Message` 类用于 `Handler` 和 `Looper` 之间的消息传递
>
> `Message`类定义一个包含描述和任意数据对象的消息，该消息可以发送给`Handler` 。此对象包含两个额外的 int 字段和一个额外的对象字段，允许您在许多情况下不进行分配。
> 虽然 `Message` 的构造函数是公共的，但获取其中一个的最佳方法是调用`Message.obtain()`或`Handler.obtainMessage()`方法之一，它们会从回收对象池中拉出它们。

### 成员变量

部分成员变量分析

```java
// what 字段是一个用户自定义的消息代码，用来标识消息的类型。
public int what;

//arg12 是一个整型值字段，用来传递简单的整型数据。
public int arg1;
public int arg2;

// obj 是一个任意的对象，允许发送者将任何类型的对象传递给接收者。
public Object obj;

// 可选的 Bundle，用于传递复杂数据。
Bundle data;

//  target 是消息最终要传递到的 Handler，负责处理该消息。
Handler target;

// 用于将 Message 对象串联在一起的指针，
Message next;
```
获取消息的首选方法是调用`Message. obtain()`

![image-20240913093007364](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240913093007364.png)

最终实际都是通过`obtain()`方法获取的`Message`对象

`obtain()`源码：

```java
// 从全局池返回一个新的 Message 实例。这允许我们在很多情况下避免分配新对象。
public static Message obtain() {
    synchronized (sPoolSync) {
        if (sPool != null) {
            Message m = sPool;
            sPool = m.next;
            m.next = null;
            m.flags = 0; // clear in-use flag
            sPoolSize--;
            return m;
        }
    }
    return new Message();
}
```

用于从全局池（`sPool`）中返回一个新的 `Message` 实例。如果池中有可用的消息对象，它将从池中获取，否则创建一个新的。可以优化性能

## MessageQueue

> `MessageQueue` 是一个用于保存消息（`Message`）的队列，其内部实现为一个单链表结构。

1. **`enqueueMessage(Message msg, long when)`**：用于将消息插入到队列的链表中，`when` 参数决定消息应该在什么时候被执行。消息按 `when` 的顺序排列，越早需要执行的消息排在前面。

2. **`next()`**：此方法用于从队列中取出下一个需要处理的消息。如果当前队列为空，线程会进入阻塞状态，直到新的消息到来。

## Looper

> 用于运行线程消息循环的类。
>
> `Looper` 是一种<u>管理消息队列</u>并<u>将消息分发到线程</u>的机制。每个线程都有自己的消息队列和消息循环，这就是由 `Looper` 来实现的。
>
> 默认情况下，线程没有与之关联的消息循环；若要创建一个消息循环，请在要运行循环的线程中调用prepare ，然后loop以让其处理消息，直到循环停止。

### 主线程自动初始化
- **主线程**（UI线程）是应用启动时的默认线程，负责处理所有与用户界面相关的任务。Android 系统在应用启动时会自动为主线程创建一个 `Looper`，因此你无需手动调用 `Looper.prepare()` 和 `Looper.loop()`。
- 这个主线程的 `Looper` 用来处理 UI 事件和系统消息，如用户点击、触摸事件等，这确保了 UI 的更新必须在主线程上执行

主线程`ActivityThread`的`main`方法中自动初始化了：

```java
public static void main(String[] args) {
    ...

    // 为主线程准备消息循环，创建与当前线程关联的Looper，并设置为主线程的Looper
    Looper.prepareMainLooper();

    ...
        
    // 初始化主线程的 Handler，用于处理消息队列中的消息
    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();
    }

    ...
        
    // 启动消息循环，开始处理消息队列中的消息，这一步将一直运行，直到应用程序终止
    Looper.loop();

    // 如果消息循环意外退出，抛出异常。正常情况下，主线程的消息循环不应该终止
    throw new RuntimeException("Main thread loop unexpectedly exited");
}
```

### 子线程手动创建
- 当你在 **子线程** 中需要处理消息时（例如子线程需要执行长时间运行的任务，并且希望在任务完成后更新 UI），你需要手动为这个子线程创建 `Looper`。步骤通常包括：
  1. 调用 `Looper.prepare()`：为当前线程创建一个 `Looper` 实例。
  2. 创建 `Handler`
  3. 调用 `Looper.loop()`：进入消息循环，不断从消息队列中获取消息并分发给相应的 `Handler` 处理。
  
  **示例**：
  ```java
  class LooperThread extends Thread {
         public Handler mHandler;
   
         public void run() {
             Looper.prepare();
   
             mHandler = new Handler(Looper.myLooper()) {
                 public void handleMessage(Message msg) {
                     // process incoming messages here
                 }
             };
   
             Looper.loop();
         }
  }
  ```

### **prepare**

> 作用：
>
> 在当前线程中创建一个 `Looper`，以便线程可以拥有一个消息循环（`MessageQueue`）。通常在子线程中需要手动调用，主线程已经自动调用了 `Looper.prepare()`

> 由于Looper构造方法被私有了，只能通过调用 `prepare()` 方法

![image-20240911213416670](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240911213416670.png)

```java
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
```

这里的sThreadLocal是一个map类型容器，可以保证了1个线程中只有1个对应的Looper

> 1个线程中可以有多个Handler，1个线程中只有1个对应的Looper

如果已经存在Looper就抛出异常，否则new一个`Looper`放入`sThreadLocal`中

![image-20240911213829044](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240911213829044.png)

### loop

- Looper负责管理线程的消息循环，它在`loop()`方法中不断从MessageQueue中取出消息，并通过Handler的`dispatchMessage()`方法将消息分发给对应的Handler进行处理。

```java
  public static void loop() {
        // 获取当前线程的looper对象
        final Looper me = myLooper();
        // 防止没有在该方法调用前调用prepare方法
        if (me == null) {
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
        }
      
        ...
            
        // 使用死循环来遍历消息队列，挑出需要执行的 Message 并分发
        for (;;) {
            if (!loopOnce(me, ident, thresholdOverride)) {
                return;
            }
        }
    } 
```

* 在Looper的循环过程中，取出的每个Message对象都会通过`Handler`的`dispatchMessage()`方法传递给目标Handler，最终由目标Handler的`handleMessage()`方法处理消息。

```java
private static boolean loopOnce(final Looper me,
        final long ident, final int thresholdOverride) {
    
    // 从消息队列中获取下一个消息。如果队列中没有消息，next() 可能会阻塞。
    // 当消息队列为空（即没有消息时），返回 null，表示消息队列正在退出。
    Message msg = me.mQueue.next(); 
    if (msg == null) {
        // 如果消息为空，表示消息队列正在退出，终止循环。
        return false;
    }

    ...
    
    try {
        // msg.target是指向一个Handler对象的引用，即消息的接收者
        // 分发消息到Handler进行处理，实际处理逻辑在msg.target.dispatchMessage()中。
        msg.target.dispatchMessage(msg);
        ...
    } catch (Exception exception) {
        ...
    } finally {
        ...
    }

    ...

    // 回收 Message 对象，避免对象的重复创建，提升性能。
    msg.recycleUnchecked();

    // 返回 true，表示循环可以继续处理下一个消息。
    return true;
}
```

## Handler的dispatchMessage

没有使用post方法的话，调用重写的`handleMessage`方法

```java
//  处理系统消息的方法。
public void dispatchMessage(@NonNull Message msg) {
    // 检查消息是否包含回调函数
    if (msg.callback != null) {
        // 如果有回调函数，处理回调
        handleCallback(msg);
    } else {
        // 如果没有回调函数
        // 检查是否设置了自定义的回调处理器
        if (mCallback != null) {
            // 如果自定义回调处理器处理了消息，直接返回
            if (mCallback.handleMessage(msg)) {
                return;
            }
        }
        // 如果自定义回调处理器没有处理消息，则调用默认的消息处理方法
        handleMessage(msg);
    }
}
```





# 法二工作原理

## Handler的post

```java
handler.post(new Runnable() {
    @Override
    public void run() {
        
    }
});
```

**post方法**，一般以匿名内部类的形式发送`Runnable`对象，在`Runnable`对象重写的`run()`方法中直接对UI进行更新

![image-20240913175006276](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240913175006276.png)

---



> post类方法用以发送Runnable对象，send类方法用以发送Message对象，二者并不矛盾也不重复

观察`Message`类，可以发现安卓把Runnable对象作为Message的成员变量

![image-20240913175515238](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240913175515238.png)

---

```java
 sendMessageDelayed(getPostMessage(r), 0)
```

`post`方法又调用了`sendMessageDelayed`，注意**`getPostMessage(r)`**，在这个方法里就把我们的Runnable对象赋值到了`Runnable`类型的`callback`上了

```java
private static Message getPostMessage(Runnable r) {
    // 获取一个 Message 实例
    Message m = Message.obtain();
    
    // 将传入的 Runnable 对象设置为 Message 的回调函数。
    m.callback = r;
    
    // 返回配置好的 Message 对象。
    return m;
}
```

## Handler的dispatchMessage

```java
public void dispatchMessage(@NonNull Message msg) {
    if (msg.callback != null) {
        // 如果消息包含 callback，调用 handleCallback 方法
        handleCallback(msg);
    } else {
        if (mCallback != null) {
            if (mCallback.handleMessage(msg)) {
                return; // 自定义回调处理器处理了消息
            }
        }
        handleMessage(msg); // 默认处理消息
    }
}
```

发现该消息有`Runnable`就调用`handleCallback`方法执行 `Runnable` 对象。

```java
    private static void handleCallback(Message message) {
        message.callback.run();
    }
```

其他部分与法一原理一致

---

---

==***感谢您的阅读
如有错误烦请指正***==

---



> 参考：
>
> 1. [Handler类中发送消息-post()和postDelay()方法精炼详解-CSDN博客](https://blog.csdn.net/weixin_41101173/article/details/79701832?ops_request_misc={"request_id"%3A"C6DC4AAC-9917-4E0F-9958-1B3D56596F76"%2C"scm"%3A"20140713.130102334.."}&request_id=C6DC4AAC-9917-4E0F-9958-1B3D56596F76&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-5-79701832-null-null.142^v100^pc_search_result_base4&utm_term=handler post&spm=1018.2226.3001.4187)
> 2. [android Handler机制原理解析（一篇就够，包你形象而深刻）_android handler的机制和原理-CSDN博客](https://blog.csdn.net/luoyingxing/article/details/86500542)
> 3. [Handler 都没搞懂，拿什么去跳槽啊？0. 前言 做 Android 开发肯定离不开跟 Handler 打交道，它通 - 掘金 (juejin.cn)](https://juejin.cn/post/6844903783139393550)
> 4. [Android 消息机制（Handler + MessageQueue + Looper） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/147783467)
> 5. [Android——Handler一篇就看懂_android handler-CSDN博客](https://blog.csdn.net/qq_31992051/article/details/131069223?ops_request_misc=%7B%22request%5Fid%22%3A%223AF017E0-8D43-4784-A5F2-AF7DD9FB5DB7%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=3AF017E0-8D43-4784-A5F2-AF7DD9FB5DB7&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-131069223-null-null.142^v100^pc_search_result_base4&utm_term=handler&spm=1018.2226.3001.4187)

