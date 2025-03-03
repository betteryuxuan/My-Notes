# JAVA第三周学习笔记

# 继承

## 概述

> 继承就是子类继承父类的特征和行为，使得子类对象（实例）具有父类的实例域和方法，或子类从父类继承方法，使得子类具有父类相同的行为。

* Java中提供一个关键字extends，用这个关键字，我们可以让一个类与另一个类建立起继承关系

  ```java
  public class Student extends Person{}
  ```

* Student称为子类（派生类），Person称为父类（基类或超类）

## 使用场景

当类与类之间存在相同（共性）的内容，并满足子类时父类的一种，就可以考虑使用继承来优化代码。

## 继承的特点

* Java只支持单继承
* Java不支持多继承，但支持多层继承
* Java中所有的类都直接或者间接继承与Object类
* 子类只能访问父类中非私有的成员

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/java-extends.png" alt="java-extends" style="zoom:80%;" />

## 子类继承的内容

|              | 非私有 |     private      |
| :----------: | :----: | :--------------: |
| **构造方法** |  不能  |       不能       |
| **成员变量** |   能   | 能（不能直接用） |
| **成员方法** |   能   |       不能       |

* 子类不能直接访问父类中的私有成员变量，因为私有成员变量只能在所属类的内部访问。子类只能通过父类提供的公有方法来间接访问父类的私有成员变量。

**虚方法表：**

能被继承的成员方法：

* 非private
* 非static
* 非final

当一个子类继承自父类时，子类会继承父类的虚方法表，并且可以重写（覆盖）父类的虚方法。

每个类都有一个对应的虚方法表，用于存储该类中所有的虚方法。当创建一个对象时，会为该对象分配一块空间来存储虚方法表，并且每个对象都包含一个指向该虚方法表的指针。

## 成员变量访问特点

**就近原则**：谁离我近就用谁

在查找成员变量时遵循就近原则，先在当前**作用域（局部位置）**中查找，然后在**本类的成员位置（成员变量）**中查找，最后在**父类的成员位置**中查找。

`super`关键字：我们可以通过super关键字来实现对父类成员的访问，用来引用当前对象的父类。

`this`关键字：指向自己的引用。

```java
//test01.java
public class test01 {
    public static void main(String[] args) {
        Zi zi1 = new Zi();
        zi1.Print();
    }
}

//Fu.java
public class Fu {
    String name = "Fu";
}

//Zi.java
class Zi extends Fu {
    String name = "Zi";

    public void Print() {
        String name = "Print";
        System.out.println(name); // Print 从局部位置向上找
        System.out.println(this.name); // Zi 从本类成员位置向上找
        System.out.println(super.name); // Fu 从父类成员位置向上找
    }
}
```

## 成员方法访问特点

## 方法的重写

当父类的方法不能满足子类现在的需求时，需要进行方法的重写

**书写格式：**

```java
class Parent {
    // 父类方法
    public void display() {
        System.out.println("Parent");
    }
}

class Child extends Parent {
    // 子类重写父类方法
    @Override
    public void display() {
        System.out.println("Child");
    }
}
```

**@Override重写注解：**

1. @Override是放在重写的方法上，检验子类重写语法是否正确
2. 没有重写，语法错误会显示红色波浪线

<u>发生重写本质是覆盖虚方法表的方法</u>

**注意：**

* 重写方法的名称，形参列表必须与父类一致
* 子类重写父类方法时，访问权限子类必须大于等于父类（空着不写<protected<public）
* 子类重写父类方法是，返回值类型必须小于等于父类
* 私有方法不能被重写
* 子类不能重写父类的静态方法
* 只有被添加到虚方法表中的方法才能重写
* 使用`super.`可以在不完全覆盖父类方法的基础上，扩展子类方法的功能

## 构造方法

* 父类的构造方法不会被子类继承，但可以通过`super`进行调用

* 子类中所有的构造方法都是默认先访问父类的无参构造，再执行自己

**原因：**

* 子类在初始化的时候，有可能会使用到父类中的数据，如果父类没有完成初始化，子类将无法使用父类的数据

* 子类初始化之前，一定要调用父类构造方法先完成父类数据空间的初始化，

**怎么调用父类构造方法**

  * 子类构造方法的第一行语句默认都是：`super()`不写也存在，且必须在第一行。
  * 如果想访问父类有参构造，必须手写`super(###)`进行调用,

## this super

this:理解为一个变量，表示当前方法调用者的地址值;
super:代表父类存储空间。

| 关键字 |              访问成员变量               |                 访问成员方法                 |          访问构造方法           |
| :----: | :-------------------------------------: | :------------------------------------------: | :-----------------------------: |
|  this  | this.成员变量<br/>（访问本类成员变量）  | this.成员方法(...)<br/>（访问本类成员方法）  | this(...)<br/>访问本类构造方法  |
| super  | super.成员变量<br/>（访问父类成员变量） | super.成员方法(...)<br/>（访问父类成员方法） | super(...)<br/>访问父类构造方法 |

`this(...)`访问本类构造方法：成员变量不全部初始化，但是想给其他没初始化的成员变量一个默认值

```java
public class Example {
    private int number;
    private String name;
    
    public Example(int number) {
        this(number, "Default Name"); // 调用另一个构造方法
    }
    
    public Example(int number, String name) {
        this.number = number;
        this.name = name;
    }
}
```

# 多态
同类型的对象，表现出的不同形态。
## 多态的表现形式
```java
父类类型 对象名称 = 子类对象;
```

## 多态的前提

* 有继承关系
* 有父类引用指向子类对象
* 有方法重写
  `Fu f = new Zi();`


## 成员变量和方法调用

* **调用成员变量:**

  **<u>编译看左边，运行也看左边</u>**
  编译看左边:javac编译代码的时候，会看左边的父类中有没有这个变量，如果有，编译成功，如果没有编译失败。
  运行也看左边:java运行代码的时候，实际获取的就是左边父类中成员变量的值

* **调用成员方法:** 

  <u>**编译看左边，运行看右边**</u>
  编译看左边:javac编译代码的时候，会看左边的父类中有没有这个方法，如果有，编译成功，如果没有编译失败
  运行看右边:java运行代码的时候，实际上运行的是子类中的方法

```java
public class Animal {
    String name = "动物";
    public void eat(){
        System.out.println("进食");
    }
}

public class Dog extends Animal {
    String name = "狗";

    @Override
    public void eat() {
        System.out.println("吃狗粮");
    }

    public void LookHome(){
        System.out.println("看家");
    }
}

public class Test {
    public static void main(String[] args) {
        Animal a = new Animal();
        Animal b = new Dog();

        System.out.println(a.name);//动物
        System.out.println(b.name);//动物

        a.eat();//进食
        b.eat();//吃狗粮
        //b.LookHome(); //编译错误
        
        //变回子类型
        Dog c = (Dog)b;
        c.LookHome();//看家
        
    }
}
```

  对于 `a`，它是 `Animal` 类的实例，所以输出的是 `Animal` 类中定义的 `name` 变量的值，即 "动物"；而对于 `b`，它虽然是 `Dog` 类的实例，但由于引用类型是 `Animal`，所以输出的也是 `Animal` 类中定义的 `name` 变量的值，同样是 "动物"。

由于多态的原因，虽然 `b` 的真实类型是 `Dog`，但是在编译时只考虑了 `Animal` 类型，所以调用的是 `Dog` 类中重写覆盖的 `eat()` 方法，输出 "吃狗粮"。

`Animal` 类型的引用不能直接调用 `Dog` 类中特有的方法

## instanceof

>  运算符，用于检查对象是否是某个类的实例或者实现了某个接口。

```java
object instanceof Class
```
确保在 b 是 Dog 类型的对象时才调用 LookHome() 方法。
```java
if (b instanceof Dog) {
    Dog c = (Dog)b;
    c.LookHome();
} else {
    System.out.println("b 不是 Dog 类型的对象");
}

//jdk14
if (b instanceof Dog c) {
    c.LookHome();
} else if (b instanceof Cat d) {
    d.CatchMouse();
} else {
    System.out.println("没有b对应的类型");
}
```

## 优势

在方法中，使用父类型作为参数，可以接受所有的子类对象

> * 通过父类类型引用来访问子类对象，这使得代码更加灵活，能够处理更多类型的对象。
>
>   当需要添加新的子类时，只需编写新的子类并确保它符合父类的接口，而无需修改现有的代码。

## 弊端

不能使用子类的特有功能

类型转换：自动类型转换，强制类型转换

```java
// 自动类型转换
Person p = new Student();
// 强制类型转换
Student s = (Student)p;
```

**强制类型转换：**

* 可以转换为真正的子类类型，从而调用子类独有功能
* 转换类型与真正对象类型不一致会报错
* 转换的时候用`instanceof`关键字判断

# 包

包就是文件夹，用来管理不同功能的java类

## 包名的规则

> 1. **使用小写字母**：包名应该全部使用小写字母，不建议使用大写字母。尽管Java编译器对大小写不敏感，但是惯例上包名使用小写字母是推荐的做法。
> 2. **使用点号分隔**：包名中的各个部分之间使用点号（`.`）进行分隔。点号用于表示包之间的层次关系，类似于文件系统中的目录结构。例如：`com.example`。
> 3. **使用有意义的名称**：包名应该具有描述性，能够清晰地表达其所包含的类和功能。通常使用公司域名的倒序作为根包名，然后根据项目或模块再进行细分。例如：`com.example.project`。
> 4. **避免使用Java保留字**：包名不能使用Java语言中的保留字（例如`class`、`int`等）。
> 5. **不要使用特殊字符**：包名中不应该包含空格、连字符、下划线等特殊字符，只能使用字母、数字和点号。
> 6. **不能以数字开头**：包名不能以数字开头。

## 全类名

> 全类名（fully qualified class name）是指包括包名在内的类的完整名称。在Java中，每个类都有一个唯一的全类名，用于标识该类的位置和名称。

全类名的格式通常为：

```java
包名.类名
```

全类名的主要作用是在Java程序中准确地定位和引用类。

有一个位于`com.example`包中的`MyClass`类，其全类名就是`com.example.MyClass`。

在使用类时，如果没有使用import语句导入类，可以通过全类名来引用类，以避免命名冲突。例如：

```java
// 通过全类名`com.example.MyClass`来创建`MyClass`类的实例。
com.example.MyClass myObj = new com.example.MyClass();
```

**使用其他类的规则：**

1. 使用同一个包中的类时，不需要导包
2. 使用java.lang包中的类时，不需要导包
3. 其他情况都需要导包
4. 如果同时使用两个包中的同名类，需要用全类名

## final

1. **final 修饰变量**：
   - 对于基本类型的变量，`final` 表示该变量的**值**不能被修改，即为常量。
   - 对于引用类型的变量，`final` 表示该变量的**引用**不可变，即不能再指向其他对象，但对象本身的内容是可以修改的。
   - `final` 变量通常在声明时初始化，可以在构造函数中初始化，或者在实例初始化块中初始化。如果没有在声明时初始化，那么必须确保在对象的构造完成之前对 `final` 变量进行初始化。
2. **final 修饰方法**：
   - `final` 修饰的方法不能被子类**重写**（覆盖），确保方法的行为不会被子类改变。
   - 如果一个类被声明为 `final`，其中的所有方法都隐式地成为 `final` 方法，即不能被子类重写。
3. **final 修饰类**：
   - `final` 修饰的类不能被继承，即该类是最终类，不能有子类。

## 常量

1. **基本数据类型常量**：

   - 对于基本数据类型（如整型、浮点型、字符型等），使用 `final` 关键字声明的变量，其值在声明后无法再次修改，这就构成了常量。

   - 例如：

     ```java
     final int MAX_VALUE = 100;
     final double PI = 3.14159;
     final char GRADE = 'A';
     ```

2. **引用类型常量**：

   - 对于引用类型，通过 `final` 关键字声明的变量，其引用不可变，即不能再指向其他对象，但是对象本身的内容是可以修改的。

   - 例如：

     ```java
     public class Example {
         public static void main(String[] args) {
             final StringBuilder sb = new StringBuilder("Hello");
             System.out.println(sb); // Hello
             
             // 可以修改StringBuilder对象的内容
             sb.append(", world!");
             System.out.println(sb); // Hello, world!
             
             // 不能将sb指向其他对象，会编译报错
             // sb = new StringBuilder("Goodbye");
         }
     }
     ```

# 权限修饰符

1. **public**（公共的）：
   - `public` 是最宽松的访问权限修饰符，在任何地方都可以访问到被 `public` 修饰的类、变量、方法和构造函数。
   - 用于表示对所有类都是可见的，比如一个公共接口或者公共类成员。
2. **protected（受保护的）**：
   - `protected` 修饰的成员对同一包内的类以及子类是可见的，即只有同一包内的类和该类的子类可以访问被 `protected` 修饰的成员。
   - 用于表示对子类和同一包内的类可见。
3. **default（只能本包使用）**：
   - 如果没有使用任何权限修饰符，默认情况下成员具有默认的访问权限，即同一包内的类可以访问，但对于不同包内的类则不可访问。
   - 在Java中，没有关键字修饰的成员就是默认访问权限。
4. **private（只能自己用）**：
   - `private` 是最严格的访问权限修饰符，只有声明它的类内部才可以访问被 `private` 修饰的成员，其他类无法访问。
   - 用于表示对当前类可见，其他类无法访问。

|      修饰符       | 同一个类中 | 同一个包中其他类 | 不同包下的子类 | 不同包下的无关类 |
| :---------------: | :--------: | :--------------: | :------------: | :--------------: |
|     `private`     |     √      |                  |                |                  |
| `default`（默认） |     √      |        √         |                |                  |
|    `protected`    |     √      |        √         |       v        |                  |
|     `public`      |     √      |        √         |       √        |        v         |

**权限修饰符的使用规则：**
实际开发中，一般只用`private`和`public`

* 成员变量私有
* 方法公开

特例：如果方法中的代码是抽取其他方法中共性代码，这个方法一般也私有。

# 代码块

## 局部代码块

- 局部代码块是指被 `{}` 包围的一组语句，通常用于限定变量的作用范围，以及在一段代码中创建局部作用域。

- 局部代码块可以出现在方法内部、循环内部、条件语句内部等任意位置。

- 示例：

  ```java
  public class Example {
      public static void main(String[] args) {
          int x = 10;
          {
              int y = 20;
              System.out.println(x + y); // 输出：30
          }
          // System.out.println(y); // 编译错误，y超出了其作用范围
      }
  }
  ```

## 构造代码块

* 写在成员位置的代码块
* 构造代码块在创建对象时会优先于构造函数执行。
* 作用：初始化实例变量，提取重复代码

用于在创建对象时执行初始化操作，与构造函数类似，但没有方法名，并且不带任何修饰符。

主要作用是对对象进行初始化，通常用于对实例变量进行初始化，或者执行一些需要在对象创建时就完成的操作。

```java
public class Example {
    int x; // 实例变量
    
    // 构造代码块
    {
        System.out.println("Constructor block: Initializing instance variable");
        x = 10; // 对实例变量进行初始化
    }
    
    // 构造函数
    public Example() {
        System.out.println("Constructor: Object created");
    }
    
    public static void main(String[] args) {
        Example obj = new Example(); // 创建对象
        System.out.println("x = " + obj.x); // 输出实例变量的值
    }
}
```

构造代码块在对象创建时首先被执行，用于初始化实例变量 `x`。然后，构造函数被执行，输出对象创建的消息。最后，在 `main` 方法中创建对象并输出实例变量 `x` 的值。

## 静态代码块

**使用场景：**用于在类加载时执行一些初始化操作。

**格式：** `static { }` 

特点和作用：

1. **在类加载时执行**：静态代码块在类加载时执行，且<u>只执行一次</u>。这意味着它在类的生命周期中只会执行一次，无论类被实例化多少次，静态代码块只会执行一次。
2. **用于对静态成员进行初始化**：静态代码块通常用于对静态成员进行初始化操作，例如初始化静态变量，加载静态资源等。因为它在类加载时执行，可以保证静态成员在使用前已经完成了初始化。

```java
public class Example {
    static {
        System.out.println("Static block: Class is being loaded");
        // 执行一些静态成员的初始化操作
        // 例如初始化静态变量
    }
    
    public static void main(String[] args) {
        System.out.println("Main method: Class is loaded and ready");
    }
}
```

静态代码块在类加载时优先于 `main` 方法执行，用于对静态成员进行初始化。

```java
class A {
    A () {
        System.out.println("父类构造方法");
    }
}
public class B extends A{
    B() {
        System.out.println("子类构造方法");
    }

    {
        System.out.println("代码初始化块");
    }

    public static void main(String[] args) {
        new B();
    }
}
```

> 父类构造方法
> 代码初始化块
> 子类构造方法
