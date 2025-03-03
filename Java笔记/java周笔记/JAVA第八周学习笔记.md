# 网络编程

> 在网络通信协议下，不同计算机上运行的程序，进行的数据传输。
>
> 应用场景：即时通信、网游对战、金融证券、国际贸易、邮件、等等，
>
> Java中可以使用`Java.net`包下的技术轻松开发出常见的网络应用程序。

## 软件架构

1. **客户端-服务器架构（Client-Server Architecture）**

客户端服务端模式需要开发客户端

适合定制专业化的办公类软件如:IDEA、网游

2. **浏览器-服务器架构（Browser-Server Architecture）**

浏览器服务端模式不需要开发客户端。

适合移动互联网应用，可以在任何地方随时访问的系统

## 三要素

三次握手协议保证连接建立

四次挥手
利用这个协议断开连接
而且保证连接通道里面的数据已经处理完毕了

## **IP**

>  设备在网络中的地址，是唯一的标识。

Internet Protocol（互联网协议）

是分配给上网设备的数字标签，也被称为IP地址。它是一种网络协议，用于在互联网上标识和定位设备，使得数据能够在网络中正确地传输和路由。

通俗地说，IP地址就像是上网设备在网络中的地址，它是唯一的，就像是我们现实生活中的门牌号一样，用于准确定位和识别设备。

**1>常见的IP地址分类**

1. IPv4（Internet Protocol version 4）：IPv4是目前广泛使用的IP地址标准，它使用32位地址，通常以四个十进制数表示，每个数的取值范围是0到255
   - IPv4：地址通常采用**“点分十进制”**表示，例如`192.168.1.1`
2. IPv6（Internet Protocol version 6）：使用128位地址，采用了更加复杂的表示方式，通常以八组十六进制数表示
   - IPv6：地址使用**“冒号分16进制”**的方式进行表示，例如`2001:0db8:85a3:0000:0000:8a2e:0370:7334`

**2>IPv4**

公网地址（万维网使用)和私有地址（局域网使用）。

192.168.开头的就是私有址址，范围即为192.168.0.0--192.168.255.255，专门为组织机构内部使用，以此节省IP

**3>特殊IP地址**

`127.0.0.1`，也可以是localhost:是回送地址也称本地回环地址，也称本机IP，永远只会寻找当前所在本机。

**4>InetAddress**

`InetAddress` 是 Java 中用于表示 IP 地址的类。它可以表示 IPv4 地址或 IPv6 地址，并提供了一系列方法用于获取主机名、IP 地址等信息，以及进行网络通信时 IP 地址的解析和操作。

```java
package com.feng.MyInetAddressDemo1;

import java.net.InetAddress;
import java.net.UnknownHostException;

public class MyInetAddressDemo1 {
    public static void main(String[] args) throws UnknownHostException {
        //1.获取InetAdderss对象
        // 获取本地主机的 InetAddress 对象
        InetAddress address1 = InetAddress.getLocalHost();
        System.out.println("本地主机信息：" + address1);
        System.out.println("本地主机名：" + address1.getHostName());
        System.out.println("本地 IP 地址：" + address1.getHostAddress());

        // 根据主机名获取 InetAddress 对象
        InetAddress address2 = InetAddress.getByName("xuanlaptop");
        System.out.println("指定主机信息：" + address2);
        System.out.println("主机名：" + address2.getHostName());
        System.out.println("IP 地址：" + address2.getHostAddress());
    }
}
```

## **端口号**

> 应用程序在设备中唯一的标识

端口号是应用程序在设备中的唯一标识，用于区分同一设备上的不同网络应用程序。以下是对端口号的详细解释：

**1.定义**

端口号是网络通信中的一个重要概念，它用于标识设备上运行的应用程序。每个网络应用程序通过一个特定的端口号来接收和发送数据。

**2.范围**

端口号由两个字节表示，取值范围是0到65535。

**3.分类**

- **0~1023**：这些端口号被称为“知名端口号”或“系统端口号”，用于一些著名的网络服务和应用，例如：
  - HTTP：80
  - HTTPS：443
  - FTP：21
  - SSH：22
  - SMTP：25
- **1024~49151**：这些端口号被称为“注册端口号”，可以由用户或公司注册使用，但需要遵循相关规范。
- **49152~65535**：这些端口号被称为“动态端口号”或“私有端口号”，通常用于临时性或私有应用，适合用户自行分配。

**4.使用规则**

- 一个端口号只能被一个应用程序使用。同一时间一个端口号不能被多个应用程序同时占用。
- 为了避免冲突，用户和开发者应当选择1024以上的端口号来开发和部署自己的应用程序。

## **协议**

> 数据在网络中传输的规则，常见的协议有UDP、TCP、http、https、ftp。

计算机网络中，连接和通信的规则被称为网络通信协议。

网络通信协议定义了数据在计算机网络中的传输方式、格式、控制信息等，为网络设备之间的互操作提供了标准和规范。

![xieyi](https://raw.githubusercontent.com/betteryuxuan/Image/main/xieyi.png)

### UDP协议

> 用户数据报协议(User Datagram Protocol)
>
> UDP是面向无连接通信协议。
>
> 速度快，有大小限制一次最多发送64K，数据不安全，易丢失数据

**`DatagramSocket`**

- **定义**：`DatagramSocket` 类用于创建用于发送和接收数据报文的套接字。

- **功能**

  - **发送数据**：通过 `send(DatagramPacket p)` 方法将数据报文发送到指定的目标地址和端口。
  - **接收数据**：通过 `receive(DatagramPacket p)` 方法接收来自网络的数据报文。

- 子类：`MulticastSocket` 

  Java中用于组播通信的类。是 `DatagramSocket` 的子类，因此具有 `DatagramSocket` 的所有功能，并且能够额外支持组播功能。

**`DatagramPacket`**

- **定义**：`DatagramPacket` 类表示数据报文，它封装了发送和接收的数据。
- **功能**
  - **发送数据包**：包含要发送的数据、目标地址和端口。
  - **接收数据包**：用于接收从网络中到达的数据

#### 发送数据

1. 创建DatagramSocker对象
2. 打包数据
3. 发送数据
4. 释放资源

```java
package com.feng.MyInetAddressDemo2;

import java.io.IOException;
import java.net.*;

// 发送消息演示类
public class SendMessageDemo {
    public static void main(String[] args) throws IOException {
        //1.创建DatagramSocker对象（快递公司）
        //绑定端口，以后我们就是通过这个端口往外发送
        //空参:所有可用的端口中随机一个进行使用
        //有参:指定端口号进行绑定
        DatagramSocket ds = new DatagramSocket();

        //2.打包数据
        String str = "你好世界！";  // 待发送的字符串数据
        byte[] bytes = str.getBytes();  // 将字符串转换为字节数组
        InetAddress address = InetAddress.getByName("xuanlaptop"); // 接收方主机名
        int port = 10086;  // 接收方端口号

        DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port); // 构造数据包

        //3.发送数据
        ds.send(dp);  // 发送数据包

        //4.释放资源
        ds.close();  // 关闭DatagramSocket
    }
}
```

#### 接收数据

1. 创建接收端的Datagramsocket对象
2. 接收打包好的数据
3. 解析数据包
4. 释放资源

先接受后发送

```java
package com.feng.MyInetAddressDemo2;

import java.io.IOException;
import java.net.DatagramPacket;
import java.net.DatagramSocket;
import java.net.InetAddress;

public class ReceiveMessageDemo {
    public static void main(String[] args) throws IOException {
        //1.创建DatagramSocket对象
        DatagramSocket ds = new DatagramSocket(10086);

        //2.接收数据包
        byte[] bytes = new byte[1024];
        DatagramPacket dp = new DatagramPacket(bytes, bytes.length);
        //该方法阻塞，程序执行到这一步时，会在这里死等，等待发送端发送消息
        ds.receive(dp);

        //3.解析数据包
        byte[] data = dp.getData();
        int len = dp.getLength();
        InetAddress address = dp.getAddress();
        int port = dp.getPort();

        System.out.println("接收到的数据：" + new String(data, 0, len));
        System.out.println("发送来源于" + address + "该台电脑中" + port + "端口");
        System.out.println();

        //4.释放资源
        ds.close();
    }
}
```

#### 三种通信方式

**一、单播**

> 单播（Unicast）是指一种一对一的通信方式，即通信的两个端点之间建立一条独立的通信连接，消息从发送端点直接传输到接收端点。在单播通信中，每个数据包都有一个确定的目的地址，只有指定目的地址的接收端才会接收到该数据包。

**二、组播**

> 它允许一个设备向多个接收设备发送数据包，而不需要为每个接收设备单独发送一个数据包。

组播地址是IPv4中D类地址，即从 `224.0.0.0` 到 `239.255.255.255`。

**预留的组播地址**

- `224.0.0.0` 到 `224.0.0.255` 是预留的组播地址，用于在本地网络段内使用，不应该被路由到其他网络。这些地址通常用于协议控制信息和本地链路多播。
- 常见的预留组播地址：
  - `224.0.0.1`：所有系统组播地址，所有支持组播的设备都应该监听这个地址。
  - `224.0.0.2`：所有路由器组播地址。
  - `224.0.0.5` 和 `224.0.0.6`：分别用于OSPF路由协议的所有OSPF路由器和DR（Designated Router）。

在Java中，可以使用 `MulticastSocket` 类来实现组播通信

接受代码示例：

```java
public class ReceiveMessageDemo2 {
       public static void main(String[] args) throws IOException {
           //1.绑定到指定端口。创建MulticastSocket对象
           MulticastSocket ms= new MulticastSocket(10000);
   
           //2.指定组播地址。将当前本机，添加到224.0.0.1的这一组当中
           InetAddress address = InetAddress.getByName("224.0.0.1");
           // 加入组播组
           ms.joinGroup(address);
   
           //3.创建DatagramPacket数据包对象
           byte []bytes = new byte[1024];
           DatagramPacket dp = new DatagramPacket(bytes, bytes.length);
   
           //4.接受数据
           ms.receive(dp);
   
           //5.解析数据
           byte[] data = dp.getData();
           int len = dp.getLength();
           String ip = dp.getAddress().getHostAddress();
           String name = dp.getAddress().getHostName();
   
           System.out.println("ip:" + ip + " name:" + name);
           System.out.println(new String(data, 0, len));
   
           //6.释放资源
           ms.close();
       }
   }
```

发送代码示例：

  ```java
public class MulticastExample {
    public static void main(String[] args) {
        try {
            // 创建 MulticastSocket 实例
            MulticastSocket multicastSocket = new MulticastSocket(8888);
            
            // 加入组播组
            InetAddress group = InetAddress.getByName("230.0.0.0");
            multicastSocket.joinGroup(group);

            // 发送数据报
            String message = "Hello, Multicast!";
            byte[] buffer = message.getBytes();
            DatagramPacket packet = new DatagramPacket(buffer, buffer.length, group, 8888);
            multicastSocket.send(packet);

            // 接收数据报
            byte[] receiveBuffer = new byte[1024];
            DatagramPacket receivePacket = new DatagramPacket(receiveBuffer, receiveBuffer.length);
            multicastSocket.receive(receivePacket);
            String receivedMessage = new String(receivePacket.getData(), 0, receivePacket.getLength());
            System.out.println("Received message: " + receivedMessage);

            // 离开组播组
            multicastSocket.leaveGroup(group);
            
            // 关闭 MulticastSocket
            multicastSocket.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
  ```

**三、广播**

> 广播地址 `255.255.255.255` 表示发送到网络中的所有主机。使用 UDP 协议可以很容易地实现广播通信。

将目标IP地址设置为`255.255.255.255`

```java
public class SendMessageDemo1 {
    public static void main(String[] args) throws IOException {
        DatagramSocket ds = new DatagramSocket();

        while (true) {
            Scanner sc = new Scanner(System.in);
            System.out.print("请输入消息：");
            String str = sc.nextLine();
            if (str.equals("886")) {
                break;
            }
            byte[] bytes = str.getBytes();

            InetAddress address = InetAddress.getByName("255.255.255.255");
            int port = 10086;

            DatagramPacket dp = new DatagramPacket(bytes, bytes.length, address, port);

            ds.send(dp);
        }
        ds.close();
    }
}
```

### TCP协议

> TCP（Transmission Control Protocol，传输控制协议）是一个面向连接的、可靠的、基于字节流的传输层通信协议。

**特点**

- **面向连接**：通信双方在传输数据之前必须先建立连接，数据传输结束后要释放连接。
- **可靠性**：TCP通过确认和重传机制确保数据无差错、不丢失、不重复并且按序到达。
- **流控制**：TCP通过滑动窗口协议进行流量控制，防止发送方发送速率过快而导致接收方来不及接收。
- **拥塞控制**：TCP具有拥塞控制机制，以减少因网络中拥塞而导致的数据包丢失。

#### 客户端

**客户端（Socket）操作**

在Java中，使用Socket类来创建TCP客户端。

1. **创建客户端的Socket对象**：
   使用`Socket`类的构造函数来指定要连接的服务器的IP地址和端口号。

   ```java
   java复制代码
   
   Socket socket = new Socket("server_ip_address", port_number);
   ```

2. **获取输出流，写数据**：
   通过`getOutputStream()`方法获取一个`OutputStream`对象，用于向服务器发送数据。

   ```java
   OutputStream outputStream = socket.getOutputStream();  
   // 使用outputStream.write(...)方法发送数据
   ```

   注意：这里您提到的`Outputstream`拼写错误，应该是`OutputStream`。

3. **释放资源**：
   完成数据传输后，应该关闭Socket以释放资源。这通常包括关闭输出流和输入流（如果你还打开了一个），然后关闭Socket本身。

   ```java
   outputStream.close(); // 如果打开了输入流，也要关闭它  
   socket.close();
   ```

   代码示例：

```java
public class Client {
    public static void main(String[] args) throws IOException {
        //1.创建socket对象
        //链接不上代码报错
        Socket socket = new Socket("127.0.0.1",10000);

        //2.从链接通道中获取输出流
        OutputStream os =socket.getOutputStream();
        //写出数据
        os.write("这是中文".getBytes());

        os.close();
        socket.close();
    }
}
```

#### 服务器端

服务器端`ServerSocket`操作

在Java中，使用`ServerSocket`类来创建TCP服务器。

1. **创建服务器端的Socket对象**：
   使用`ServerSocket`类的构造函数来指定服务器要监听的端口号。

   ```java
   ServerSocket serverSocket = new ServerSocket(port_number);
   ```

2. **监听客户端连接，返回一个Socket对象**：
   调用`ServerSocket`对象的`accept()`方法来等待客户端的连接。这个方法会阻塞，直到有客户端连接为止。当客户端连接建立后，`accept()`方法会返回一个与客户端通信的`Socket`对象。

   ```java
   Socket clientSocket = serverSocket.accept();
   ```

   此时可以使用返回的`clientSocket`对象与客户端进行通信。

3. **获取输入流，读数据，并把数据显示在控制台**：
   通过`clientSocket`对象的`getInputStream()`方法获取一个`InputStream`对象，用于从客户端读取数据。然后，您可以将这些数据转换为可以在控制台上显示的格式。

   ```java
   InputStream inputStream = clientSocket.getInputStream();  
   // InputStreamReader读中文，BufferedReader提高效率
   BufferedReader reader = new BufferedReader(new InputStreamReader(inputStream));  
   int b;
   while ((b = br.read()) != -1) {
   	System.out.print((char) b);
   }
   ```

   使用`BufferedReader`来读取文本数据

4. **释放资源**：
   完成与客户端的通信后，应该关闭所有打开的流和Socket，以释放系统资源。

   

   ```java
   reader.close(); // 关闭读取器  
   inputStream.close(); // 如果直接操作InputStream，则关闭它  
   // 如果有输出流，也要关闭它  
   // clientSocket.close(); // 当与特定客户端的通信结束时关闭  
   // 当服务器不再接受任何连接时，关闭serverSocket  
   // serverSocket.close();
   ```

   代码示例：

```java
public class Server {
    public static void main(String[] args) throws IOException {
        //1.创建ServerSocket对象
        ServerSocket ss = new ServerSocket(10000);

        //2.监听客户端链接
        Socket socket = ss.accept();

        //3.从连接通道中获取输入流读取数据
        InputStream is = socket.getInputStream();
        InputStreamReader isr = new InputStreamReader(is);
        BufferedReader br = new BufferedReader(isr);
        int b;
        while ((b = br.read()) != -1) {
            System.out.print((char) b);
        }
        socket.close();
        ss.close();
    }
}
```

#### 三次握手四次挥手

**三次握手（Three-way Handshake）**

三次握手是 TCP 协议用于建立连接的过程，其步骤如下：

1. **客户端发送 SYN 包**：客户端向服务器发送一个 SYN（同步）包，表示客户端请求连接。
2. **服务器回应 SYN-ACK 包**：服务器接收到客户端的 SYN 包后，回复一个 SYN-ACK（同步-确认）包，表示服务器已经收到请求，并准备好建立连接。
3. **客户端发送 ACK 包**：客户端再次发送一个 ACK（确认）包，表示客户端确认连接请求，连接建立成功。

这样，连接就建立起来了，客户端和服务器可以开始进行数据传输。

**四次挥手（Four-way Handshake）**

四次挥手是 TCP 协议用于关闭连接的过程，其步骤如下：

1. **客户端发送 FIN 包**：客户端向服务器发送一个 FIN（结束）包，表示客户端不再发送数据，但仍愿意接收数据。
2. **服务器回应 ACK 包**：服务器接收到客户端的 FIN 包后，发送一个 ACK（确认）包，表示已收到客户端的关闭请求。
3. **服务器发送 FIN 包**：服务器不再发送数据后，向客户端发送一个 FIN 包，表示服务器也准备关闭连接。
4. **客户端回应 ACK 包**：客户端接收到服务器的 FIN 包后，发送一个 ACK 包作为确认，表示客户端已收到服务器的关闭请求。

# 反射

> 反射（Reflection）是Java语言中的一种机制，它允许程序在运行时获取有关自身的信息并能动态地调用对象的方法、属性等。通过反射，可以在运行时检查类、接口、字段和方法等结构，还可以实例化对象、调用方法、访问和修改字段。

反射允许对封装类的字段，方法和构造函数的信息进行编程访问。

反射允许对成员变量，成员方法和构造方法的信息进行编程访问

## 获取字节码文件

**一、获取class对象的三种方式**：

1. **Class.forName(”全类名“);**
2. **类名.class**
3. **对象.getClass();**

```java
package com.feng.myreflect;

public class MyReflectDemo1 {
    public static void main(String[] args) throws ClassNotFoundException {
        //第一种方式（常用）
        //全类名：包名+类名
        Class clazz1 = Class.forName("com.feng.myreflect.Student");

        //第二种方式
        //当做参数传递
        Class clazz2 = Student.class;

        //第三种方式
        //已经存在类的对象，才可以使用
        Student s = new Student();
        Class clazz3 = s.getClass();

        System.out.println(clazz1);
        System.out.println(clazz2);
        System.out.println(clazz3);
    }
}
```

## 获取构造方法

| **Class类方法**                                              | **描述**                                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **`Constructor<?>[] getConstructors()`**                     | **返回此`Class`对象所表示的类的所有公共构造方法对象的数组。** |
| **`Constructor<?>[] getDeclaredConstructors()`**             | **返回此`Class`对象所表示的类的所有构造方法对象（包括私有）的数组。** |
| **`Constructor<T> getConstructor(Class<?>... parameterTypes)`** | **返回此`Class`对象所表示的类的具有指定参数类型的公共构造方法对象。** |
| **`Constructor<T> getDeclaredConstructor(Class<?>... parameterTypes)`** | **返回此`Class`对象所表示的类的具有指定参数类型的构造方法对象（包括私有）。** |

| **Constructor类方法**                   | **描述**                                                     |
| --------------------------------------- | ------------------------------------------------------------ |
| **`T newInstance(Object... initargs)`** | **使用此`Constructor`对象表示的构造方法来创建该构造方法的类的新实例，并用指定的初始化参数初始化它。** |
| **`void setAccessible(boolean flag)`**  | **将此对象的`accessible`标志设置为指示的布尔值。如果值为`true`，则允许访问，否则不允许。这可以用于访问私有或受保护的构造方法。** |

```java
public class MyReflectDemo2 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, InstantiationException, IllegalAccessException {
        //1.获取class字节码文件对象
        Class clazz = Class.forName("com.feng.myreflect.Student");

        //2.获取构造方法
        //公共构造方法
        Constructor[] cons1 = clazz.getConstructors();
        for (Constructor con : cons1) {
            System.out.println(con);
        }
        System.out.println("===========");

        //所有构造方法
        Constructor[] cons2 = clazz.getDeclaredConstructors();
        for(Constructor con: cons2){
            System.out.println(con);
        }
        System.out.println("===========");

        //单个获取所有构造方法
        Constructor con1 = clazz.getDeclaredConstructor();
        Constructor con2 = clazz.getDeclaredConstructor(int.class);
        Constructor con3 = clazz.getDeclaredConstructor(String.class);
        Constructor con4 = clazz.getDeclaredConstructor(String.class, int.class);

        System.out.println(con1);
        System.out.println(con2);
        System.out.println(con3);
        System.out.println(con4);
        System.out.println("===========");

        //获取修饰符整数值
        int modifiers = con4.getModifiers();
        System.out.println(modifiers);

        //获取参数类型
        Parameter[] parameters = con4.getParameters();
        for (Parameter parameter : parameters) {
            System.out.println(parameter);
        }

        //临时取消权限校验
        con4.setAccessible(true);
        Student stu = (Student) con4.newInstance("张三", 10);

        System.out.println(stu);
    }
}
```

## 获取成员变量

**一、Class类中用于获取成员变量的方法**

| 方法签名                              | 描述                           |
| ------------------------------------- | ------------------------------ |
| `Field[] getFields()`                 | 返回所有公共成员变量对象的数组 |
| `Field[] getDeclaredFields()`         | 返回所有成员变量对象的数组     |
| `Field getField(String name)`         | 返回单个公共成员变量对象       |
| `Field getDeclaredField(String name)` | 返回单个成员变量对象           |

**二、Field类中用于创建对象的方法**

| 方法签名                             | 描述   |
| ------------------------------------ | ------ |
| `void set(Object obj, Object value)` | 赋值   |
| `Object get(Object obj)`             | 获取值 |

```java
public class MyReflectDemo3 {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchFieldException, IllegalAccessException {
        Class clazz = Class.forName("com.feng.myreflect.Student");

        //获取所有的成员变量
        Field[] fields = clazz.getDeclaredFields();
        for(Field field:fields){
            System.out.println(field);
        }

        System.out.println("==========");

        //获取单个成员变量
        Field field = clazz.getDeclaredField("name");
        System.out.println(field);

        //获取权限修饰符
        int modifiers = field.getModifiers();
        System.out.println(modifiers);

        //获取成员变量名
        String n = field.getName();
        System.out.println(n);

        //获取成员变量名
        Class<?> type = field.getType();
        System.out.println(type);

        //获取成员变量记录的值
        //先创建一个，把私有的名字设置为可访问的
        Student s = new Student("张三", 10,"男");
        field.setAccessible(true);
        String o = (String) field.get(s);
        
        System.out.println(o);
        System.out.println(s);
        
        //修改对象里面记录的值
        field.set(s,"lisi");

        System.out.println(s);
    }
}
```

## 获取成员方法

**一、Class类中用于获取成员方法的方法**

| 方法签名                                                     | 描述                                       |
| ------------------------------------------------------------ | ------------------------------------------ |
| `Method[] getMethods()`                                      | 返回所有公共成员方法对象的数组，包括继承的 |
| `Method[] getDeclaredMethods()`                              | 返回所有成员方法对象的数组，不包括继承的   |
| `Method getMethod(String name, Class<?>... parameterTypes)`  | 返回单个公共成员方法对象                   |
| `Method getDeclaredMethod(String name, Class<?>... parameterTypes)` | 返回单个成员方法对象                       |

**二、Method类中用于创建对象的方法**

| 方法签名                                    | 描述     |
| ------------------------------------------- | -------- |
| `Object invoke(Object obj, Object... args)` | 运行方法 |

参数一：用obj对象调用该方法<br>参数二：调用方法时传递的参数(如果没有参数就不写)<br>返回值：方法的返回值(如果没有返回值就不写)

**三、示例**

```java
public class Test {
    public static void main(String[] args) throws ClassNotFoundException, NoSuchMethodException, InvocationTargetException, IllegalAccessException {
        //1.获取class字节码文件对象
        Class clazz = Class.forName("com.feng.MyReflectDemo1.Student1");

        //2.获取里面所有的方法对象（包含父类中的所有公共方法）
        Method[] methods1 = clazz.getMethods();
        for (Method method : methods1) {
            System.out.println(method);
        }

        System.out.println("=============");

        //获取里面所有的方法对象（不能获取父类中的，可以获取本类中的私有方法）
        Method[] methods2 = clazz.getDeclaredMethods();
        for (Method method : methods2) {
            System.out.println(method);
        }
        System.out.println("=============");

        //获取指定单个方法
        Method m = clazz.getDeclaredMethod("eat", String.class);
        System.out.println(m);

        //获取修饰符
        int modifiers = clazz.getModifiers();
        System.out.println(modifiers);

        //获取名字
        System.out.println(m.getName());

        //获取方法形参
        int parameterCount = m.getParameterCount();
        Parameter[] parameters = m.getParameters();
        for (Parameter parameter : parameters) {
            System.out.println(parameter);
        }

        //获取抛出的异常
        Class[] exceptionTypes = m.getExceptionTypes();
        for (Class exceptionType : exceptionTypes) {
            System.out.println(exceptionType);
        }

        //运行方法   获取方法返回值
        Student1 s = new Student1();
        m.setAccessible(true);
        //参数一：方法的调用者
        //参数二：调用传递的参数
        String ret = (String) m.invoke(s, "火锅");
        System.out.println(ret); 
    }
}
```

## 反射的作用

1. 获取一个类里面所有的信息，获取到了之后，再执行其他的业务逻辑
2. 结合配置文件，动态的创建对象并调用方法

# 动态代理

> 代理模式是一种常用的设计模式，用于为对象提供一个代理，以控制对对象的访问。在Java中，动态代理的主要用途包括：

- **无侵入式增强**：可以在不修改原始类代码的情况下，为对象添加额外的功能。
- **访问控制**：可以在代理中控制对原始对象的访问，例如实现权限检查、日志记录等。
- **延迟加载**：当需要时才加载资源，例如数据库连接或网络请求。

**一、代理**

在Java中，动态代理是通过接口来实现的。代理对象必须实现与原始对象相同的接口，以便可以调用相同的方法。但代理对象实际上并不执行这些方法的具体实现，而是将方法调用转发给`InvocationHandler`进行处理。

Java通过接口来保证代理对象与原始对象具有相同的“外观”（即方法签名）。代理对象和原始对象都必须实现相同的接口，这样调用者才能通过接口来调用代理对象的方法。

**二、`java.lang.reflect.Proxy`类**

`Proxy`类是Java提供的用于创建动态代理的工具类。它有一个静态方法`newProxyInstance`，用于生成代理对象。这个方法的参数包括：

- `ClassLoader loader`：指定类加载器，用于加载生成的代理类。通常使用原始对象的类加载器。
- `Class<?>[] interfaces`：指定代理对象需要实现的接口列表。这些接口决定了代理对象的方法签名。
- `InvocationHandler h`：指定代理对象的方法调用处理器。当代理对象的方法被调用时，会调用这个处理器的`invoke`方法。

# 注解

**Annotation**

注解是从JDK 5.0开始引入的新技术

## 作用

java代码里的特殊标记，比如:@Override、@Test等

作用：让其他程序根据注解信息来决定怎么执行该程序

可以用在类上、构造器上、方法上、成员变量上、参数上、等位置处。

- **不是程序本身**：注解对程序作出解释，类似于注释（comment）。
- **可被读取**：注解可以被其他程序（如编译器等）读取，用于辅助程序处理。

## 格式

注解以 `@注释名` 的形式存在于代码中

可以添加一些参数值。例如：

```java
@SuppressWarnings(value="unchecked")
```

## 使用位置

注解可以附加在 package、class、method、field 等上面，相当于为它们添加了额外的辅助信息。我们可以通过反射机制编程实现对这些元数据的访问。

## 注解的原理

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/annotation.jpg" style="zoom:50%;" />

**注解本质是一个接口，Java中所有注解都是继承了Annotation接口的**

![Annotation2](https://raw.githubusercontent.com/betteryuxuan/Image/main/Annotation2.png)

**@注解(...)：其实就是一个实现类对象，实现了该注解以及Annotation接口**

## 常见注解

1. **@Override**

   - 定义在 `java.lang.Override` 中。
   - 适用于修饰方法，表示一个方法声明打算重写超类中的另一个方法声明。

   ```java
   @Override
   public String toString() {
       return "Override example";
   }
   ```

2. **@Deprecated**

   - 定义在 `java.lang.Deprecated` 中。
   - 可以用于修饰方法、属性、类，表示不鼓励程序员使用这样的元素，通常因为它很危险或者存在更好的选择。

   ```java
   @Deprecated
   public void oldMethod() {
       // This method is deprecated
   }
   ```

3. **@SuppressWarnings**

   - 定义在 `java.lang.SuppressWarnings` 中。
   - 用来抑制编译时的警告信息。与前两个注释不同，你需要添加一个参数才能正确使用，这些参数都是已经定义好的，可以选择性地使用。

   ```java
   @SuppressWarnings("all")
   public void someMethod() {
       // Suppress all warnings
   }
   
   @SuppressWarnings("unchecked")
   public void anotherMethod() {
       // Suppress unchecked warnings
   }
   
   @SuppressWarnings(value={"unchecked", "deprecation"})
   public void multipleWarnings() {
       // Suppress unchecked and deprecation warnings
   }
   ```

从 Java 7 开始，额外添加了 3 个注解

- `@SafeVarargs` - Java 7 开始支持，忽略任何使用参数为泛型变量的方法或构造函数调用产生的警告。
- `@FunctionalInterface` - Java 8 开始支持，标识一个匿名函数或函数式接口。
- `@Repeatable` - Java 8 开始支持，标识某注解可以在同一个声明上使用多次。

## 元注解

元注解：负责注解其他注解

Java定义了4个标准的`meta-annotation`类型,他们被用来提供对其他`annotation`类型作说明

这些类型和它们所支持的类在`java.ang.annotation`包中可以找到(`@Target`,`@Retention`,`@Documented` ,`@inherited`)

- @Target: 用于描述注解的使用范围(即:被描述的注解可以用在什么地方)
- @Retention: 表示需要在什么级别保存该注释信息,用于描述注解的生命周期(`SOURCE` < `CLASS` < `RUNTIME`)
- @Documented: 说明该注解将被包含在Javadoc中
- @Inherited: 说明子类可以继承父类中的该注解

示例：

```java
package com.feng.annotation;

import java.lang.annotation.*;

public class Annotation {

    @myAnnotation
    public void test() {

    }
}

//定义一个注解
//Target 表示我们的注解可以用在哪些地方
@Target(value = {ElementType.METHOD, ElementType.TYPE})

//Retention 表示我们的注解在哪些地方还有效
//SOURCE < CLASS < RUNTIME
@Retention(value = RetentionPolicy.RUNTIME)

//Documented 表示是否将我们的注解生成在Javadoc中
@Documented

//子类可以继承父类的注解
@Inherited
@interface myAnnotation {

}
```

## 自定义注解

使用 `@interface` 可以自定义注解。自定义注解自动继承了 `java.lang.annotation.Annotation` 接口。

**一、定义**

`@interface` 用来声明一个注解，格式如下：

```java
public @interface 注解名 {
    public 属性类型 属性名() default 默认值;
}
```

**二、注解的参数类型**

- 基本类型（如 int、long、double 等）
- `Class` 类型
- `String` 类型
- `enum` 类型

**三、自定义注解示例**

```java
public @interface MyAnnotation {
    // 定义一个int类型的参数，默认值为0
    int id() default 0;
    
    // 定义一个String类型的参数，默认值为空字符串
    String description() default "";
    
    // 定义一个Class类型的参数，默认值为Object.class
    Class<?> clazz() default Object.class;
    
    // 定义一个枚举类型的参数
    enum Status { ACTIVE, INACTIVE }
    Status status() default Status.ACTIVE;
    
    // 如果只有一个参数成员，参数名一般为value
    String value() default "";
}
```

自定义注解的使用示例：

```java
@MyAnnotation(id = 1, description = "Test Annotation", clazz = String.class, status = MyAnnotation.Status.ACTIVE)
public class TestClass {
    // 类的内容
}

@MyAnnotation("Single parameter value")
public class AnotherTestClass {
    // 类的内容
}
```

**四、细节**

1. **注解元素必须有值**

   注解元素必须有值，在定义注解元素时，通过default来声明参数的默认值，通常使用<u>空字符串</u>或<u>0</u>作为默认值

2. **特殊属性名`value`**

   如果注解中只有一个value属性，使用注解时，value名称可以不写

## 解析注解

**一、注解解析**

注解解析是指判断类、方法、成员变量上是否存在注解，并解析注解中的内容。

解析注解时，首先需要获取目标对象的 `Class`、`Method`、`Field` 或 `Constructor` 对象，再通过这些对象解析其上的注解。

`Class`、`Method`、`Field`、`Constructor`都实现了`AnnotatedElement`接口，它们都拥有解析注解的能力。
`AnnotatedElement`接口提供了解析注解的方法

| 方法                                                         | 说明                           |
| ------------------------------------------------------------ | ------------------------------ |
| `public Annotation[] getDeclaredAnnotations()`               | 获取当前对象上面的注解。       |
| `public <T> T getDeclaredAnnotation(Class<T> annotationClass)` | 获取指定的注解对象             |
| `public boolean isAnnotationPresent(Class<Annotation> annotationClass)` | 判断当前对象上是否存在某个注解 |

**二、步骤**

**要解析谁上面的注解，就应该先拿到谁**

比如要解析类上面的注解，则应该先获取该类的Class对象，再通过Class对象解析其上面的注解

比如要解析成员方法上的注解，则应该获取到该成员方法的Method对象，再通过Method对象解析其上面的注解

**1. 解析类上的注解**

- **获取类的 `Class` 对象**
- **通过 `Class` 对象解析其上的注解**

```java
// 获取类的Class对象
Class clazz = MyClass.class;

// 判断类上是否存在某个注解
if (clazz.isAnnotationPresent(MyAnnotation.class)) {
    // 获取类上的注解
    MyAnnotation annotation = clazz.getAnnotation(MyAnnotation.class);
    // 解析注解内容
    String value = annotation.value();
    System.out.println(value);
}
```

**2. 解析方法上的注解**

- **获取方法的 `Method` 对象**
- **通过 `Method` 对象解析其上的注解**

```java
// 获取方法的Method对象
Method method = clazz.getMethod("myMethod");

// 判断方法上是否存在某个注解
if (method.isAnnotationPresent(MyAnnotation.class)) {
    // 获取方法上的注解
    MyAnnotation annotation = method.getAnnotation(MyAnnotation.class);
    // 解析注解内容
    String value = annotation.value();
    System.out.println(value);
}
```

**3. 解析成员变量上的注解**

- **获取成员变量的 `Field` 对象**
- **通过 `Field` 对象解析其上的注解**

```java
// 获取成员变量的Field对象
Field field = clazz.getField("myField");

// 判断成员变量上是否存在某个注解
if (field.isAnnotationPresent(MyAnnotation.class)) {
    // 获取成员变量上的注解
    MyAnnotation annotation = field.getAnnotation(MyAnnotation.class);
    // 解析注解内容
    String value = annotation.value();
    System.out.println(value);
}
```

