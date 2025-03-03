# JAVA第二周学习笔记

# IDEA

1. **项目（Project）**：
   - 项目是一个完整的软件工程，通常包含了多个模块或子系统，用于实现一个特定的功能或解决一个特定的问题。
   - 项目可以包含多个模块，每个模块可以独立编译、打包和部署。
2. **模块（Module）**：
   - 模块是项目中的一个独立单元，它负责实现一个特定的功能或提供一组相关的功能。
   - 模块可以包含多个包，通常是逻辑上相关的一组包，用于组织代码和资源。
3. **包（Package）**：
   - 包是Java中用来组织类和接口的一种机制，它类似于文件系统中的文件夹。
   - 包用于对相关的类和接口进行分组，提供了命名空间管理和访问控制的功能。
   - 包名通常采用逆域名的命名规则，例如`com.example.project`。
4. **类（Class）**：
   - 类是Java中最基本的代码单元，用于描述对象的属性和行为。
   - 类定义了对象的结构和行为，包括属性（字段）和方法。
   - 类可以被实例化为对象，对象是类的具体实例，具有类定义的属性和行为。

**层级关系：**

project(工程) - module(模块) - package(包) - class(类)

**具体的：**

1. 一个 project 中可以创建多个 module

2. 一个 module 中可以创建多个 package

   > 包名要求：
   >
   > 1.不要有中文
   >
   > 2.不要以数字开头
   >
   > 3.给包取名时一般都是公司域名倒着写,而且都是小写
   >
   > 比如：尚硅谷网址是 www.atguigu.com
   >
   > 那么 package 包名应该写成：com.atguigu.子名字。

3. 一个 package 中可以创建多个 class

`class`类中不必包含 `main` 方法。在 Java 中，只有包含 `main` 方法的类才能作为程序的入口点，因为 JVM（Java 虚拟机）会在运行程序时自动调用 `main` 方法来执行程序的逻辑。但是，除了程序的入口点之外，其他类并不需要包含 `main` 方法。

# 方法

> 方法是程序中的最小执行单元，也称为函数或过程。它是一段可重复使用的代码块，用于执行特定的任务或操作。方法通常接受输入参数（也称为参数）并产生输出结果（也称为返回值）。

方法可以类比为C语言中的函数

## 格式

方法写在类里面，`main`的外面

```java
public class MethodDemo1 {
    public static void main(String[] args) {
        playgame();
    }
    public static void playgame(){
        System.out.println("you");
        System.out.println("will");
        System.out.println("lose");
    }
}
```

## 带参数及返回值的方法

```java
import java.util.Scanner;
public class MethodDemo1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        int a = sc.nextInt();
        int b = sc.nextInt();
        int c = GetSum(a, b);
        System.out.println(c);
    }

    public static int GetSum(int a, int b) {
        return a + b;
    }
}
```

## 方法的重载

> 方法的重载是指在同一个类中可以定义多个同名的方法，但它们的参数列表（**参数类型、参数个数、参数顺序**）必须不同。重载方法可以提高代码的灵活性和可读性，使得方法名可以更加直观地反映其功能。

Java中的方法重载必须满足以下规则：

1. 方法名必须相同。
2. 参数列表必须不同（参数类型、参数个数、参数顺序），可以有不同的类型和个数的参数，也可以有不同顺序的参数。
3. 方法的返回类型可以相同也可以不同。
4. 与**方法的返回值**、方法的访问修饰符和方法抛出的异常无关。
5. 在同一个类中。

## 方法的内存

>  栈（Stack）通常用于存储方法的调用和局部变量等，它的特点是后进先出（LIFO，Last In First Out）。

当一个方法被调用时，会在栈中分配一段内存，称为栈帧，用于存储方法的参数、局部变量以及方法调用的返回地址等信息。方法执行完成后，对应的栈帧会被销毁，释放出的内存可以被重用。

> 堆（Heap）则是用于动态分配内存的一种方式，通常用于存储对象和数据结构等。

在堆中分配的内存不会自动释放，需要手动进行管理。java通过关键字 `new` ，C通过 `malloc`在堆上分配的内存空间，直到显式释放或程序结束时才会被回收。

---

引用：使用了其他空间中的数据

数组是一种引用数据类型，意味着声明一个数组变量时，实际上是在栈内存中分配了一个引用（或者说指针），该引用指向堆内存中的实际数组对象。数组对象在堆内存中连续存储着多个元素。

例如，声明一个整型数组变量：

```java
int[] arr;//存储的是地址值
```

这里的`arr`是一个引用类型的变量，它在栈内存中分配了空间，但是并没有在堆内存中分配实际的数组对象。需要通过`new`关键字来创建数组对象，例如：

```java
arr = new int[3];
```

这行代码在堆内存中创建了一个长度为3的整型数组对象，并将该对象的引用赋给了`arr`变量。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/1713794915312.jpg" alt="1713794915312" style="zoom: 25%;" />

# 二维数组

## 静态初始化


```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

## 动态初始化

```java
dataType[][] arrayName = new dataType[rows][columns];
```

```java
dataType[][] arrayName = [rows][columns];
```

- 在 Java 中，二维数组是由一系列一维数组组成的，每个一维数组可以具有不同的长度。Java 的多维数组实际上是数组的数组，每个数组元素都是一个引用，指向另一个数组。

# 面向对象

## 类和对象

类：共同特征的描述。

对象：是真实存在的具体示例。

## 如何定义类

```java
public class 类名 {
        1.成员变量
        2.成员方法
        3.构造器
        4.代码块
        5.内部类
}
```
## 如何得到对象

```java
public class 类名 {
   		1.成员变量(属性，名词)
   		2.成员方法(行为，动词)
}
```

```java
类名 对象名 = new 类名();
```

举例：

```java
public class Phone {
    String brand;
    double price;

    public void call(){
        System.out.println("打电话");
    }
    public void play() {
        System.out.println("玩手机");
    }
}
```

```java
public class PhoneTest {
    public static void main(String[] args) {
        Phone p = new Phone();
        p.brand = "mi";
        p.price = 1000;

        System.out.println(p.brand);
        System.out.println(p.price);

        p.call();
        p.play();
    }
}
```

## 注意

1. `javabean`类：用于描述一类事物的类，在`javabean`类中，不写`main`方法

2. 测试类：编写`main`方法的类，可以再测试类中创建`javabean`类，进行赋值调用

3. 定义类名的规定：

   * 首字母以字母、下划线（_）或美元符号（$）开头
   * 不能用关键字
   * 一个`.java`文件可以定义多个类
   * 一个`.java`文件中的多个类中只有一个类可以用`public`修饰，该类名需要与`.java`文件名一致
   * 建议一个文件里一个类

4. 成员变量完整定义：

   ```java
   修饰符 数据类型 变量名 = 初始值
   ```
	**一般无需初始值**
5. 对象的成员变量默认值规则：

   ![moren](https://raw.githubusercontent.com/betteryuxuan/Image/main/moren.jpg)

   

# 封装

> 在面向对象程式设计方法中，封装(Encapsulation)是指一种将抽象性函式接口的实现细节部分包装、隐藏起来的方法。
>
> 封装可以被认为是一个保护屏障，防止该类的代码和数据被外部类定义的代码随机访问。
>
> 要访问该类的代码和数据，必须通过严格的接口控制。

对象代表什么，就得封装对应的数据，并提供数据对应的行为.对象代表现实世界中的实体或概念，封装对应着将对象的数据和行为组合在一起，形成一个独立的、可重用的单位。

## 封装的优点

- 良好的封装能够减少耦合。
- 类内部的结构可以自由修改。
- 可以对成员变量进行更精确的控制。
- 隐藏信息，实现细节。

## private关键字

* 是一个权限修饰符
* 可以修饰成员（成员变量和成员方法）
* 被private修饰的成员在本类中才能访问
* 针对private修饰的成员变量，如果需要被其他类使用，提供相应的操作。

1. 提供`set###(参数)`方法，用于给成员变量赋值，用`public`修饰
2. 提供`get###(参数)`方法，用于获取成员变量的值，用`public`修饰

```java
//GirlFriend.java
package com.feng;

public class GrilFriend {
    private String name;
    private int age;

    public void setName(String n) {
        name = n;
    }

    public String getName() {
        return name;
    }

    public void setAge(int n) {
        if (n >= 0 && n <= 70) {
            age = n;
        } else {
            System.out.println("Invalid Age");
        }
    }

    public int getAge(){
        return age;
    }
}
```

```java
//GirlFriendTest.java
package com.feng;

public class GirlFriendTest {
    public static void main(String[] args) {
        GrilFriend gf1 = new GrilFriend();
        gf1.setName("甜甜");
        gf1.setAge(18);

        System.out.println(gf1.getName());
        System.out.println(gf1.getAge());

        GrilFriend gf2 = new GrilFriend();
        gf2.setName("香香");
        gf2.setAge(20);

        System.out.println(gf2.getName());
        System.out.println(gf2.getAge());
    }
}
```

## **成员变量和局部变量**

**成员变量：**

- **定义位置：** 在类中，方法之外。
- **作用域：** 在整个类中可见，可以被类中的所有方法访问和操作。
- **生命周期：** 随着对象的创建而创建，随着对象的销毁而销毁。
- **存储位置：** 存储在堆内存中，每个对象都有自己的一份成员变量副本。
- **初始化：** 成员变量可以显式初始化，如果未显式初始化，则会根据数据类型自动赋予默认值。
- **作用：** 用于表示对象的状态或属性，通常用来描述对象的特征和属性。

**局部变量：**

- **定义位置：** 在方法、代码块或构造方法内部。
- **作用域：** 仅在声明它的方法、代码块或构造方法内部可见，出了声明的作用域就无法访问。
- **生命周期：** 随着方法、代码块或构造方法的执行而创建，随着执行结束而销毁。
- **存储位置：** 存储在栈内存中，存储的是变量的引用或值。
- **初始化：** 局部变量必须在声明时进行初始化，否则编译器会报错。
- **作用：** 用于临时存储数据或在方法中进行计算，通常用于方法内部的临时变量。

![Screenshot_2024-04-24-20-39-43-696_tv.danmaku.bil](https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-04-24-20-39-43-696_tv.danmaku.bil.jpg)

# this关键字

是一个引用，指向当前对象的引用。this本质是方法调用者的地址值

`this` 关键字可以在类的方法中使用，用于引用当前对象的实例。

```java
public class GrilFriend {
    private String name;
    private int age;

    public void method(){
        int age = 10;
        System.out.println(age);//10
        System.out.println(this.age);//0
    }
}
```

**区分同名变量：** 当对象的成员变量与方法的参数或局部变量同名时，使用 `this` 关键字可以明确指示成员变量。

```java
public class MyClass {
    private int x;
    public void setX(int x) {
    this.x = x; // 使用this引用成员变量x，区分参数x
	}
}
```
# 构造方法

## 作用

创建对象时，由虚拟机自动调用，给成员变量进行初始化的

1. **初始化对象的状态：** 构造方法用于初始化对象的成员变量，确保对象在创建后处于一个合理的状态。
2. **提供对象的初始化选项：** 构造方法可以带有参数，允许在创建对象时传递参数来指定对象的初始状态。
3. **创建对象：** 在 Java 中，使用 `new` 关键字来创建对象时，会自动调用该对象的构造方法来初始化对象。

## 类型

```java
public class Student {
    修饰符 类名(参数) {
        方法体;
    }
}
```
无参数和有参数构造：

```java
public class MyClass {
    // 无参构造方法
    public MyClass() {
    }

    // 带参数的构造方法
    public MyClass(int x, int y) {
    }
}
```
## 特点

1. 方法名与类名相同，大小写也要一致

2. 没有返回值类型，连void都没有

3. 没有具体的返回值(不能由return带回结果数据)

## 执行时机

1. 创建对象的时候由虚拟机调用，不能手动调用构造方法
2. 每创建一次对象，就会调用一次构造方法

## 定义

* 如果没有定义构造方法，系统会给出**一个默认的无参数构造方法**
* 如果定义了构造方法，系统**不再提供默认的无参数构造方法**

## 重载

在同一个类中定义多个构造方法，它们的方法名相同但参数列表不同。

可以提供不同的初始化选项，使得创建对象时可以根据不同的需求选择合适的构造方法进行初始化。

* <u>一般空参构造有参构造都要编写，以应对不同场景。</u>

* **无论是否使用，都要手动书写无参数构造和带全部参数的构造。**

# 标准javabean

1. 类名见名知义
2. 成员变量使用`private`修饰
3. 至少提供两个构造方法
   * 无参构造方法
   * 带全部参数的构造方法
4. 成员方法
   * 提供每一个成员变量对应的`set###()` `get###()`
   * 其他行为

>  快捷键：
>
> 1. `alt` + `insert`/鼠标右键生成(generate)：构造函数获得构造方法，`getter and setter`设置get和set成员方法
> 2. 插件ptg：右键`ptg to javabean`一键生成

# 对象内存图

> 方法区（Method Area）是 Java 虚拟机（JVM）内存中的一个重要组成部分，用于存储类的元信息、静态变量、常量、编译器编译后的代码等数据。方法区在 JVM 规范中也被称为永久代（Permanent Generation），但在 JDK 8 及以后的版本中，永久代已被元空间（Metaspace）所取代。

```java
Student s = new Student();
```
1. 加载class文件

2. 申明局部变量

3. 在堆内存中开辟一个空间

4. 默认初始化

5. 显示初始化

6. 构造方法初始化

7. 将堆内存中的地址值赋值给左边的局部变量

   

   ![Screenshot_2024-04-24-20-15-29-009_tv.danmaku.bil](https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-04-24-20-15-29-009_tv.danmaku.bil.jpg)

基本数据类型：数据存在自己空间

引用数据类型：数据村其他空间中，自己存储的是地址值

this本质是方法调用者的地址值

# API

java API：指的就是JDK中提供的各种功能的java类

Java API帮助文档：Java编程语言的官方文档，提供了关于Java标准库中各个类、接口、方法的详细信息和使用说明。

[Overview (Java SE 17 & JDK 17) (oracle.com)](https://docs.oracle.com/en/java/javase/17/docs/api/index.html)

# 字符串

## 创建Sting对象

1. 直接赋值

   ```java
   String name = "xuanxuan";
   ```

2. `new`

   |               构造方法               |                      说明                      |
   | :----------------------------------: | :--------------------------------------------: |
   |        **`public String( )`**        |     **无参构造方法，创建一个空字符串对象**     |
   | **`public String(String original)`** | **创建一个与原始字符串内容相同的新字符串对象** |
   |   **`public String(char[] chs)`**    |    **创建一个包含字符数组内容的字符串对象**    |
   |   **`public String(byte[] chs)`**    |   **创建一个使用指定字符集编码的字符串对象**   |

   举例：

```java
public class StringDemo {
    public static void main(String[] args) {
        String name1 = "xuan";
        System.out.println(name1);

        String name2 = new String();
        name2 = "xuan";
        System.out.println(name2);

        String name3 = new String("xuan");
        System.out.println(name3);

        char chs[]={'x','u','a','n'};
        String name4 = new String(chs);
        System.out.println(name4);

        byte bytes[]={97,98,99,100};
        String name5 = new String(bytes);
        System.out.println(name5);

    }
}
```


## 常用方法

### 字符串比较

`==`对于基本数据类型比较数据值，对于引用数据类型比较地址值

**字符串内容比较：**

1.  **`boolean equals`**：完全一样返回`true`
2.  **`boolean equalsIgnoreCase`**：忽略大小写

键盘输入的也是new出来的

### **字符串遍历**

1. **`public char charAt(int index)`**：根据索引返回字符
2. **`public int length()`**：返回字符串长度

> 数组名.length
>
> 字符串对象.length()

### 字符串截取

1. **`String substring(int beginIndex, int endIndex)`**: 这个方法返回一个新的字符串，包含从 `beginIndex` 到 `endIndex - 1` 之间的字符。即提取的子串是从 `beginIndex` 开始的，直到但不包括 `endIndex` 位置的字符。
2. **`String substring(int beginIndex)`**: 这个方法返回一个新的字符串，包含从 `beginIndex` 到字符串末尾的所有字符。即提取的子串是从 `beginIndex` 开始一直到字符串的末尾。

### 字符串替换

1. **`String replace(旧值，新值)`**：返回值是替换后结果

## SpringBulider

当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。

和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

StringBuilder可以看做一个容器（一个动态大小的字符数组）。

`StringBuilder` 允许修改字符串内容，而不会产生新的字符串对象，从而提高了字符串操作的效率。

### 构造方法

|               方法名               |                      说明                      |
| :--------------------------------: | :--------------------------------------------: |
|      `public StringBuilder()`      | 才能构建一个空白可变字符的对象，不含有任何内容 |
| `public StringBuilder(String str)` |      根据字符串内容，来创建可变字符串对象      |
|  `  StringBuilder(int capacity)`   | 创建一个具有指定初始容量的StringBuilder对象。  |

### 常用方法

|                 方法名                  |                     说明                      |
| :-------------------------------------: | :-------------------------------------------: |
| `public StringBuilder append(任意类型)` |            添加数据，返回对象本身             |
|    `public StringBuilder reverse()`     |               翻转容器中的内容                |
|          `public int length()`          |                   返回长度                    |
|        `public Sting toString()`        | 通过该方法实现把`StringBuilder`转换为`String` |

## 链式编程

链式编程是一种编程风格，它允许在**同一个对象**上通过**多个方法**的调用链实现一系列操作。

从而简化代码，提高可读性，和代码的可维护性。

这有一个反转字符串的示例，使用了链式编程

```java
package com.feng;

import java.util.Scanner;

public class ReverseString {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String str = sc.next();
        StringBuilder sb = new StringBuilder();

        String ret = sb.append(str).reverse().toString();

        System.out.println(ret);

    }
}
```

## StringJoiner

> 使用 `StringJoiner` 可以指定分隔符，并向其中添加多个字符串，最后得到一个拼接好的字符串。它的构造方法可以接受分隔符、前缀和后缀作为参数，用于指定拼接的格式。

### 构造方法

|                     方法名                      |                             说明                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
|          **`StringJoiner(分隔符号)`**           |          创建一个StringJoiner对象，使用指定间隔符号          |
| **`StringJoiner(分隔符号,开始符号，结束符号)`** | 创建一个StringJoiner对象，使用指定间隔符号，开始符号，结束符号 |

### 成员方法

|               方法名                |                      说明                      |
| :---------------------------------: | :--------------------------------------------: |
| `public StringJoiner add(添加内容)` |             添加数据，返回对象本身             |
|        `public int length()`        |                    返回长度                    |
|     `public String toString()`      | 获取拼接器中的字符串表示，包括前缀、元素和后缀 |

## 字符串原理

### 拼接

如果**没有变量参与**，都是字符串直接相加，<u>编译之后</u>就是拼接之后的结果，会复用串池中的字符串。

如果**有变量**：

> JDK8以前：系统底层会自动创建一个StringBuilder对象，然后再调用其`append`方法完成拼接。
> 拼接后，再调用其`toString`方法转换为String类型，而`toString`方法的底层是直接new了一个字符串对象
> JDK8版本：系统会预估要字符串拼接之后的总大小，把要拼接的内容都放在数组中，此时也是产生一个新的字符串。

### 存储的内存原理

StringTable串池

> `String` 类中的字符串池（String Pool）是一种特殊的存储区域，用于存储字符串常量。它位于堆内存中，并且被所有线程共享。字符串池的主要作用是提高字符串的重用性，避免在内存中重复创建相同内容的字符串，从而节省内存空间。

使用字符串字面量双引号直接创建字符串时，系统会检查该字符串在串池中是否存在：

* 不存在：创建新的

* 存在：复用

```java
String s1 = "Hello"; // 创建一个字符串对象，放入字符串池中
String s2 = "Hello"; // 直接从字符串池中获取已存在的字符串对象
```

* **通过 `new` 关键字创建的字符串对象会先在字符串常量池中是否存在字符串。<u>存在</u>，就在堆内存中创建一个新的字符串对象。不存在，就先在字符串常量池中创建一个字符串对象，再在堆内存中创建一个新的字符串对象，最后返回堆内存中的对象引用**

  **即使字符串池中已经存在字符串，系统也会在堆内存中创建一个新的字符串对象，而不会复用字符串池中的对象。**
  
  `intern()`方法：
  
  当你调用一个字符串的`intern()`方法时，如果池中已经包含了一个与该字符串值相等的字符串，则返回池中的字符串引用；否则，将该字符串添加到池中，并返回该字符串的引用。
  
  ```java
  public class InternExample {
      public static void main(String[] args) {
          String str1 = new String("hello");
          String str2 = "hello";
  
          // str1和str2不是同一个对象，因为str1是用new创建的，存储在堆内存中
          System.out.println(str1 == str2); // 输出 false
  
          // 将str1的引用指向字符串池中的"hello"
          String str3 = str1.intern();
  
          // str2和str3是同一个对象，因为str2本身就是存储在字符串池中的
          System.out.println(str2 == str3); // 输出 true
      }
  }
  ```

> 1. 从 JDK 1.7 开始，字符串常量池被移动了堆内存中;
> 2. 直接赋值字符串，会先去字符串常量池中判断是否存在相同值的字符串对象，存在则返回常量池中的引用，不存在则在常量池中创建一个新对象，并返回引用;
> 3. 使用new 关键字创建字符串对象，会先去字符串常量池中判断是否存在相同值的字符串对象，存在则去堆中创建一个新对象并返回；不存在也在字符串常量池和堆内存中各创建一个字符串对象，并返回堆内存中的对象引用;
> 4. 常量之间的连接，Java 编译器在编译期间会做优化;
> 5. 连接时含有变量的，会被编译成(new StringBuilder()).append(a).append(b).append(c).toString();
> 6. 当调用 intern()时，首先判断常量池中是否存在相同值的字符串常量，如果存在则返回该字符串的引用;如果不存在，则会在常量池中增加一个相同值的字符串常量，并返回它在堆内存中的引用;
> 7. 对于final修饰的变量，它在编译时被解析为常量值的一个本地拷贝存储到自己的常量池中或嵌入到它的字节码流中。
> 8. Java 中的部分关键字在初始化的类中可能被提前使用而放入常量池。

### StringBuilder源码分析

* 默认创建一个长度为16的字节数组

* 添加的内容长度小于16，直接存

  添加的内容大于16会扩容(原来的容量 * 2 + 2)

* 如果扩容之后还不够，以实际长度为准

> 有效地减少频繁扩容带来的性能开销，并且在大部分情况下能够保证足够的容量来存储字符串内容

# 集合

集合长度可变

只能存引用数据类型，基本数据类型（包装类）

## ArrayList

ArrayList 类位于 java.util 包中，使用前需要引入

```java
import java.util.ArrayList;

ArrayList<E> objectName =new ArrayList<>();
```

- **E**: 泛型数据类型，用于设置 objectName 的数据类型，**只能为引用数据类型**。
- **objectName**: 对象名。

### 成员方法

|         方法名         |                说明                 |
| :--------------------: | :---------------------------------: |
|   `boolean add(E e)`   |  添加元素，返回值表示是否添加成功   |
| `boolean remove(E e)`  | 删除指定元素,返回值表示是否删除成功 |
| `E remove(int index)`  |  删除指定索引的元素,返回被删除元素  |
| `E set(int index,E e)` | 修改指定索引下的元素,返回原来的元素 |
|   `E get(int index)`   |         获取指定索引的元素          |
|      `int size()`      | 集合的长度，也就是集合中元素的个数  |

```java
import java.util.ArrayList;
public class ArrayListDemo1 {
    public static void main(String[] args) {
        ArrayList<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");
        System.out.print("[");
        for (int i = 0; i < list.size(); i++) {
            if (i != list.size() - 1)
                System.out.print(list.get(i) + ",");
            else
                System.out.print(list.get(i));
        }
        System.out.println("]");
    }
}
```

### 包装类

<E> 只能为引用数据类型，想使用基本类型使用到基本类型的包装类。

| 基本类型 |   引用类型    |
| :------: | :-----------: |
| boolean  |    Boolean    |
|   byte   |     Byte      |
|  short   |     Short     |
| **int**  |  **Integer**  |
|   long   |     Long      |
|  float   |     Float     |
|  double  |    Double     |
| **char** | **Character** |

```java
ArrayList<Integer> intList = new ArrayList<>();
```

使用 `ArrayList` 存储自定义的对象:

```java
//Student.java
public class Student {
    private String name;
    private int age;
...
}
//ArrayListDemo.java
import java.util.ArrayList;

public class ArrayListDemo2 {
    public static void main(String[] args) {
        ArrayList<Student> list = new ArrayList<>();

        Student s1 = new Student("张三", 10);
        Student s2 = new Student("李四", 12);
        Student s3 = new Student("王五", 14);
        
        list.add(s1);
        list.add(s2);
        list.add(s3);
        
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i).getName() + "," + list.get(i).getAge());
        }
    }
}
```

# static

> `static` 关键字用于创建类级别的成员，这意味着它们属于类本身，而不是类的实例。这些静态成员在整个程序运行期间只有一份副本，并且可以通过类名直接访问，而不需要创建类的实例。


## 静态变量

> 被static修饰的成员变量

```java
public class Student {
    private String name;
    private int age;
    public static String teacherName;// 静态变量
}
```

**特点：**

* 被该类所有对象共享
* 不属于对象，属于类
* 静态变量是随着类的加载而加载的，优先于对象的出现

**调用方式：**

1. 类名调用

   ```java
   //类名调用
   Student.teacherName =  "lihua";
   ```
   
2. 对象名调用

   ```java
   Student st = new Student();
   //对象名调用
   st.teacherName = "lihua";
   ```

## 静态方法

>  静态方法是属于类而不是对象的方法。这意味着你可以直接通过类名调用静态方法，而不需要先创建类的实例。

特点：

* 多用在测试类和工具类中
* javabean类中很少会用

**调用方式：**

1. 类名调用

2. 对象名调用

### 工具类

* javabean类：用来描述一类事物的类
* 测试类：用来检查其他类的正确性，带main方法的类，程序的入口。通过创建对象并调用其方法来测试目标类的功能。
* 工具类：不是用来描述一类事物，而是为了提供一些通用的方法或功能。

**工具类不需要实例化，因为它们提供的方法都是静态的，可以直接通过类名调用。**

**JavaBean类的方法是实例方法，必须通过对象引用来调用，而不能直接使用类名。**

```java
import java.util.List;

public class StudentUtil {
    private StudentUtil(){
        // 私有构造方法，意味着它不能被实例化，也就是说无法使用new关键字创建StudentUtil类的对象。
        // 这样做是为了确保StudentUtil类中的方法都是静态的，可以直接通过类名调用，而不需要创建对象。
    }

    // 计算学生平均分的静态方法
    // 用static修饰
    public static double calculateAverage(List<Student> students) {
        double total = 0;
        for (Student student : students) {
            total += student.getScore();
        }
        return total / students.size();
    }
}
```

```java
//在测试类中直接用类名调用		
double average = StudentUtil.calculateAverage(students);
System.out.println("Average score: " + average);
```

1. **作用**：
   - 提供常用功能的封装：如字符串处理、日期时间操作、文件操作等。
   - 提高代码复用性和可维护性：将常用功能封装在工具类中，可以在多个地方重复使用，同时也方便统一维护和修改。
2. **设计原则**：
   - 单一职责原则（SRP）：每个工具类应该只负责一个功能领域，保持类的简洁和易于理解。
   - 静态方法：通常工具类中的方法都是静态的，方便直接调用，无需实例化类对象。
   - 私有化构造方法：通过将构造方法私有化，可以防止工具类被实例化，确保工具类中的方法都是静态的，可以直接通过类名调用。

## 注意事项

1. **静态方法只能访问静态成员**（静态变量和静态方法），无法直接访问类的非静态成员（实例变量和实例方法）。
2. **非静态方法可以访问所有**：静态变量或者静态方法，非静态的成员变量和非静态的成员方法。
3. **不能使用this关键字**：由于静态方法属于类而不是对象，因此不能在静态方法中使用`this`关键字来引用当前对象。

* 静态随着类的加载而加载
* 非静态跟对象有关

## main方法

`main`方法是Java程序的入口点，它是Java程序的起始点，所有的Java应用程序都必须包含一个`main`方法。

当你运行一个Java程序时，Java虚拟机会首先加载并执行`main`方法。

```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Hello, world!");
    }
}
```

- `public`表示`main`方法是公有的，可以被外部调用。
- `static`表示`main`方法是静态的，可以直接通过类名调用，无需实例化类对象。
- `void`表示`main`方法没有返回值。
- `String[] args`是一个字符串数组参数，用于接收命令行参数。

可以向`String[] args`中传递参数，在 `Program arguments`输入框中输入传递的参数，参数之间用空格分隔，表示不同的字符串。现在很少使用。
