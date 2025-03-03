# 介绍

![img](https://github.com/greenrobot/EventBus/raw/master/EventBus-Publish-Subscribe.png)

## 优点

- 简化组件之间的通信
  - 解耦事件发送者和接收者
  - 在 Activity、Fragment 和后台线程中表现良好
  - 避免复杂且容易出错的依赖关系和生命周期问题
-  让你的代码更简单
-  很快，很小
- 具有高级功能，如交付线程、订阅者优先级等。



## 基本用法

导入依赖

```groovy
implementation "org.greenrobot:eventbus:3.3.1"
```

1.  定义事件：

   ```java
   public static class MessageEvent { /* Additional fields if needed */ }
   ```

2. 准备订阅者：声明并注释您的订阅方法，可以选择指定线程模式

   ```java
   @Subscribe(threadMode = ThreadMode.MAIN)  
   public void onMessageEvent(MessageEvent event) {
       // Do something
   }
   ```

   注册和取消注册您的订户。例如在 Android 上，活动和片段通常应根据其生命周期进行注册：

   ```java
    @Override
    public void onStart() {
        super.onStart();
        EventBus.getDefault().register(this);
    }
   
    @Override
    public void onStop() {
        super.onStop();
        EventBus.getDefault().unregister(this);
    }
   ```

3.  发布活动：

   ```java
    EventBus.getDefault().post(new MessageEvent());
   ```

## 线程模式

### POSTING

- **特点**：订阅者在发布事件的同一线程中被调用。
- **优点**：开销最小，避免了线程切换。
- **适用场景**：已知任务简单且快速完成，不依赖主线程。
- **注意**：长时间任务可能阻塞发布线程（如是主线程，会导致UI卡顿）。

```java
@Subscribe(threadMode = ThreadMode.POSTING)
public void onMessage(MessageEvent event) {
    log(event.message); // 快速返回的简单任务
}
```

### MAIN

- 特点：

  * 订阅者在主线程（UI线程）中被调用

  - 如果发布线程为主线程，则同步调用（与 `POSTING` 类似）

- **适用场景**：UI更新或需要在主线程完成的轻量任务。

- **注意**：避免执行耗时任务，否则会阻塞主线程，导致卡顿。

```java
@Subscribe(threadMode = ThreadMode.MAIN)
public void onMessage(MessageEvent event) {
    textView.setText(event.message); // 更新UI
}
```

### MAIN_ORDERED

- 特点：
  1. 在主线程中执行。
  2. 按顺序执行：事件会一个接一个地处理，不会乱序。
- **适用场景**：依赖特定执行顺序的UI更新逻辑。
- **注意**：与 `MAIN` 类似，避免耗时任务，确保任务快速返回。

```java
@Subscribe(threadMode = ThreadMode.MAIN_ORDERED)
public void onMessageEvent(String event) {
    Log.d("EventBus", "Event received: " + event); // 按顺序更新UI
}
```

### BACKGROUND

- 特点：
  - 如果发布线程为主线程，事件处理方法会切换到后台线程。
  - 如果发布线程是非主线程，事件处理方法直接在发布线程中执行。
- **适用场景**：后台任务，如数据库存储、文件操作。
- **注意**：快速返回，避免阻塞后台线程。

```java
@Subscribe(threadMode = ThreadMode.BACKGROUND)
public void onMessage(MessageEvent event) {
    saveToDisk(event.message); // 后台存储操作
}
```

### ASYNC

- **特点**：事件处理程序始终在独立线程中调用，与发布线程或主线程完全分离。
- **适用场景**：耗时操作，如网络请求、复杂计算。
- **注意**：避免触发大量异步任务，防止线程池耗尽资源。

```java
@Subscribe(threadMode = ThreadMode.ASYNC)
public void onMessage(MessageEvent event) {
    backend.send(event.message); // 异步网络请求
}
```

## 黏性事件

发送事件之后再订阅也能收到该事件

```java
@Subscribe(threadMode = ThreadMode.POSTING, sticky = true)
public void onMessageEvent(MessageEvent messageEvent) {
    tv.setText(messageEvent.getMessage());
}
```

```java
EventBus.getDefault().postSticky(new MessageEvent("SecondActivity的信息"));
```

# 源码

## 构造

```java
public class EventBus {

    // 静态变量，存储唯一的 EventBus 实例
    // 使用 volatile 关键字，确保多线程环境下变量的可见性和防止指令重排
    static volatile EventBus defaultInstance;

    public static EventBus getDefault() {
        // 将静态变量 defaultInstance 赋值给局部变量 instance，减少对主内存的访问
        EventBus instance = defaultInstance;

        // 第一次检查，避免不必要的同步开销
        if (instance == null) {
            // 如果实例未被初始化，进入同步块
            synchronized (EventBus.class) {
                // 再次将 defaultInstance 的值赋给 instance（看这个时候defaultInstance为不为空）
                instance = EventBus.defaultInstance;

                // 第二次检查，确保实例仍未被初始化（双重检查锁定）
                if (instance == null) {
                    // 创建新的 EventBus 实例并赋值给 defaultInstance 和局部变量 instance
                    instance = EventBus.defaultInstance = new EventBus();
                }
            }
        }
        return instance;
    }
}
```

```java
public EventBus() {
    this(DEFAULT_BUILDER);
}
```

```java
private static final EventBusBuilder DEFAULT_BUILDER = new EventBusBuilder();
```

建造者模式

```java
EventBus(EventBusBuilder builder) {
    //日志
    logger = builder.getLogger();
    //这个集合可以根据事件类型获取订阅者
    //key：事件类型，value：订阅该事件的订阅者集合
    subscriptionsByEventType = new HashMap<>();
    //订阅者所订阅的事件集合
    //key：订阅者，value：该订阅者订阅的事件集合
    typesBySubscriber = new HashMap<>();
    //粘性事件集合
    //key：事件Class对象，value：事件对象
    stickyEvents = new ConcurrentHashMap<>();
    //Android主线程处理事件
    mainThreadSupport = builder.getMainThreadSupport();
    mainThreadPoster = mainThreadSupport != null ? mainThreadSupport.createPoster(this) : null;
    //Background事件发送者
    backgroundPoster = new BackgroundPoster(this);
    //异步事件发送者
    asyncPoster = new AsyncPoster(this);
    indexCount = builder.subscriberInfoIndexes != null ? builder.subscriberInfoIndexes.size() : 0;
    //订阅者订阅事件查找对象
    subscriberMethodFinder = new SubscriberMethodFinder(builder.subscriberInfoIndexes,
                                                        builder.strictMethodVerification, builder.ignoreGeneratedIndex);
    logSubscriberExceptions = builder.logSubscriberExceptions;
    logNoSubscriberMessages = builder.logNoSubscriberMessages;
    sendSubscriberExceptionEvent = builder.sendSubscriberExceptionEvent;
    sendNoSubscriberEvent = builder.sendNoSubscriberEvent;
    throwSubscriberException = builder.throwSubscriberException;
    eventInheritance = builder.eventInheritance;
    executorService = builder.executorService;
}
```

这个方法内部首先通过单例模式创建一个`EventBus`对象，在创建`EventBus`时最终会调用它的有参构造函数，传入一个`EventBus.Builder`对象。在这个有参构造函数内部对属性进行初始化

## 注册

```java
public void register(Object subscriber) {
    // 1、通过反射获取到订阅者的Class对象
    Class<?> subscriberClass = subscriber.getClass();
    // 2、获取订阅者所订阅事件的集合(1)
    List<SubscriberMethod> subscriberMethods = subscriberMethodFinder.findSubscriberMethods(subscriberClass);
    synchronized (this) {
        // 3、遍历集合进行注册
        for (SubscriberMethod subscriberMethod : subscriberMethods) {
            // 遍历订阅者的订阅方法来完成订阅者的订阅操作
            subscribe(subscriber, subscriberMethod);
        }
    }
}
```

### 查找订阅者的订阅方法

`SubscriberMethod`（订阅方法）类中，主要就是用保存订阅方法的Method对象、线程模式、事件类型、优先级、是否是粘性事件等属性

```java
public class SubscriberMethod {
    final Method method; // 处理事件的Method对象
    final ThreadMode threadMode; //线程模型
    final Class<?> eventType; //事件类型
    final int priority; //事件优先级
    final boolean sticky; //是否是粘性事件
    String methodString;
}
```

> **`eventType`**：
>
>  是一个 `Class<?>` 类型的字段，表示订阅方法（Subscriber Method）所订阅的事件的类型
>
> 假设有一个事件类：
>
> ```java
> public class UserEvent {
>     public final String userName;
> 
>     public UserEvent(String userName) {
>         this.userName = userName;
>     }
> }
> ```
>
> 订阅方法如下：
>
> ```java
> @Subscribe(threadMode = ThreadMode.MAIN)
> public void onUserEvent(UserEvent event) {
>     System.out.println("User event received: " + event.userName);
> }
> ```
>
> 当 EventBus 解析订阅方法时，会为该方法创建一个 `SubscriberMethod` 实例，其中的 `eventType` 就是 `UserEvent.class`。

**源码**：

```java
// SubscriberMethodFinder.java
List<SubscriberMethod> findSubscriberMethods(Class<?> subscriberClass) {
    // 优先从缓存中获取方法列表
    List<SubscriberMethod> subscriberMethods = METHOD_CACHE.get(subscriberClass);
    if (subscriberMethods != null) {
        return subscriberMethods;
    }

    // 表示是否忽略注解器生成的MyEventBusIndex，忽略就使用反射获取
    // ignoreGeneratedIndex 默认是false
    if (ignoreGeneratedIndex) {
        subscriberMethods = findUsingReflection(subscriberClass);
    } else {
        // 获得默认的EventBus对象的情况下一般调用这个方法 (2)
        subscriberMethods = findUsingInfo(subscriberClass);
    }
    //在获得subscriberMethods以后，如果订阅者中不存在@Subscribe注解并且为public的订阅方法，则会抛出异常。
    if (subscriberMethods.isEmpty()) {
        throw new EventBusException("Subscriber " + subscriberClass
                                    + " and its super classes have no public methods with the @Subscribe annotation");
    } else {
        // 将方法存入缓存
        METHOD_CACHE.put(subscriberClass, subscriberMethods);
        return subscriberMethods;
    }
}
```

```java
// (2)
private List<SubscriberMethod> findUsingInfo(Class<?> subscriberClass) {
    // 准备findState对象，用于保存订阅方法的Method对象，线程模式，事件类型，优先级，是否为黏性事件
    FindState findState = prepareFindState();
    // 初始化findState，指定订阅者类。
    findState.initForSubscriber(subscriberClass);

    // 遍历当前类及其所有父类。
    while (findState.clazz != null) {
        // 尝试获取当前类的预生成订阅者信息。
        findState.subscriberInfo = getSubscriberInfo(findState);

        if (findState.subscriberInfo != null) {
            // 如果找到订阅者信息对象，获取订阅方法数组。
            SubscriberMethod[] array = findState.subscriberInfo.getSubscriberMethods();
            for (SubscriberMethod subscriberMethod : array) {
                // 检查方法是否已经添加（防止重复）。
                if (findState.checkAdd(subscriberMethod.method, subscriberMethod.eventType)) {
                    // 添加方法到订阅方法列表中。
                    findState.subscriberMethods.add(subscriberMethod);
                }
            }
        } else {
            // 如果没有订阅者信息对象，则降级为反射查找。(3)
            findUsingReflectionInSingleClass(findState);
        }
        // 切换到当前类的父类继续查找。
        findState.moveToSuperclass();
    }
    // 对findstate做回收处理，并返回订阅方法的List集合
    return getMethodsAndRelease(findState);
}

// (3)
private void findUsingReflectionInSingleClass(FindState findState) {
    Method[] methods;
    try {
        // 优先使用 getDeclaredMethods，因为只获取当前类的方法，性能更高。
        methods = findState.clazz.getDeclaredMethods();
    } catch (Throwable th) {
        // 如果 getDeclaredMethods 抛出异常（如 NoClassDefFoundError），
        // 则使用 getMethods 获取当前类及其父类的公共方法。
        try {
            methods = findState.clazz.getMethods();
        } catch (LinkageError error) {
            // 如果获取方法失败，抛出异常提示开发者使用注解处理器（更高效）。
            String msg = "Could not inspect methods of " + findState.clazz.getName();
            if (ignoreGeneratedIndex) {
                msg += ". Please consider using EventBus annotation processor to avoid reflection.";
            } else {
                msg += ". Please make this class visible to EventBus annotation processor to avoid reflection.";
            }
            throw new EventBusException(msg, error);
        }
        // 如果方法获取失败，跳过当前类的父类检查。
        findState.skipSuperClasses = true;
    }

    // 遍历当前类的所有方法。
    for (Method method : methods) {
        int modifiers = method.getModifiers();

        // 检查方法是否符合以下条件：
        // 1. 必须是 public。
        // 2. 不能是 static 或 abstract。
        if ((modifiers & Modifier.PUBLIC) != 0 && (modifiers & MODIFIERS_IGNORE) == 0) {
            // 检查方法的参数，必须只有一个参数。
            Class<?>[] parameterTypes = method.getParameterTypes();
            if (parameterTypes.length == 1) {
                // 检查是否标注了 @Subscribe 注解。
                Subscribe subscribeAnnotation = method.getAnnotation(Subscribe.class);
                if (subscribeAnnotation != null) {
                    // 提取事件类型（参数类型）和注解信息。
                    Class<?> eventType = parameterTypes[0];
                    // 检查并添加到订阅方法列表中（防止重复）。
                    if (findState.checkAdd(method, eventType)) {
                        // 创建 SubscriberMethod 并保存。
                        ThreadMode threadMode = subscribeAnnotation.threadMode();
                        findState.subscriberMethods.add(new SubscriberMethod(
                            method, eventType, threadMode, 
                            subscribeAnnotation.priority(), 
                            subscribeAnnotation.sticky()
                        ));
                    }
                }
            } else if (strictMethodVerification && method.isAnnotationPresent(Subscribe.class)) {
                // 如果参数数量不符合要求，抛出异常。
                String methodName = method.getDeclaringClass().getName() + "." + method.getName();
                throw new EventBusException("@Subscribe method " + methodName +
                                            " must have exactly 1 parameter but has " + parameterTypes.length);
            }
        } else if (strictMethodVerification && method.isAnnotationPresent(Subscribe.class)) {
            // 如果方法不是 public 或者是 static/abstract，抛出异常。
            String methodName = method.getDeclaringClass().getName() + "." + method.getName();
            throw new EventBusException(methodName +
                                        " is a illegal @Subscribe method: must be public, non-static, and non-abstract");
        }
    }
}
```

### 订阅者的注册

> 查找完所有的订阅方法后对所有的订阅方法进行注册

#### Subscription

`Subscription`表示一个事件订阅关系。

封装了某个 **订阅者**（`subscriber`）和其订阅的 **方法**（`subscriberMethod`）之间的关联。

```java
final class Subscription {
    final Object subscriber;
    final SubscriberMethod subscriberMethod;
    // 标识某个订阅者是否仍然处于有效状态
    volatile boolean active;

    Subscription(Object subscriber, SubscriberMethod subscriberMethod) {
        this.subscriber = subscriber;
        this.subscriberMethod = subscriberMethod;
        active = true;
    }
	//...
}
```

如果 `EventBus.unregister(Object)` 被调用，`active` 将被设置为 `false`

#### subscribe


订阅信息被保存在两个 `Map` 中：

- `subscriptionsByEventType`：将事件类型（类类型）映射到订阅该事件类型的所有 `Subscription`（订阅者和订阅方法的封装）。
- `typesBySubscriber`：将订阅者映射到它所订阅的所有事件类型。

> **`subscriptionsByEventType`是我们投递订阅事件的时候，就是根据我们的`EventType`找到我们的订阅事件，从而去分发事件，处理事件的**
>
> **`typesBySubscriber`是在调用`unregister(this)`的时候，根据订阅者找到我们的`EventType`，又根据我们的`EventType`找到订阅事件，从而解绑用的**

```java
public class EventBus {
    private final Map<Class<?>, CopyOnWriteArrayList<Subscription>> subscriptionsByEventType;
    private final Map<Object, List<Class<?>>> typesBySubscriber;
}
```

```java
private void subscribe(Object subscriber, SubscriberMethod subscriberMethod) {
    // 4、获取事件类，就是自定义的MessageEvent事件类
    Class<?> eventType = subscriberMethod.eventType;
    // 5、根据订阅者和订阅方法构造一个订阅事件
    Subscription newSubscription = new Subscription(subscriber, subscriberMethod);
    // 6、获取当前特定类型的事件的所有Subscription对象（所有订阅该事件类型的方法）的List集合
    CopyOnWriteArrayList<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
    // 7、该事件对应的Subscription的List集合不存在，则重新创建并保存在subscriptionsByEventType中
    if (subscriptions == null) {
        // 创建集合，存入subscriptionsByEventType集合中
        subscriptions = new CopyOnWriteArrayList<>();
        subscriptionsByEventType.put(eventType, subscriptions);
    } else { 
        // 8、如果有订阅者已经订阅了该事件
        // 判断这些订阅者中是否有重复订阅的现象
        if (subscriptions.contains(newSubscription)) {
            throw new EventBusException("Subscriber " + subscriber.getClass() + " already registered to event "
                                        + eventType);
        }
    }
    int size = subscriptions.size();
    // 9、遍历该事件的所有订阅者，将当前订阅插入合适位置
    for (int i = 0; i <= size; i++) {
        // 按照优先级高低进行插入，如果优先级最低，插入到集合尾部
        if (i == size || subscriberMethod.priority > subscriptions.get(i).subscriberMethod.priority) {
            subscriptions.add(i, newSubscription);
            break;
        }
    }

    // 10、获取该事件订阅者订阅的所有事件类的集合
    // 这里是为了unregister的时候，通过typesBySubscriber这个map获取到该订阅者订阅的事件类
    // 再遍历所有的事件类，从subscriptionsByEventType这个map拿到每一个事件类存储subscription的list，移除list中是该订阅者的subscription
    List<Class<?>> subscribedEvents = typesBySubscriber.get(subscriber);
    if (subscribedEvents == null) {
        subscribedEvents = new ArrayList<>();
        typesBySubscriber.put(subscriber, subscribedEvents);
    }
    // 11、将该事件加入到subscribedEvents集合中
    subscribedEvents.add(eventType);

    // 12、判断该事件是否是粘性事件
    if (subscriberMethod.sticky) {
        if (eventInheritance) { 
            // 13、获取所有粘性事件并遍历，判断继承关系,默认是不可继承
            Set<Map.Entry<Class<?>, Object>> entries = stickyEvents.entrySet();
            for (Map.Entry<Class<?>, Object> entry : entries) {
                Class<?> candidateEventType = entry.getKey();
                if (eventType.isAssignableFrom(candidateEventType)) {
                    Object stickyEvent = entry.getValue();
                    // 14、调用该方法post
                    checkPostStickyEventToSubscription(newSubscription, stickyEvent);
                }
            }
        } else {
            Object stickyEvent = stickyEvents.get(eventType);
            checkPostStickyEventToSubscription(newSubscription, stickyEvent);
        }
    }
}

private void checkPostStickyEventToSubscription(Subscription newSubscription, Object stickyEvent) {
    if (stickyEvent != null) {
        postToSubscription(newSubscription, stickyEvent, isMainThread());
    }
}
```

> 1. 将我们的订阅方法和订阅者封装到`subscriptionsByEventType`和`typesBySubscriber`中
>
>    `subscriptionsByEventType`是我们投递订阅事件的时候，根据我们发送的事件类型找到我们的订阅事件，从而去分发处理事件的
>
>    `typesBySubscriber`在调用`unregister(this)`的时候，根据订阅者找到EventType，又根据EventType找到订阅事件，从而对订阅者进行解绑。
>
> 2. 如果是粘性事件的话，就立马投递、执行。

### 小结

1. 根据单例设计模式创建一个`EventBus`对象，同时创建一个`EventBus.Builder`对象对`EventBus`进行初始化，其中有三个比较重要的集合和一个`SubscriberMethodFinder`对象。
2. 调用`register`方法,首先通过反射获取到订阅者的`Class`对象。
3. 通过`SubscriberMethodFinder`对象获取订阅者中所有订阅的事件集合,它先从缓存中获取，如果缓存中有，直接返回；如果缓存中没有，通过反射的方式去遍历订阅者内部被注解的方法，将这些方法放入到集合中进行返回。
4. 遍历第三步获取的集合，将订阅者和事件进行绑定。
5. 在绑定之后会判断绑定的事件是否是粘性事件，如果是粘性事件，直接调用`postToSubscription`方法，将之前发送的粘性事件发送给订阅者。

## 发送

* `PostingThreadState`：`Eventbus`内部类，存储了事件队列和线程状态信息

```java
// 线程中存储数据
final static class PostingThreadState {
    final List<Object> eventQueue = new ArrayList<>(); // 线程的事件队列
    boolean isPosting; //是否正在发送中
    boolean isMainThread; //是否在主线程中发送
    Subscription subscription; //事件订阅者和订阅事件的封装
    Object event; //事件对象
    boolean canceled; //是否被取消发送
}
```

* `currentPostingThreadState` 是一个 `ThreadLocal` 对象，用于为每个线程提供一个独立的 `PostingThreadState` 实例。避免线程间的相互干扰。
* `ThreadLocal`是一个线程内部的数据存储类，通过它可以在指定的线程中存储数据， 并且线程之间的数据是相互独立的。内部通过创建一个它包裹的泛型对象的数组，不同的线程对应不同的数组索引，每个线程通过`get`方法获取对应的线程数据。

```java
private final ThreadLocal<PostingThreadState> currentPostingThreadState = new ThreadLocal<PostingThreadState>() {
    @Override
    // ThreadLocal为每个线程第一次访问currentPostingThreadState提供一个新的PostingThreadState
    protected PostingThreadState initialValue() {
        return new PostingThreadState();
    }
};
```

**源码解析**：

```java
public void post(Object event) {
    // 1、获取当前线程
    PostingThreadState postingState = currentPostingThreadState.get();
    // 2、当前线程的事件集合
    List<Object> eventQueue = postingState.eventQueue;
    // 3、将要发送的事件加入事件队列
    eventQueue.add(event);

    // 查看是否正在发送事件
    if (!postingState.isPosting) {
        // 判断是否是主线程
        postingState.isMainThread = isMainThread();
        postingState.isPosting = true;
        if (postingState.canceled) {
            throw new EventBusException("Internal error. Abort state was not reset");
        }
        try {
            // 4、处理事件队列中所有事件
            while (!eventQueue.isEmpty()) {
                postSingleEvent(eventQueue.remove(0), postingState);
            }
        } finally {
            postingState.isPosting = false;
            postingState.isMainThread = false;
        }
    }
}
```

1. 从`PostingThreadState`对象中取出事件队列
2. 将当前的事件插入到事件队列当中
3. 将队列中的事件依次交由`postSingleEvent`方法进行处理，并移除该事件。

```java
private void postSingleEvent(Object event, PostingThreadState postingState) throws Error {
    // 5. 获取事件的Class对象
    Class<?> eventClass = event.getClass();
    boolean subscriptionFound = false;
    // eventInheritance表示是否向上查找事件的父类,默认为true
    if (eventInheritance) {
        // 6. 获取事件类的所有相关类型（包括父类和接口）
        List<Class<?>> eventTypes = lookupAllEventTypes(eventClass);
        int countTypes = eventTypes.size();
        for (int h = 0; h < countTypes; h++) {
            Class<?> clazz = eventTypes.get(h);
            // 7、遍历集合发送单个事件
            subscriptionFound |= postSingleEventForEventType(event, postingState, clazz);
        }
    } else {
        subscriptionFound = postSingleEventForEventType(event, postingState, eventClass);
    }
    ...
}
```

```java
private boolean postSingleEventForEventType(Object event, PostingThreadState postingState, Class<?> eventClass) {
    CopyOnWriteArrayList<Subscription> subscriptions;
    synchronized (this) {
        // 8、根据事件获取所有订阅它的订阅者
        subscriptions = subscriptionsByEventType.get(eventClass);
    }
    if (subscriptions != null && !subscriptions.isEmpty()) {
        // 9、遍历集合
        for (Subscription subscription : subscriptions) {
            // 将该事件的event和对应的Subscription中的信息（包扩订阅者类和订阅方法）传递给postingState
            postingState.event = event;
            postingState.subscription = subscription;
            boolean aborted = false;
            try {
                // 10、将事件发送给订阅者
                postToSubscription(subscription, event, postingState.isMainThread);
                aborted = postingState.canceled;
            } finally {
                // 11、重置postingState
                postingState.event = null;
                postingState.subscription = null;
                postingState.canceled = false;
            }
            if (aborted) {
                break;
            }
        }
        return true;
    }
    return false;
}
```

```java
private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
    // 12、根据订阅方法的线程模式调用订阅方法
    switch (subscription.subscriberMethod.threadMode) {
        // 默认类型，表示发送事件操作直接调用订阅者的响应方法，不需要进行线程间的切换
        case POSTING: 
            invokeSubscriber(subscription, event);
            break;
        // 主线程，表示订阅者的响应方法在主线程进行接收事件
        case MAIN: 
            // 如果发送者在主线程，直接调用订阅者的响应方法
            if (isMainThread) { 
                invokeSubscriber(subscription, event);
            //如果事件的发送者不是主线程，添加到mainThreadPoster的队列中去，在主线程中调用响应方法
            } else {
                mainThreadPoster.enqueue(subscription, event); 
            }
            break;
        // 主线程优先模式
        case MAIN_ORDERED:
            if (mainThreadPoster != null) {
                mainThreadPoster.enqueue(subscription, event);
            } else {
                //如果不是主线程就在消息发送者的线程中进行调用响应方法
                invokeSubscriber(subscription, event);
            }
            break;
        case BACKGROUND:
            if (isMainThread) {
                // 如果事件发送者在主线程，加入到backgroundPoster的队列中，在线程池中调用响应方法
                backgroundPoster.enqueue(subscription, event);
            } else {
                // 如果不是主线程，在事件发送者所在的线程调用响应方法
                invokeSubscriber(subscription, event);
            }
            break;
        case ASYNC:
            //这里没有进行线程的判断，也就是说不管是不是在主线程中，都会在子线程中调用响应方法
            asyncPoster.enqueue(subscription, event);
            break;
        default:
            throw new IllegalStateException("Unknown thread mode: " + subscription.subscriberMethod.threadMode);
    }
}
```

### 小结

1. 获取当前线程的事件集合，将要发送的事件加入到集合中。
2. 通过循环，只要事件集合中还有事件，就一直发送。
3. 获取事件的`Class`对象，找到当前的`event`的所有父类和实现的接口的`class`集合。遍历这个集合，调用发送单个事件的方法进行发送。
4. 根据事件获取所有订阅它的订阅者集合，遍历集合，将事件发送给订阅者。
5. 发送给订阅者时，根据订阅方法的线程模式调用订阅方法，如果需要线程切换，则切换线程进行调用；否则，直接调用。

![VnQkQI](https://raw.githubusercontent.com/betteryuxuan/Image/main/VnQkQI.png)

### 黏性事件

`EventBus.getDefault().postSticky(Object event)`

```java
public void postSticky(Object event) {
    synchronized (stickyEvents) {
        // 1、将事件添加到粘性事件集合中
        stickyEvents.put(event.getClass(), event);
    }
    // 2、发送事件
    post(event);
}
```

> 1. 将粘性事件加入到`EventBus`对象的粘性事件集合中，当有新的订阅者进入后，如果该订阅者订阅了该粘性事件，可以直接发送给订阅者。
> 2. 将粘性事件发送给已有的事件订阅者。

## 解注册

`EventBus.getDefault().unregister(Object subscriber)`

```java
public synchronized void unregister(Object subscriber) {
    // 1、获取订阅者订阅的所有事件类型
    List<Class<?>> subscribedTypes = typesBySubscriber.get(subscriber);
    if (subscribedTypes != null) {
        // 2、遍历事件类型集合
        for (Class<?> eventType : subscribedTypes) {
            // 3、遍历该事件类型的所有订阅，删除掉该订阅者的订阅
            unsubscribeByEventType(subscriber, eventType);
        }
        // 4、将订阅者从事件类型集合中移除
        typesBySubscriber.remove(subscriber);
    } else {
        logger.log(Level.WARNING, "Subscriber to unregister was not registered before: " + subscriber.getClass());
    }
}
```

```java
private void unsubscribeByEventType(Object subscriber, Class<?> eventType) {
    // 获取该事件的所有订阅者
    List<Subscription> subscriptions = subscriptionsByEventType.get(eventType);
    if (subscriptions != null) {
        int size = subscriptions.size();
        // 遍历集合
        for (int i = 0; i < size; i++) {
            Subscription subscription = subscriptions.get(i);
            // 将订阅者从集合中移除
            if (subscription.subscriber == subscriber) {
                subscription.active = false;
                subscriptions.remove(i);
                i--;
                size--;
            }
        }
    }
}
```

![VnQPWd](https://raw.githubusercontent.com/betteryuxuan/Image/main/VnQPWd.png)

## MAIN模式

```java
private void postToSubscription(Subscription subscription, Object event, boolean isMainThread) {
    switch (subscription.subscriberMethod.threadMode) {
        case MAIN:
            if (isMainThread) {
                invokeSubscriber(subscription, event);
            } else {
                mainThreadPoster.enqueue(subscription, event);
            }
            break;
            ...
    }
}
```

主要分析一下`MAIN`模式

> 如果是主线程，通过反射直接运行订阅的方法
>
> 如果不是主线程，需要`mainThreadPoster`将订阅事件入队列

### invokeSubscriber

```java
void invokeSubscriber(Subscription subscription, Object event) {
    try {
        // 通过反射调用订阅者方法，将事件对象作为参数传递
        subscription.subscriberMethod.method.invoke(subscription.subscriber, event);
    } catch (InvocationTargetException e) {
        handleSubscriberException(subscription, event, e.getCause());
    } catch (IllegalAccessException e) {
        throw new IllegalStateException("Unexpected exception", e);
    }
}
```

### enqueue

`mainThreadPoster.enqueue`

`mainThreadPoster`是`EventBus`进行初始化时创建的

```java
private final MainThreadSupport mainThreadSupport;
private final Poster mainThreadPoster;
EventBus(EventBusBuilder builder) {
    // 1、创建mainThreadSupport
    mainThreadSupport = builder.getMainThreadSupport();
    // 2、创建mainThreadPoster
    mainThreadPoster = mainThreadSupport != null ? mainThreadSupport.createPoster(this) : null;
}
```

这里创建了两个对象：`mainThreadSupport`和`mainThreadPoster`，我们先来看一下`mainThreadSupport`是什么。

```java
MainThreadSupport getMainThreadSupport() {
    if (mainThreadSupport != null) {
        return mainThreadSupport;
    } else if (Logger.AndroidLogger.isAndroidLogAvailable()) {
        // 获取主线程Looper对象，这里又可能为空
        Object looperOrNull = getAndroidMainLooperOrNull();
        // 根据主线程Looper对象返回AndroidHandlerMainThreadSupport对象
        return looperOrNull == null ? null :
        new MainThreadSupport.AndroidHandlerMainThreadSupport((Looper) looperOrNull);
    } else {
        return null;
    }
}

Object getAndroidMainLooperOrNull() {
    try {
        // 返回主线程Looper
        return Looper.getMainLooper();
    } catch (RuntimeException e) {
        return null;
    }
}

public interface MainThreadSupport {
    boolean isMainThread();
    Poster createPoster(EventBus eventBus);

    class AndroidHandlerMainThreadSupport implements MainThreadSupport {
        private final Looper looper;
        public AndroidHandlerMainThreadSupport(Looper looper) {
            // 根据外部传入的looper对象进行本地初始化
            this.looper = looper;
        }

        @Override
        public boolean isMainThread() {
            // 判断是否为主线程
            return looper == Looper.myLooper();
        }

        @Override
        public Poster createPoster(EventBus eventBus) {
            // 当调用createPoster方法时创建一个HandlerPoster对象
            return new HandlerPoster(eventBus, looper, 10);
        }
    }
}
```

我们看到创建`mainThreadSupport`对象实际上是根据主线程的`Looper`对象来创建的，`MainThreadSupport`实际上是一个接口，所以这里返回的是它的实现类`AndroidHandlerMainThreadSupport`对象。
 接下来让我们看一下`mainThreadPoster`对象，在创建`mainThreadPoster`对象时，调用了`mainThreadSupport.createPoster`方法，由于`MainThreadSupport`是一个接口，所以实际上是调用了它的实现类对象的`createPoster`方法，在方法的内部创建了一个`HandlerPoster`对象并返回，我们看一下`HandlerPoster`。

```java
//这个类继承自Handler并且实现了Poster接口
public class HandlerPoster extends Handler implements Poster {

    private final PendingPostQueue queue;
    private final int maxMillisInsideHandleMessage;
    private final EventBus eventBus;
    private boolean handlerActive;

    protected HandlerPoster(EventBus eventBus, Looper looper, int maxMillisInsideHandleMessage) {
        //这里传入的是主线程的Looper对象，所以这个Handler对象是主线程的Handler
        super(looper);
        this.eventBus = eventBus;
        this.maxMillisInsideHandleMessage = maxMillisInsideHandleMessage;
        //创建一个事件队列
        queue = new PendingPostQueue();
    }

    public void enqueue(Subscription subscription, Object event) {
        //根据传入的参数封装一个PendingPost对象
        PendingPost pendingPost = PendingPost.obtainPendingPost(subscription, event);
        synchronized (this) {
            //将PendingPost加入到队列中
            queue.enqueue(pendingPost);
            if (!handlerActive) {
                handlerActive = true;
                // 调用sendMessage，发送事件回到主线程
                // 最终会调用下面的handleMessage方法
                if (!sendMessage(obtainMessage())) {
                    throw new EventBusException("Could not send handler message");
                }
            }
        }
    }

    @Override
    public void handleMessage(Message msg) {
        boolean rescheduled = false;
        try {
            long started = SystemClock.uptimeMillis();
            //无限循环
            while (true) {
                PendingPost pendingPost = queue.poll();
                //进行两次检查，确保pendingPost不为null，如果为null直接跳出循环
                if (pendingPost == null) {
                    synchronized (this) {
                        pendingPost = queue.poll();
                        if (pendingPost == null) {
                            handlerActive = false;
                            return;
                        }
                    }
                }
                //使用反射的方法调用订阅者的订阅方法。
                eventBus.invokeSubscriber(pendingPost);
                long timeInMethod = SystemClock.uptimeMillis() - started;
                if (timeInMethod >= maxMillisInsideHandleMessage) {
                    if (!sendMessage(obtainMessage())) {
                        throw new EventBusException("Could not send handler message");
                    }
                    rescheduled = true;
                    return;
                }
            }
        } finally {
            handlerActive = rescheduled;
        }
    }
}
```



---



> 参考：
>
> 1. [greenrobot/EventBus(github.com)](https://github.com/greenrobot/EventBus#add-eventbus-to-your-project)
> 2. [EventBus源码解析 - 掘金 (juejin.cn)](https://juejin.cn/post/6844904007199113229#heading-20)
> 3. [EventBus3.0 源码解析_evenbus3.0在源码mk编译引入注解-CSDN博客](https://blog.csdn.net/yanghuinipurean/article/details/51646819)
> 4. [Android事件总线（二）EventBus3.0源码解析 | BATcoder - 刘望舒](https://liuwangshu.cn/application/eventbus/2-eventbus-sourcecode.html)



