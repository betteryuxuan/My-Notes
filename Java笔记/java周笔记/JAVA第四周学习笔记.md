# JAVA第四周学习笔记

# 抽象类和抽象方法

**抽象方法：**将共性的行为（方法）抽取到父类之后，由于每一个子类执行的内容是不一样，所以，在父类中
不能确定具体的方法体。该方法就可以定义为抽象方法。

**抽象类：**如果一个类中存在抽象方法，那么该类就必须声明为抽象类

意义：强制子类按照固定格式编写，增强了代码的规范性。

## 抽象类的定义格式

```java
public abstract 返回值类型 方法名(参数列表);
```

## 抽象方法的定义格式

```java
public abstract class 类名{}
```

## 注意事项

1. **抽象类不能实例化**：抽象类是为了被子类继承而设计的，因此不能被直接实例化。抽象类的主要目的是作为其他类的基类，用于被继承，而不是被实例化。如果尝试实例化一个抽象类，编译器会报错。
2. **抽象类中不一定有抽象方法**：抽象类中可以包含普通方法、静态方法、成员变量等，不一定要有抽象方法。但是，如果一个类中有抽象方法，那么这个类必须是抽象类。
3. **抽象类可以有构造方法**：抽象类可以包含构造方法，用于初始化抽象类的成员变量等。子类在实例化时会调用抽象类的构造方法，可以通过super关键字来调用。
4. **抽象类的子类**：子类要么重写抽象类中的所有抽象方法，要么自己也是抽象类。一般使用前者。如果一个子类没有实现父类的所有抽象方法，那么这个子类必须声明为抽象类。

继承抽象类的子类如何重写抽象方法：

```java
// Animal.java
public abstract class Animal {
    private String name;
    private int age;

    public abstract void eat();

    public void drink() {
        System.out.println("喝水");
    }


    public Animal() {

    }
// 这里省略setter和gertter
}

// Frog.java
public class Frog extends Animal{
    public Frog() {
    }

    public Frog(String name, int age) {
        super(name, age);
    }

    @Override
    public void eat() {
        System.out.println("吃虫子");
    }
}
```

# 接口

接口是一种规则，是对行为的抽象

## 定义和使用

* 接口用关键字`interface`来定义

```java
[可见度] interface 接口名称 [extends 其他的接口名] {
        // 声明变量
        // 抽象方法
}
```

```java
/* 文件名 : Animal.java */
public abstract interface Animal {
   public abstract void eat();
   public abstract void travel();
}
```

* 接口不能实例化

* 接口和类之间是实现关系，通过`implements`关键字表示

  ```java
  public class 类名 implements 接口名{}
  ```

* 接口的子类(实现类)
  要么重写接口中的所有抽象方法
  要么是抽象类

  

* 注意1：接口和类的实现关系，可以单实现，也可以多实现。

  ```java
  public class 类名 implements 接口名1,接口名2 {}
  ```

* 注意2：实现类还可以在继承一个类的同时实现多个接口。

  ```java
  public class 类名 extends 父类 implements 接口名1,接口名2 {}
  ```

  

## 成员特点

* 成员变量

  **只能是常量，默认修饰符为 `public static final`**

  公共的、静态的（不与任何实例对象相关联）、且只能赋值一次的常量。在接口中声明变量时，可以直接初始化，但不可以在接口中使用实例初始化块进行初始化。

* 构造方法

  **接口不包含构造方法。**因为接口主要是用于描述行为，而不是描述对象的状态，所以没有构造方法的需求。

* 成员方法

  **只能是抽象方法，默认修饰符为`public abstract`**

## 和类之间的关系

* 类和类的关系

继承关系，只能单继承，不能多继承，但是可以多层继承

* 类和接口的关系

实现关系，可以单实现，也可以多实现，还可以在继承一个类的同时实现多个接口

> 一个类实现了多个接口，并且这些接口中存在同名方法时，实现类并不需要显式地重写这个方法,只需要提供一次具体的实现即可。

* 接口和接口的关系

继承关系，可以单继承，也可以多继承

> 实现类实现最下面的子接口要重写所有抽象方法

## 新增

* **JDK7以前：接口中只能定义<u>抽象方法</u>，**

* **JDK8的新特性：接口中可以定义有方法体的方法。(<u>默认、静态</u>)**

* **JDK9的新特性：接口中可以定义<u>私有方法</u>。**

  

---

### JDK8新增方法

* **<u>允许在接口中定义默认方法，需要使用关键字 default 修饰</u>**
  作用：解决接口升级的问题
* 接口中默认方法的定义格式：
  格式：`public default 返回值类型 方法名(参数列表){}`
  范例：`public default void show(){}`
* 接口中默认方法的注意事项
  1. 默认方法不是抽象方法，所以不强制被重写。但是如果被重写，重写的时候去掉default关键字
  2. public可以省略，default不能省略
  3. 如果实现了多个接口，多个接口中存在相同名字的默认方法，子类就必须对该方法进行重写



* **<u>允许在接口中定义静态方法，需要用 static 修饰</u>**
* 接口中静态方法的定义格式：
  格式：`public static 返回值类型 方法名(参数列表){}`
  范例：`public static void show(){}`
* 接口中静态方法的注意事项：
  静态方法只能通过接口名调用，不能通过实现类名或者对象名调用
  public可以省略，static不能省略

> 正常强制重写，默认不强制重写，静态强制不能重写

### JDK9新增方法

* **<u>允许在接口中定义私有方法，需要用 private 修饰</u>**

  不让外界访问，给本接口默认方法或静态方法服务

* 接口中静态方法的定义格式：

  格式：`private 返回值类型 方法名(参数列表){}`（默认方法）
  范例：`private void show(){}`

  格式：`private static 返回值类型 方法名(参数列表){}`（静态方法）
  范例：`private static void method(){}`

## 总结

接口与类之间的关系更多地是一种协议或契约，类通过实现接口来说明自己能够执行某些操作或具备某些行为。

1. 接口代表规则，是行为的抽象。想要让哪个类拥有一个行为，就让这个类实现对应的接口就可以了，
2. 当一个方法的参数是接口时，可以传递接口所有实现类的对象，这种方式称之为接口多态，

## 设计模式

> 设计模式是一套被反复使用、多数人知晓、经过分类编目的、代码设计经验的总结。设计模式的本质是解决问题的方案，是一种在特定场景下解决特定问题的经验总结。设计模式提供了一种标准的解决方案，可以帮助开发人员解决常见的设计问题，提高代码的可读性、可维护性和可扩展性。

**适配器设计模式：**

> 当一个接口中的抽象方法过多，但我只需要使用其中一部分，就可以使用适配器设计模式

步骤：

1. 编写中间类`xxxAdapter`实现对应接口
2. 对接口中的抽象方法进行空实现
3. 让真正的实现类继承中间类，并重写需要用的方法
4. 为了避免其他类创建适配器类的对象，中间的适配器类使用`abstract`修饰

# 内部类

> 内部类是定义在另一个类内部的类。

* 内部类可以访问外部类的成员，包括私有成员

* 外部类访问内部类的成员，必须创建对象

## 使用场景

B类表示的事物是A类的一部分，且B单独存在没有意义

## 分类

1. 成员内部类
2. 静态内部类
3. 局部内部类
4. 匿名内部类

## 成员内部类

> 写在成员位置的，属于外部类的成员。

* 成员内部类可以被一些修饰符所修饰，比如:`private`，默认，`protected`，`public`，`static`等
* 在成员内部类里面，JDK16之前不能定义静态变量，JDK16开始才可以定义静态变量。

### 获取内部类对象

1. 当成员内部类被**`private`**修饰时
   在外部类编写方法，对外提供内部类对象
2. 当成员内部类被**非私有**修饰时，直接创建对象，
   `Outer.inner oi= new Outer().new inner()`

* 当成员内部类被非私有修饰时，直接创建对象，

```java
// 当成员内部类被非私有修饰时，直接创建对象，
public class Outer {
    String name;
    int age;

    public class Inner {
        String finger;
        int count;
    }
}

public class Test {
    public static void main(String[] args) {
        Outer i = new Outer();
        System.out.println(i.name);
        System.out.println(i.age);

        Outer.Inner j = new Outer().new Inner();// 创建内部类的对象 j
        System.out.println(j.finger);
        System.out.println(j.count);
    }
}
```

* 当内部类使用`private`修饰，无法直接访问内部类成员，可以让外部类提供方法返回内部类对象

  ```java
  // 内部类使用private修饰
  public class Outer {
      String name;
      int age;
  
      private class Inner {
          String finger;
          int count;
      }
  
      public Inner getInstance(){
          return new Inner();
      }
  }
  ```

### 访问成员变量

堆内存中存储内部类对象的同时，还会存储一个指向外部类对象的引用（地址）

```java
public class Outer {
    private int a = 10;

    class Inner {
        private int a = 20;

        public void show() {
            int a = 30;
            //堆内存存储内部类中，还存了Outer this，指向外部类对象
            System.out.println(Outer.this.a); // 10
            System.out.println(this.a); // 20
            System.out.println(a); // 30
        }
    }

}
```

```java
public class Test {
    public static void main(String[] args) {
        Outer.Inner i = new Outer().new Inner();
        i.show();
    }
}
```

## 静态内部类

> 静态内部类只能访问外部类中的静态变量和静态方法，如果想要访问非静态的需要创建对象。

创建静态内部类对象的格式：`外部类名.内部类名 对象名 = new 外部类名.内部类名();`

例：`Outer.Inner oi = new Outer.Inner();`

调用**非静态方法**的格式：`先创建对象，用对象调用`
调用**静态方法**的格式：`外部类名.内部类名.方法名();`

例：`Outer.Inner.show2();`

```java
//静态内部类只能访问外部类中的静态变量和静态方法，如果想要访问非静态的需要创建对象。
public class Outer {
    int a = 10;
    static int b = 20;

    static class Inner {
        public void show1() {
            Outer oi = new Outer(); // 创建外部类对象
            System.out.println(oi.a); // 访问外部类的非静态成员变量 a
            System.out.println(b); // 访问外部类的静态成员变量 b
        }

        public static void show2() {
            Outer oi = new Outer();
            System.out.println(oi.a);
            System.out.println(b);
        }
    }
}
```

```java
public class TestOuter {
    public static void main(String[] args) {
        // 调用静态内部类的非静态方法
        Outer.Inner inner1 = new Outer.Inner();
        inner1.show1();

        // 调用静态内部类的静态方法
        Outer.Inner.show2();
    }
}
```

## 局部内部类

> 将内部类定义在方法里面就叫做局部内部类，类似于方法里面的局部变量。

1. **外界无法直接使用：** 外界无法直接访问局部内部类，因为它的作用域被限制在方法内部。如果需要使用局部内部类，必须在方法内部先创建该类的对象，然后通过对象来访问其方法和成员变量。
2. **可以直接访问外部类的成员：** 局部内部类可以直接访问外部类的成员，包括成员变量和成员方法。这是因为局部内部类相当于外部类的成员，具有访问外部类成员的权限。
3. **可以访问方法内的局部变量：** 局部内部类可以访问所在方法内的局部变量

<u>声明局部类时不能有访问说明符(即`public`或`private`)。局部类的作用域被限定在声明这个局部类的块中。</u>

```java
public class Outer {
    private int outerVariable = 10;

    public void method() {
        int methodVariable = 20; // 方法内的局部变量

        class LocalInner {
            public void print() {
                System.out.println(outerVariable); // 访问外部类的成员变量
                System.out.println(methodVariable); // 访问方法内的局部变量
            }
        }

        LocalInner localInner = new LocalInner(); // 创建局部内部类的对象
        localInner.print(); // 调用局部内部类的方法
    }
}
```

## 匿名内部类

> 没有显式的类名，直接在创建对象时定义类的实现

前提：需要一个类或接口作为基础。

如果是实现接口，那么匿名内部类会实现该接口的方法；

如果是继承类，那么匿名内部类会重写该类的方法。

可以写在成员位置，也可以写在局部位置

### 格式

```java
new 类名或接口名() {
重写方法;
}
```

### 使用场景

当方法的参数是接口或者类时
以接口为例，可以传递这个接口的实现类对象
如果实现类只要使用一次，就可以用匿名内部类简化代码

## 示例

**类：**

```java
public abstract class Animal {
    public abstract void eat();
}

// Test.java
public class Test {
    public static void main(String[] args) {
        //形式1
        new Animal() {
            @Override
            public void eat() {
                System.out.println("狗吃骨头");
            }
        }.eat();

        //形式2
        Animal dog = new Animal() {
            @Override
            public void eat() {
                System.out.println("狗吃骨头");
            }
        };
        dog.eat();
    }
}
```

**接口：**

```java
public interface Inter {
    public abstract void method();
}
// Demo.java
public class Demo {
    public static void main(String[] args) {
        //形式1
        new Inter() {
            @Override
            public void method() {
                System.out.println("重写后方法");
            }
        }.method();

        //形式2:将匿名内部类的实例作为参数传递给另一个方法。
        function(new Inter() {
            @Override
            public void method() {
                System.out.println("重写后方法");
            }
        });
    }

    public static void function(Inter i) {
        i.method();
        //按照以往需要写接口的实现类，并创建对象传递给该方法
    }
}
```

# 常用API

## Math

私有化构造方法，所以方法都是静态的

常用方法：

|                      方法名                       |                    说明                     |
| :-----------------------------------------------: | :-----------------------------------------: |
|        **`public static int abs(int a)`**         |               获取参数绝对值                |
|     **`public static double ceil(double a)`**     |                  向上取整                   |
|    **`public static double floor(double a)`**     |                  向下取整                   |
|      **`public static int round(float a)`**       |                  四舍五入                   |
|     **`public static int max(int a,int b)`**      |                  取较大值                   |
| **`public static double pow(double a,double b)`** |              返回a的b次幂的值               |
|        **`public static double random()`**        | 返回值为double的随机值，范围**`[0.0,1.0)`** |
|     **`public static double sqrt(double a)`**     |                返回a的平方根                |
|     **`public static double cbrt(double a)`**     |                返回a的立方根                |

1. `round`

   **round** 表示"**四舍五入**"，算法为**Math.floor(x+0.5)** ，即将原来的数字加上 0.5 后再向下取整，所以 **Math.round(11.5)** 的结果为 12，Math.round(-11.5) 的结果为 -11。

   ```java
   double result = Math.round(11.5);
   System.out.println(result); // 输出 11.0
   
   double result2 = Math.round(-11.5);
   System.out.println(result2); // 输出 -11.0
   ```
2. `floor`
   floor 返回不大于的最大整数。
   
   ```java
   double result = Math.floor(11.5);
   System.out.println(result); // 输出 11.0
   
   double result2 = Math.floor(-11.5);
   System.out.println(result2); // 输出 -12.0
   ```
   
   

```java
public class Demo {
    public static void main(String[] args) {
        for (int i = 0; i < 100; i++)
            System.out.println(Math.floor(Math.random() * 100 + 1));// 1 到 100 之间随机数
    }
}
```



## System

|                            方法名                            |             说明             |
| :----------------------------------------------------------: | :--------------------------: |
|          **`public static void exit(int status)`**           |  终止当前运行的 Java 虚拟机  |
|         **`public static long currentTimeMillis()`**         | 返回当前系统的时间毫秒值形式 |
| **`public static void arraycopy(数据源数组，起始索引，目的地数组，起始索引，拷贝个数)`** |           数组拷贝           |

**`public static void arraycopy(数据源数组，起始索引，目的地数组，起始索引，拷贝个数)`**：

- `数据源数组`：表示要复制的数组。
- `起始索引`：表示从数据源数组的哪个位置开始复制。
- `目的地数组`：表示要将元素复制到的数组。
- `起始索引`：表示从目的地数组的哪个位置开始存放。
- `拷贝个数`：表示要复制的元素个数。

ex：`System.arraycopy(arr1, 0, arr2, 0, 10);`

1. 如果数据源数组和目的地数组都是基本数据类型，那么两者的类型必须保持一致，否则会报错
2. 在拷贝的时候需要考虑数组的长度，如果超出范围也会报错
3. 如果数据源数组和目的地数组都是引用数据类型，那么子类类型可以赋值给父类类型

## Runtime

Runtime表示当前虚拟机的运行环境

|                  方法名                   |                  说明                   |
| :---------------------------------------: | :-------------------------------------: |
| **`public static Runtime getRuntime()`**  |         当前系统的运行环境对象          |
|    **`public void exit(int status)`**     |               停止虚拟机                |
|  **`public int availableProcessors()`**   |             获得CPU的线程数             |
|       **`public long maxMemory()`**       |  JVM能从系统中获取总内存大小(单位byte)  |
|      **`public long totalMemory()`**      | JVM已经从系统中获取总内存大小(单位byte) |
|      **`public long freeMemory()`**       |        JVM剩余内存大小(单位byte)        |
| **`public process exec(String command)`** |               运行cmd命令               |


Runtime类构造方法是`private Runtime() {}`

![runtime](https://raw.githubusercontent.com/betteryuxuan/Image/main/runtime.png)

 `Runtime` 类的构造方法是私有的，因此无法直接通过 `new` 关键字来创建 `Runtime` 类的对象。但是 `Runtime` 类提供了一个静态方法 `getRuntime()` 来获取当前应用程序的 `Runtime` 对象，因此可以通过调用这个静态方法来获取 `Runtime` 对象。

```java
// 方式一
Runtime runtime = Runtime.getRuntime();
System.out.println(runtime.availableProcessors());
// 方式二
System.out.println(Runtime.getRuntime().availableProcessors());
```
## Object

* Object是Java中的顶级父类。所有的类都直接或间接的继承于Object类。
* Object类中的方法可以被所有子类访问

**构造方法：**

`public Object()`只有空参构造

|                 方法名                  |           说明           |
| :-------------------------------------: | :----------------------: |
|     **`public string tostring()`**      | 返回对象的字符串表示形式 |
| **`public boolean equals(object obj)`** |   比较两个对象是否相等   |
|   **`protected object clone(int a)`**   |         对象克隆         |

**tostring：**

```java
Student stu= new student();
String str2 = stu.tostring();
System.out.println(str2); // com.itheima.a04objectdemo.student@4eec7777
System.out.println(stu); // com.itheima.a04objectdemo.student@4eec7777
```

`toString()` 方法返回的是包含类名和对象的哈希码的字符串表示形式，格式为 `类名@哈希码`。
`system`：类名
`out`：静态变量
`system.out`:获取打印的对象
`println()`：方法
参数：表示打印的内容
逻辑：打印一个对象的时，底层会调用对象的`tostring`方法，把对象变成字符串。然后再打印在控制台上，打印完毕换行处理。

**<u>可以通过重写类中`toString()` 方法输出对象的自定义字符串表示形式，而不是默认的地址值。</u>**

**equals：**

`equals()` 方法是用于比较两个对象是否相等的方法。在 `Object` 类中，`equals()` 方法默认实现是比较两个对象的地址值（即引用是否指向同一个对象），因为在 `Object` 类中，`equals()` 方法没有被重写。

如果需要在自定义类中实现对象相等的比较，通常需要重写 `equals()` 方法，以便根据对象的属性值来判断对象是否相等。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/equals.png" alt="equals" style="zoom:50%;" />

```java
public class Person {
    private String name;
    private int age;
    
    // 构造方法、getter 和 setter 方法省略
    
    @Override
    public boolean equals(Object obj) {
        if (this == obj) {
            return true; // 如果是同一个对象，则相等
        }
        if (obj == null || getClass() != obj.getClass()) {
            return false; // 如果传入的对象为空或者不是同一个类的实例，则不相等
        }
        Person person = (Person) obj; // 强制转换为 Person 类型
        return age == person.age && Objects.equals(name, person.name); // 比较姓名和年龄是否相等
    }
}
```

**clone：**

1. **浅克隆**：在浅克隆中，只复制对象本身以及对象中的基本数据类型字段的值。对于对象中的引用类型字段，仅复制引用而不复制引用指向的对象。因此，原始对象和克隆对象共享相同的引用对象。如果引用对象被修改，那么原始对象和克隆对象都会受到影响。
2. **深克隆**：在深克隆中，不仅复制对象本身，还会递归地复制对象中所有引用类型字段所引用的对象，直到所有引用的对象都被复制（字符串不是new的会复用串池中的）。这样，原始对象和克隆对象拥有各自独立的引用对象，它们之间互不影响。

默认浅克隆，需要深克隆需要重写方法或者使用第三方工具类                                                                                                                                                                                                                                                                                                                                                      

## Objects

工具类，提供了一些方法去完成一些功能

|                         方法名                         |                  说明                   |
| :----------------------------------------------------: | :-------------------------------------: |
| **`public static boolean equals(object a, object b)`** |       先做非空判断，比较两个对象        |
|     **`public static boolean isNull(object obj)`**     | 判断对象是否为Null，为Null返回true,反之 |
|    **`public static boolean nonNull(object obj)`**     | 判断对象是否为Null，跟isNull的结果相反  |

## BigInteger

|                    方法名                     |                 说明                  |
| :-------------------------------------------: | :-----------------------------------: |
| **`public BigInteger(int num，Random rnd)`**  | 获取随机大整数，范围:[0~2的num次方-1] |
|      **`public BigInteger(string val)`**      |           获取指定的大整数            |
| **`public BigInteger(String val,int radix)`** |         获取指定进制的大整数          |

| **`public static BigInteger valueof(long val)`** | 静态方法获取BigInteger的对象，内部有优化 |
| :----------------------------------------------: | :--------------------------------------: |

```java
//1
	Random rd = new Random();
	for (int i = 0; i < 20; i++) {
     	BigInteger bd1 = new BigInteger(5, rd);
         System.out.println(bd1);
     }
	
//2
	BigInteger bd2 = new BigInteger("9876543210");//字符串必须整数
	System.out.println(bd2);

//3
	BigInteger bd3 = new BigInteger("1111", 2);
	System.out.println(bd3);

//4 静态方法获取对象，范围较小,long的范围
	BigInteger bd4 = BigInteger.valueOf(2147483647);
	System.out.println(bd4);
```

`valueOf`在内部对常用的数字-16 ~ 16进行了优化，提前把-16~16 先创建好`BigInteger`的对象，如果多次获取不会重新创建新的。

>小结：
>
>1. 如果BigInteger表示的数字没有超出long的范围，可以用静态方法获取，
>2. 如果BigInteger表示的超出long的范围，可以用构造方法获取。
>3. 对象一旦创建，BigInteger内部记录的值不能发生改变。
>4. 只要进行计算都会产生一个新的BigInteger对象

|                            方法名                            |              说明               |
| :----------------------------------------------------------: | :-----------------------------: |
|         **`public BigInteger add(BigInteger val)`**          |              加法               |
|       **`public BigInteger subtract(BigInteger val)`**       |              减法               |
|       **`public BigInteger multiply(BigInteger val)`**       |              乘法               |
|        **`public BigInteger divide(BigInteger val)`**        |          除法，获取商           |
| **`public BigInteger[] divideAndRemainder(BigInteger val)`** |       除法，获取商和余数        |
|            **`public boolean equals(Object x)`**             |          比较是否相同           |
|          **`public BigInteger pow(int exponent)`**           |              次幂               |
|       **`public BigInteger max/min(BigInteger val)`**        |        返回较大值/较小值        |
|          **`public int intValue(BigInteger val)`**           | 转为int类型整数超出范围数据有误 |

##  BIgDecimal

|                            方法名                            |   说明   |
| :----------------------------------------------------------: | :------: |
|      **`public static BigDecimal valueOf(double val)`**      | 获取对象 |
|         **`public BigDecimal add(BigDecimal val)`**          |   加法   |
|       **`public BigDecimal subtract(BigDecimal val)`**       |   减法   |
|       **`public BigDecimal multiply(BigDecimal val)`**       |   乘法   |
|        **`public BigDecimal divide(BigDecimal val)`**        |   除法   |
| **`public BigDecimal divide(BigDecimal  val，精确几位，舍入模式)`** |   除法   |

```java
import java.math.BigDecimal;

public class Demo2 {
    public static void main(String[] args) {
        //1.通过传递double类型的小数来创建对象,不精确
        BigDecimal bd1 = new BigDecimal(0.09);
        BigDecimal bd2 = new BigDecimal(0.01);
        BigDecimal bd3 = bd1.add(bd2);
        System.out.println(bd3);

        //2.通过传递字符串表示的小数来创建对象
        BigDecimal bd4 = new BigDecimal("0.09");
        BigDecimal bd5 = new BigDecimal("0.01");
        BigDecimal bd6 = bd4.add(bd5);
        System.out.println(bd6);//0.10

        //3.通过静态方法获取对象
        BigDecimal bd7 = BigDecimal.valueOf(10);
        BigDecimal bd8 = BigDecimal.valueOf(10);
        System.out.println(bd7 == bd8);//true

        BigDecimal bd9 = BigDecimal.valueOf(10.0);
        BigDecimal bd10 = BigDecimal.valueOf(10.0);
        System.out.println(bd9 == bd10);//false
    }
}
```

1. 如果要表示的数字不大，没有超出double的取值范围，建议使用静态方法
2. 如果要表示的数字比较大，超出了double的取值范围，建议使用构造方法
3. 如果我们传递的是0-10之间的**整数**，包含0，10，那么方法会返回己经创建好的对象，不会重新new

底层存储：

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/bigdecimal.png" alt="bigdecimal" style="zoom:67%;" />

转字符，变为ANCII，存数组

负数存符号，正数不存。

## 正则表达式 

|                           方法名                           |                说明                |
| :--------------------------------------------------------: | :--------------------------------: |
|        **`public String[] matches(String regex)`**         | 判断字符串是否满足正则表达式的规则 |

| 符号 | 含义                                                   |
| ---- | ------------------------------------------------------ |
| `.`  | 任意单个字符，除换行符外                               |
| `[]` | [ ] 中的任意一个字符                                   |
| `-`  | [ ] 内表示字符范围                                     |
| `^`  | 在[ ]内的开头，匹配除[ ]内的字符之外的任意一个字符     |
| `| ` | 或                                                     |
| `\`  | 将下一字符标记为特殊字符、文本、反向引用或八进制转义符 |

字符类（只匹配一个字符）

|         用法         |                  注释                  |
| :------------------: | :------------------------------------: |
|     **`[abc]`**      |            只能是a，b，或c             |
|     **`[^abc]`**     |       除了a，b，c之外的任何字符        |
|    **`[a-zA-Z]`**    |         a到z A到Z，包括(范围)          |
|   **`[a-d[m-p]]`**   |              a到d，或m到p              |
|  **`[a-z&&[def]]`**  |      a-z和def的交集。为：d，e，f       |
| **`[a-z&&[ ^bc ]]`** |    a-z和非bc的交集。(等同于[ad-z])     |
| **`[a-z&&[ ^m-p ]`** | a到z和除了m到p的交集。(等同于[a-lq-z]) |

预定义字符（只匹配一个字符）

| **`.`**  |    任何字符    |       任何字符，\n不匹配       |
| :------: | :------------: | :----------------------------: |
| **`\d`** |  数字字符匹配  |        一个数字：[0-9]         |
| **`\D`** | 非数字字符匹配 |        非数字：[ ^0-9]         |
| **`\s`** |  匹配空白字符  |  一个空白字符：[\t\n\x0B\f\r]  |
| **`\S`** | 匹配非空白字符 |       非空白字符：[ ^\s]       |
| **`\w`** |  匹配单词字符  | [a-zA-Z_0-9]英文、数字、下划线 |
| **`\W`** | 匹配非单词字符 |      [^\w]一个非单词字符       |

|   **`?`**   |       0次或1次       |
| :---------: | :------------------: |
|   **`*`**   |      0次或多次       |
|   **`+`**   |      1次或多次       |
|  **`{n}`**  |       正好n次        |
| **`{n,}`**  |       至少n次        |
| **`{n,m}`** |   至少n但不超过m次   |
| **`(?i)`**  | 忽略后面字符的大小写 |

```java
public class Demo3 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s1 = sc.next();
        String s2 = sc.next();

        String reges1 = "\\w{4,16}"; //手机号
        String reges2 = "[1-9]\\d{16}(\\d|(?i)x)"; //身份证
        String reges3 = "[1-9]\\d{16}[\\dXx]";

        System.out.println(s1.matches(reges1));
        System.out.println(s2.matches(reges2));
    }
}
```

### 爬虫

在一段文本中查找满足要求的内容：

```java
public class Demo4 {
    public static void method1(String str) {
        Pattern p = Pattern.compile("(?i)Java\\s\\d{0,2}");
        //Pattern p = Pattern.compile("((?i)java\\s)(?=1|5|6|7|8)");
        //Pattern p = Pattern.compile("((?i)java\\s)(1|5|6|7|8)");
        //Pattern p =Pattern.compile("((?i)java\\s)(?:1|5|6|7|8)");与上面等价
        Matcher m = p.matcher(str);
        while (m.find()) {
            String s = m.group();
            System.out.println(s);
        }
    }

    public static void main(String[] args) {
        String s = "Java 5（2004年，也称为Java SE 5或Java 1.5）：引入了重要的语言增强，如泛型、枚举、自动装箱/拆箱、可变参数和注解等。Java 6（2006年，也称为Java SE 6）：加入了JDBC 4.0、Java编译器API、集合框架增强和Web服务支持等功能。Java 7（2011年，也称为Java SE 7）：带来了重要的语言和API更新，如try-with-resources语句、switch语句的字符串支持、钻石操作符和Fork/Join框架等。Java 8（2014年，也称为Java SE 8）：引入了Lambda表达式、Stream API、新的日期/时间API、默认方法和可重复注解等功能。";
        method1(s);
    }
}
```

**贪婪爬取和非贪婪爬取**：

> 贪婪爬取：在爬取数据的时候尽可能的多获取数据
> 非贪婪爬取：在爬取数据的时候尽可能的少获取数据

 ab+:

贪婪爬取:abbbbbbbbbbbb
非贪婪爬取:ab

Java中默认的就是贪婪爬取
在数量词`+` `*`的后面加上问号，此时就是非贪婪爬取

### 字符串方法

|                           方法名                           |                说明                |
| :--------------------------------------------------------: | :--------------------------------: |
|        **`public String[] matches(String regex)`**         | 判断字符串是否满足正则表达式的规则 |
| **`public String replaceAll(String regex,String newStr)`** |    按照正则表达式的规则进行替换    |
|         **`public String[] split(String regex)`**          |   按照正则表达式的规则切割字符串   |

Java中一个方法的形参是正则表达式，则这个方法识别正则表达式

### 捕获分组

分组就是一个小括号`()`

每组是有组号的，也就是序号。
规则1：从1开始，连续不间断，
规则2：以左括号为基准，最左边的是第一组，其次为第二组，以此类推

`\\`组号: 表示把第x组的内容再出来用一次

```java
// 判断一个字符串的开始字符和结束字符是否一致,只考虑一个字符
String regex1 = "(.).+\\1";
// 判断一个字符串的开始部分和结束部分是否一致,可以有多个字符
String regex2 = "(.+).+\\1";
// 判断一个字符串的开始部分和结束部分是否一致,开始部分内部每个字符也需要一致
String regex3 = "((.)\\2*).+\\1";
```

> 后续还要继续使用本组的数据：
> 正则内部使用：`\\`组号
> 正则外部使用：`$`组号

```java
// 表示把重复内容的第一个字符看做一组
// \\1;表示第一字符再次出现
// +;至少一次
// $1:表示把正则表达式中第一组的内容，再拿出来用
    "我要要学学学学编程程程"
    String result= str.replaceAll("(.)\\1+","$1");
```

### 非捕获分组

**分组之后不需要再用本组数据,仅仅是把数据括起来**

|    符号     |            含义            |        举例        |
| :---------: | :------------------------: | :----------------: |
| `(?: 正则)` |          获取所有          | `Java(?:8|11|17) ` |
| `(?= 正则)` |        获取前面部分        | `Java(?=81|11|17)` |
| `(?! 正则)` | 获取不是指定内容的前面部分 | `Java(?!8|11|17）` |

## JDK7时间

### Date时间类

Date类是一个JDK写好的avabean类，用来描述时间，精确到毫秒
利用空参构造创建的对象，默认表示系统当前时间，
利用有参构造创建的对象，表示指定的时间

创建时间对象

```java
Date date = new Date(); //系统当前时间
Date date =new Date(指定毫秒值); //时间原点开始加指定毫秒值后的时间
```
修改时间对象中的毫秒值：
 `void setTime(毫秒值);`**时间原点开始加指定毫秒值后的时间获取时间对象中的毫秒值**
`long getTime();`

### Calendar

日历类，抽象类

`static Calendar getInstance()`

国外月份减1

平闰年判断练习

```java
  public static void method1() {
        Scanner sc = new Scanner(System.in);
        Calendar calendar = Calendar.getInstance();
        int year = sc.nextInt();
        calendar.set(year, 2, 1);
        calendar.add(Calendar.DATE, -1);
        int date = calendar.get(Calendar.DATE);
        if (date == 29) {
            System.out.println("闰年");
        } else {
            System.out.println("平年");
        }
    }
```



### SimpleDateFormat 类

|                   构造方法                    |                   说明                   |
| :-------------------------------------------: | :--------------------------------------: |
|        **`public simpleDateFormat()`**        |  构造一个simpleDateFormat，使用默认格式  |
| **`public SimpleDateFormat(string pattern)`** | 构造一个simpleDateFormat，使用指定的格式 |

|                  常用方法                   |           说明           |
| :-----------------------------------------: | :----------------------: |
| **`public final string format(Date date)`** | 格式化(日期对象->字符串) |
|   **`public Date parse(string source)`**    | 解析(字符串 ->日期对象)  |

![simpledateformat](https://raw.githubusercontent.com/betteryuxuan/Image/main/simpledateformat.png)


```java
public class test02 {
    public static void main(String[] args) throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat();
        // 
        Date date1 = new Date();
        String s = sdf.format(date1);
        System.out.println(s);

        Date date2 = sdf.parse("2024/5/11 下午9:58");
        System.out.println(date2);
    }
}
```

```java
public class test02 {
    public static void main(String[] args) throws ParseException {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
        Date date1 = new Date();
        String s = sdf.format(date1);
        System.out.println(s);

        Date date2 = sdf.parse("2024-05-11 22:04:28");
        System.out.println(date2);
    }
}
```

`sdf.parse`只能解析`sdf`对应的格式

## JDK8时间

1. Date类
2. 日期格式化类：SimpleDateFormat
3. 日历类：Calendar
4. 工具类

### Zoneld时区

|                     方法名                      |           说明           |
| :---------------------------------------------: | :----------------------: |
| **`static Set\<string> getAvailableZoneIds()`** | 获取Java中支持的所有时区 |
|       **`static ZoneId systemDefault()`**       |     获取系统默认时区     |
|     **`static ZoneId of(String zoneId)()`**     |     获取一个指定时区     |
