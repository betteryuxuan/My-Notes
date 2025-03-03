# JAVA学习笔记

# java语言概述

Java语言是一种跨平台的面向对象编程语言，由Sun Microsystems（后被Oracle收购）于1995年推出。它是一种高性能、健壮、安全、简单易学的编程语言，被广泛用于开发各种类型的应用程序，从桌面应用到企业级服务器应用，再到移动应用和嵌入式系统。

以下是Java语言的主要特点和概述：

1. **面向对象**：Java是一种纯粹的面向对象编程语言，支持类、对象、封装、继承、多态等面向对象的特性。
2. **跨平台性**：Java程序可以在不同的操作系统上运行，因为它的编译器将源代码编译成中间代码（字节码），然后在Java虚拟机（JVM）上执行。JVM负责将字节码解释成特定平台的机器码，从而实现跨平台性。
3. **健壮性**：Java具有严格的数据类型检查、异常处理机制、内存管理和垃圾回收功能，使得程序更加健壮、稳定和可靠。
4. **安全性**：Java提供了安全性机制，如类加载机制、字节码校验、安全管理器等，可以防止恶意代码对系统造成损害。

# **JDK**与JRE

**JDK** (Java *D*evelopment Kit)：是Java程序开发工具包，包含JRE和开发人员使用的工具。

**jre**(Java Runtime Environment) ：是Java程序的运行时环境，包含JVM和运行时所需要的核心类库。

![jre](https://raw.githubusercontent.com/betteryuxuan/Image/main/jre.png)



> JDK = JRE + 开发工具集（例如 Javac 编译工具等）
>
> JRE = JVM + Java SE 标准类库

![OIP](https://raw.githubusercontent.com/betteryuxuan/Image/main/OIP.jpg)

# 编写执行过程

编写：将java代码编写在`.java`结尾的源文件中

编译：`.java`结尾的源文件进行编译操作。`java 原文件名.java`

运行：针对编译后的字节码文件，进行解释运行。`java 字节码文件名`

![step](https://raw.githubusercontent.com/betteryuxuan/Image/main/step.png)

# 第一份java代码解读

```java
class HelloChina{
	public static void main(String[] args){
		System.out.println("HelloChina!!你好呀");
	}
}
```

![yunxing](https://raw.githubusercontent.com/betteryuxuan/Image/main/yunxing.png)

一、编写

1. `class`关键字，表示“类”，后面跟着类名

2. Java 程序的入口是 main 方法，`public static void main(String[] args)`是Java程序的入口点.

   写法二：`public static void main(String args[])`

3. `System.out.println()`用于打印信息到控制台。

   `println`换行`print`不换行，相当于最后面加不加`\n`

4. 严格区分大小写

5. ![662E827A-FA32-4464-B0BD-40087F429E98](https://raw.githubusercontent.com/betteryuxuan/Image/main/662E827A-FA32-4464-B0BD-40087F429E98.jpg)

二、编译

1. 编译后生成1个或多个字节码文件，每一个字节码对应一个java类，并且字节码文件与类名相同。

三、运行

   `java HelloChina`编译时类名要区分大小写。

四、其他

1. 一个源文件可以有多个类，但最多只有一个类可以使用`public`声明，且要求声明为`public`的类的类名源文件名相同

# 注释

**三种注释方法**：

* 单行注释

```java
//注释文字
```

* 多行注释

  不能嵌套使用

```java
/* 

注释文字 1 

注释文字 2

注释文字 3

*/
```

* 文档注释 (Java 特有)

  > 文档注释以 **/\**** 开始，以 ***/** 结束，通常出现在类、方法、字段等的声明前面，用于生成代码文档，这种注释可以被工具提取并生成 API 文档
  >
  > 文档注释的格式通常包含一些特定的标签，如 **@param** 用于描述方法参数，**@return** 用于描述返回值，**@throws** 用于描述可能抛出的异常等等，这些标签有助于生成清晰的API文档，以便其他开发者能够更好地理解和使用你的代码。

```java
/**
 @author 指定 java 程序的作者
 @version 指定源文件的版本
*/ 
```

在命令行中使用 Javadoc 工具来生成 API 文档。可以通过 JDK 中的 `javadoc` 命令来执行。通常的格式为：

```css
javadoc [options] [packagenames] [sourcefiles] [@files]
```

- `[options]`：Javadoc 的选项，例如 `-d` 指定生成文档的目录，`-author` 包含作者信息等。
- `[packagenames]`：需要生成文档的包名。
- `[sourcefiles]`：需要生成文档的源文件。
- `[@files]`：包含了其他选项的文本文件。

例如：

```css
javadoc -d doc -author -version MyPackage
```

这个命令会生成 MyPackage 包中所有类的文档，包括作者信息和版本号，并将文档保存在 doc 目录中。

可以通过浏览器打开 `index.html` 文件来查看整个文档。

注意：Java 文档注释应该位于类、方法或字段的声明之前。

# java API文档

Java API文档是Java编程语言的官方文档，提供了关于Java标准库中各个类、接口、方法的详细信息和使用说明。

[Overview (Java SE 17 & JDK 17) (oracle.com)](https://docs.oracle.com/en/java/javase/17/docs/api/index.html)

# 关键字

Java关键字是一组被Java编程语言保留并用于特定目的的单词。这些关键字具有特殊含义，不能用作标识符（如类名、方法名、变量名等）。

1. 48个关键字：abstract、assert、boolean、break、byte、case、catch、char、class、continue、default、do、double、else、enum、extends、final、finally、float、for、if、implements、import、int、interface、instanceof、long、native、new、package、private、protected、public、return、short、static、strictfp、super、switch、synchronized、this、throw、throws、transient、try、void、volatile、while。`

2. 2个保留字：goto、const。

3. 3个特殊直接量：true、false、null。 

Java的关键字（keyword）有51+2个保留字=53个关键字

* 关键字全是小写字母

# 标识符

在 Java 中，标识符（Identifier）是用来标识各种编程元素的名称，包括变量、方法、类、接口、包等。Java 的标识符需要遵循以下规则：

1. **字符集**：标识符可以包含字母（大小写均可）、数字和下划线 `_`，但必须以字母、美元符号 `$` 或下划线 `_` 开头。
2. **大小写敏感**：标识符是区分大小写的，也就是说 `myVariable` 和 `MyVariable` 是不同的标识符。
3. **长度限制**：标识符的长度理论上是没有限制的，但实际上应根据编程规范和代码可读性来合理命名，通常建议不要过长。
4. **保留字**：标识符不能与 Java 的保留字相同，Java 的保留字是被编程语言保留用于特定用途的关键字，例如 `if`、`else`、`while` 等。
5. **规范命名**：标识符应该采用驼峰命名法（Camel Case）或者下划线分隔法（Snake Case）来命名，以提高可读性。驼峰命名法是指除了第一个单词之外，后续单词的首字母大写，例如 `myVariableName`；下划线分隔法是指单词之间用下划线 `_` 分隔，例如 `my_variable_name`。
6. **包名约定**：包名应该使用小写字母，多个单词之间使用点 `.` 分隔，例如 `com.example.package`。
7. **类名，接口名**：应该使用大写字母开头的大驼峰命名法，例如 `MyClass`。
8. **变量名，方法名**：第二个单词开始每个单词大写的小驼峰命名法。
9. **常量名约定**：常量名应该使用全部大写字母，并用下划线 `_` 分隔单词，例如 `MAX_VALUE`。

# 数据类型

**一、基本数据类型**

1. **整型**：byte \ short \ int \ long

* 声明`long`类型变量是，需要提供后缀，需要使用后缀`L`或者`l`。

```java
long myLongVariable = 10000000000L;
```
2. **浮点型**： float \ double


* 声明`float`类型的变量时，需要使用后缀`F`或者`f`。例如：

```java
float myFloatVariable = 3.14F;
```

3. **字符型**：char

​	`char`类型表示一个16位的`Unicode`字符，它占用2个字节的存储空间。

 * 直接'',里面只能一个字符

 * 将Unicode编码值赋给`char`类型的变量。在Java中，可以使用`\u`前缀来表示Unicode编码值。例如：

   ```java
   char myChar = '\u0041'; // 这里的\u0041表示十六进制编码值为41的字符，即'A'
   ```

   这样，`myChar`变量将被赋值为大写字母'A'。

 * 使用转义字符来表示。一些常见的转义字符包括：

   - `\t`: 制表符（Tab）
   - `\n`: 换行符（Newline）
   - `\r`: 回车符（Carriage return）
   - `\'`: 单引号
   - `\"`: 双引号
   - `\\`: 反斜杠

   例如，要表示换行符，可以使用`\n`：

   ```java
   char newlineChar = '\n';
   ```

 * 使用具体字符对应ASCII码值

3. **布尔型**：boolean

* 在Java中不能使用数字1或0来代替布尔值，与C语言不同。
* 在Java中，布尔类型（`boolean`）的占用空间大小并没有明确定义。

**二、自动类型提升规则**

规则：将取值范围小（或容量小）的类型自动提升为取值范围大（或容量大）的类型 。

![Snipaste_2024-04-18_21-21-45](https://raw.githubusercontent.com/betteryuxuan/Image/main/Snipaste_2024-04-18_21-21-45.png)

1. 当把存储范围小的值（常量值、变量的值、表达式计算的结果值）赋值给存储范围大的变量时
2. 当存储范围小的数据类型与存储范围大的数据类型变量一起混合运算时，会按照其中最大的类型运算
3. 当 byte,short,char 数据类型的变量进行算术运算时，按照 int 类型处理。

**三、引用数据类型**

1. **类（Class）**
2. **接口（Interface）**
3. **数组（Array）**

# String

**一、概述**

1. 是一种引用数据类型

2. 使用“”进行赋值

   ```java
   String myString = "Hello, World!";
   ```

3. String声明的字符串可以包含0到多个字符

**二、String与基本数据类型的变量间的运算**

1. 任意八种基本数据类型的数据与 String 类型只能进行连接“+”运算，且结果一定也是 String 类型
2. String 类型不能通过强制类型()转换，转为其他的类型

#  运算符

![1713583471888](https://raw.githubusercontent.com/betteryuxuan/Image/main/1713583471888.jpg)

与C语言相似，java多了一个`>>>`，无符号右移

在C语言中，`>>` 运算符是算术右移。算术右移是针对有符号整数的右移操作，它会保留符号位，并在左侧用符号位填充空缺位。

在java中，`>>`与C一致，对比一下java中`>>`与`>>>`

1. **`>>`（有符号右移）**：

   使用 `>>` 运算符时，对于正数，它的操作结果与无符号右移 `>>>` 相同。对于负数，则会在左侧用1填充空缺位，`>>` 右移后会保持负数的符号。

2. **`>>>`（无符号右移）**：

   使用 `>>>` 运算符时，无论是正数还是负数，都会在左侧用0填充空缺位。`>>>` 右移操作不保留符号位，因此结果总是正数。

 举例：

`x = -8` `1111 1111 1111 1111 1111 1111 1111 1000`

`x >>> 1` 结果为 `0111 1111 1111 1111 1111 1111 1111 1100`，即 `2147483644`。

**右移后会在左侧用0填充空缺位，结果总是正数**

其他注意：

1. 结果与被模数符号相同

   > 举例：
   >
   > 5 % -2 = 1
   >
   > -5 % 2 =-1
   >
   > -5 % -2 = -1

2. “+”号的两种用法

   第一种：对于*+*两边都是数值的话，*+*就是加法的意思

   第二种：对于*+*两边至少有一边是字符串的话，*+*就是拼接的意思

3. 逻辑运算符

   ![yunsuanfu](https://raw.githubusercontent.com/betteryuxuan/Image/main/yunsuanfu.png)

4. 位运算

      ![weiyunsuan](https://raw.githubusercontent.com/betteryuxuan/Image/main/weiyunsuan.png)
# 键盘录入
> `Scanner`是Java中的一个类，位于`java.util`包中。它主要用于从各种输入源（如键盘、文件等）中获取用户输入的数据。`Scanner`类提供了多种方法，可以方便地读取不同类型的数据，例如整数、浮点数、字符串等。

**使用scanner获取不同类型的步骤**

      1. 导包：找到这个类`import java.unti.Scanner`
      2. 创建对象：创建一个Scanner类的实例
      3. 接收数据：调用Scanner类中的方法，获取指定类型的变量

```java
import java.util.Scanner;
class ScannerDemo{
	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		System.out.print("输入一个数字：");
		int i = sc.nextInt();
		System.out.println(i);
	}
}
```
# 流程控制语句

   `if-else`，`Switch`，`while`，`for`，`do-while`基本用法与C语言一致

- switch 语句中的变量类型可以是： byte、short、int 或者 char。从 Java SE 7 开始，switch 支持字符串 String 类型了，同时 case 标签必须为字符串常量或字面量。也可以接受枚举类型的表达式。

语句只有一行，可以省略大括号


```java
import java.util.Scanner;
class SwitchDemo{
	public static void main(String[] args){
		Scanner sc = new Scanner(System.in);
		System.out.print("输入一个数字：");
		int num = sc.nextInt();
		switch (num) {
		case 1 -> System.out.println("这是1");
		case 2 -> System.out.println("这是2");
		default -> System.out.println("啥都不是");
		}
	}
}
```

# 数组

数组是一种容器

在Java中，数组是一种用来存储相同类型数据的数据结构。数组可以存储基本数据类型（如整数、浮点数等）或者引用类型（如对象）。

**一、定义与静态初始化**

以下是关于Java数组的一些重要特点和用法：

1. **声明数组**：在Java中声明数组需要指定数组的类型和数组名，语法如下：

   ```java
   // 声明一个整型数组
   int[] numbers;
   
   // 声明一个字符串数组
   String[] names;
   ```

2.  **初始化数组**：

   完整格式：

   `数据类型 [ ] 数组名 = new 数据类型 [ ] {元素1， 元素2...};`

   ```c
   int[] numbers = new int[] {10, 20, 30, 40, 50};
   ```

   简化格式：

   `数据类型 [ ] 数组名 ={元素1， 元素2...};`

   ```java
   int[] numbers = {10, 20, 30, 40, 50};
   ```

在Java中，直接输出数组名得到数组的引用（地址）

```java
int[] numbers = {1, 2, 3, 4, 5};
System.out.println(numbers); // 输出数组引用
```

![arr dizhi](https://raw.githubusercontent.com/betteryuxuan/Image/main/arr%20dizhi.png)

1. **`[`**：表示这是一个数组。
2. **`I`**：表示数组中元素的数据类型。在这里，`I` 代表整型（`int`）数组。Java 中的其他数据类型对应的标识符有：`[B`（字节数组）、`[S`（短整型数组）、`[J`（长整型数组）、`[F`（浮点型数组）、`[D`（双精度浮点型数组）、`[C`（字符数组）、`[Z`（布尔型数组）等。
3. **`@`**：表示分隔符，用于分隔类型标识符和哈希码。
4. **`5acf9800`**：表示数组对象的哈希码，即数组在内存中的地址值的十六进制表示。

**二、数组元素访问**

```c
 System.out.println("第一个元素：" + numbers[0]);
```

与C语言一致

**三、动态初始化**

`数据类型 [ ] 数组名 = new 数据类型 [数组长度] ;`

数组默认初始化数值：

- 对于基本数据类型的数组：
  - `byte`, `short`, `int`, `long`: 默认初始化为0。
  - `float`, `double`: 默认初始化为0.0。
  - `char`: 默认初始化为 `\u0000`，即空字符。
  - `boolean`: 默认初始化为 `false`。
- 对于引用数据类型的数组（如对象数组）：
  - 默认初始化为 `null`，即表示没有指向任何对象。

**四、内存**

堆主要用于存储对象和数组，而栈主要用于存储方法调用和局部变量。

对于数组来说：

1. 只要是new出来的一定在堆里开辟了一个小空间
2. new了多次，就有多个小空间

**两个数组指向同一个空间的情况：**

`array1` 和 `array2` 实际上指向相同的数组对象。因此，当我们通过 `array1` 修改数组的第一个元素为10时，实际上也修改了数组对象，所以 `array2` 中的第一个元素也会变为10。

```java
public class Main {
    public static void main(String[] args) {
        int[] array1 = {1, 2, 3, 4, 5};
        int[] array2 = array1; // array2指向array1所指向的数组

        // 修改array1的第一个元素
        array1[0] = 10;

        // 输出array2的第一个元素
        System.out.println("array2[0] = " + array2[0]); // 输出为10
    }
}
```

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

**一、格式**

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

**二、带参数及返回值的方法**

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

**三、方法的重载**

> 方法的重载是指在同一个类中可以定义多个同名的方法，但它们的参数列表（**参数类型、参数个数、参数顺序**）必须不同。重载方法可以提高代码的灵活性和可读性，使得方法名可以更加直观地反映其功能。

Java中的方法重载必须满足以下规则：

1. 方法名必须相同。
2. 参数列表必须不同（参数类型、参数个数、参数顺序），可以有不同的类型和个数的参数，也可以有不同顺序的参数。
3. 方法的返回类型可以相同也可以不同。
4. 与**方法的返回值**、方法的访问修饰符和方法抛出的异常无关。
5. 在同一个类中。

**四、方法的内存**

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

**一、静态初始化**


```java
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};
```

**二、动态初始化**

```java
dataType[][] arrayName = new dataType[rows][columns];
```

```java
dataType[][] arrayName = [rows][columns];
```

- 在 Java 中，二维数组是由一系列一维数组组成的，每个一维数组可以具有不同的长度。Java 的多维数组实际上是数组的数组，每个数组元素都是一个引用，指向另一个数组。

# 面向对象

**一、类和对象**

类：共同特征的描述。

对象：是真实存在的具体示例。

**二、如何定义类**

```java
public class 类名 {
        1.成员变量
        2.成员方法
        3.构造器
        4.代码块
        5.内部类
}
```
**三、如何得到对象**

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

**四、注意**

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

**一、封装的优点**

- 良好的封装能够减少耦合。
- 类内部的结构可以自由修改。
- 可以对成员变量进行更精确的控制。
- 隐藏信息，实现细节。

**二、private关键字**

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

**三、成员变量和局部变量**

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

**一、作用**

创建对象时，由虚拟机自动调用，给成员变量进行初始化的

1. **初始化对象的状态：** 构造方法用于初始化对象的成员变量，确保对象在创建后处于一个合理的状态。
2. **提供对象的初始化选项：** 构造方法可以带有参数，允许在创建对象时传递参数来指定对象的初始状态。
3. **创建对象：** 在 Java 中，使用 `new` 关键字来创建对象时，会自动调用该对象的构造方法来初始化对象。

**二、类型**

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
**三、特点**

1. 方法名与类名相同，大小写也要一致

2. 没有返回值类型，连void都没有

3. 没有具体的返回值(不能由return带回结果数据)

**四、执行时机**

1. 创建对象的时候由虚拟机调用，不能手动调用构造方法
2. 每创建一次对象，就会调用一次构造方法

**五、定义**

* 如果没有定义构造方法，系统会给出**一个默认的无参数构造方法**
* 如果定义了构造方法，系统**不再提供默认的无参数构造方法**

**六、重载**

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
>  1. `alt` + `insert`/鼠标右键生成(generate)：构造函数获得构造方法，`getter and setter`设置get和set成员方法
>  2. 插件ptg：右键`ptg to javabean`一键生成

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

| 方法                                  | 功能                                   |
| ------------------------------------- | -------------------------------------- |
| `indexOf(char)`                       | 返回指定字符的索引                     |
| `charAt(int)`                         | 返回指定索引处的字符                   |
| `replace(char oldChar, char newChar)` | 字符串替换                             |
| `trim()`                              | 去除字符串两端空白                     |
| `split(String regex)`                 | 分割字符串，返回一个分割后的字符串数组 |
| `getBytes()`                          | 返回字符串的 byte 类型数组             |
| `length()`                            | 返回字符串长度                         |
| `toLowerCase()`                       | 将字符串转成小写字母                   |
| `toUpperCase()`                       | 将字符串转成大写字母                   |
| `substring(int beginIndex)`           | 截取字符串                             |
| `equals(Object obj)`                  | 字符串比较                             |

1. 字符串比较

`==`对于基本数据类型比较数据值，对于引用数据类型比较地址值

**字符串内容比较：**

1.  **`boolean equals`**：完全一样返回`true`
2.  **`boolean equalsIgnoreCase`**：忽略大小写

键盘输入的也是new出来的

2. **字符串遍历**

1. **`public char charAt(int index)`**：根据索引返回字符
2. **`public int length()`**：返回字符串长度

> 数组名.length
>
> 字符串对象.length()

3. 字符串截取

1. **`String substring(int beginIndex, int endIndex)`**: 这个方法返回一个新的字符串，包含从 `beginIndex` 到 `endIndex - 1` 之间的字符。即提取的子串是从 `beginIndex` 开始的，直到但不包括 `endIndex` 位置的字符。
2. **`String substring(int beginIndex)`**: 这个方法返回一个新的字符串，包含从 `beginIndex` 到字符串末尾的所有字符。即提取的子串是从 `beginIndex` 开始一直到字符串的末尾。

4. 字符串替换

1. **`String replace(旧值，新值)`**：返回值是替换后结果

## SpringBulider

当对字符串进行修改的时候，需要使用 StringBuffer 和 StringBuilder 类。

和 String 类不同的是，StringBuffer 和 StringBuilder 类的对象能够被多次的修改，并且不产生新的未使用对象。

StringBuilder可以看做一个容器（一个动态大小的字符数组）。

`StringBuilder` 允许修改字符串内容，而不会产生新的字符串对象，从而提高了字符串操作的效率。

**一、构造方法**

|               方法名               |                      说明                      |
| :--------------------------------: | :--------------------------------------------: |
|      `public StringBuilder()`      | 才能构建一个空白可变字符的对象，不含有任何内容 |
| `public StringBuilder(String str)` |      根据字符串内容，来创建可变字符串对象      |
|  `  StringBuilder(int capacity)`   | 创建一个具有指定初始容量的StringBuilder对象。  |

**二、常用方法**

|                 方法名                  |                     说明                      |
| :-------------------------------------: | :-------------------------------------------: |
| `public StringBuilder append(任意类型)` |            添加数据，返回对象本身             |
|    `public StringBuilder reverse()`     |               翻转容器中的内容                |
|          `public int length()`          |                   返回长度                    |
|        `public Sting toString()`        | 通过该方法实现把`StringBuilder`转换为`String` |

**链式编程**

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

**一、构造方法**

|                     方法名                      |                             说明                             |
| :---------------------------------------------: | :----------------------------------------------------------: |
|          **`StringJoiner(分隔符号)`**           |          创建一个StringJoiner对象，使用指定间隔符号          |
| **`StringJoiner(分隔符号,开始符号，结束符号)`** | 创建一个StringJoiner对象，使用指定间隔符号，开始符号，结束符号 |

**二、成员方法**

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

### 内存存储

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
   - `final` 修饰的方法可以被继承，不能被子类**重写**（覆盖），确保方法的行为不会被子类改变。
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

## 可见度

**现该接口的类中对应的方法的可见性大于等于接口方法的可见性**

* 实现类可以具有和接口中方法相同的可见性，或者比接口方法更严格的可见性。换句话说，实现类中实现接口的方法可以是 `public`、`protected` 或默认（包级访问），但不能是 `private`。
* 如果实现类中实现接口的方法的可见性比接口方法更严格，例如将接口方法声明为 `public` 而实现类中将其声明为 `protected` 或默认（包级访问），这样会造成编译错误。

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

2. 在外部类编写方法，对外提供内部类对象

3. 当成员内部类被**非私有**修饰时，直接创建对象，

   `Outer.inner oi= new Outer().new inner()`

* 当成员内部类被非私有修饰时，直接创建对象

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

| 符号     | 含义                                                       |
| -------- | ---------------------------------------------------------- |
| **`.`**  | **任意单个字符，除换行符外**                               |
| **`[]`** | **[ ] 中的任意一个字符**                                   |
| **`-`**  | **[ ] 内表示字符范围**                                     |
| **`^`**  | **在[ ]内的开头，匹配除[ ]内的字符之外的任意一个字符**     |
| **`| `** | **或**                                                     |
| **`\`**  | **将下一字符标记为特殊字符、文本、反向引用或八进制转义符** |

字符类（只匹配一个字符）

|         用法         |                    注释                    |
| :------------------: | :----------------------------------------: |
|     **`[abc]`**      |            **只能是a，b，或c**             |
|     **`[^abc]`**     |       **除了a，b，c之外的任何字符**        |
|    **`[a-zA-Z]`**    |         **a到z A到Z，包括(范围)**          |
|   **`[a-d[m-p]]`**   |              **a到d，或m到p**              |
|  **`[a-z&&[def]]`**  |      **a-z和def的交集。为：d，e，f**       |
| **`[a-z&&[ ^bc ]]`** |    **a-z和非bc的交集。(等同于[ad-z])**     |
| **`[a-z&&[ ^m-p ]`** | **a到z和除了m到p的交集。(等同于[a-lq-z])** |

预定义字符（只匹配一个字符）

| **`.`**  |    **任何字符**    |       **任何字符，\n不匹配**       |
| :------: | :----------------: | :--------------------------------: |
| **`\d`** |  **数字字符匹配**  |        **一个数字：[0-9]**         |
| **`\D`** | **非数字字符匹配** |        **非数字：[ ^0-9]**         |
| **`\s`** |  **匹配空白字符**  |  **一个空白字符：[\t\n\x0B\f\r]**  |
| **`\S`** | **匹配非空白字符** |       **非空白字符：[ ^\s]**       |
| **`\w`** |  **匹配单词字符**  | **[a-zA-Z_0-9]英文、数字、下划线** |
| **`\W`** | **匹配非单词字符** |      **[^\w]一个非单词字符**       |

|   **`?`**   |       **0次或1次**       |
| :---------: | :----------------------: |
|   **`*`**   |      **0次或多次**       |
|   **`+`**   |      **1次或多次**       |
|  **`{n}`**  |       **正好n次**        |
| **`{n,}`**  |       **至少n次**        |
| **`{n,m}`** |   **至少n但不超过m次**   |
| **`(?i)`**  | **忽略后面字符的大小写** |
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

# 包装类

> 用一个对象，把基本数据类型包起来

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

|                         方法名                          |                   说明                    |
| :-----------------------------------------------------: | :---------------------------------------: |
|             **`public Integer(int value)`**             |     根据传递的整数创建一个Integer对象     |
|             **`public Integer(string s)`**              |    根据传递的字符串创建一个Integer对象    |
|       **`public static Integer valueof(int i)`**        |     根据传递的整数创建一个Integer对象     |
|      **`public static Integer valueof(string s)`**      |    根据传递的字符串创建一个Integer对象    |
| **`public static Integer valueof(string s,int radix)`** | 根据传递的字符串和进制创建一个Integer对象 |



使用`valueOf`方法创建一个包装对象时，如果参数在[-128, 127]范围内，则会返回缓存中的对象，而不是新创建一个对象。提前放到一个数组里面，因为这个范围内的数据用的比较多

包装类如何进行计算：

**对象之间是不能直接进行计算**

* JDK5前

**步骤：**

1. 把对象进行拆箱，变成基本数据类型
2. 相加
3. 把得到的结果再次进行装箱(再变回包装类)

```java
Integer a = new Integer(10); //利用构造方法
Integer b = Integer.valueof(20); //利用静态方法valueOf

// 拆箱，将Integer对象转换为int类型
int sum = a.intValue() + b.intValue();

// 装箱，将int类型转换回Integer对象
Integer result = Integer.valueOf(sum);

System.out.println(result);
```

* JDK5：

JDK5时提出了一个机制：**自动装箱和自动拆箱**
自动装箱：把基本数据类型会自动的变成其对应的包装类
自动拆箱：把包装类自动的变成其对象的基本数据类型
在底层，此时还会去自动调用静态方法`valueof`得到一个`Integer`对象，只不过这个动作不需要我们自己去操作了

```java
public class test1 {
    public static void main(String[] args) {
        //自动装箱
        Integer i1 = 10;
        Integer i2 = 15;

        Integer i3 = i1 + i2;

        System.out.println(i1);
        System.out.println(i2);
        System.out.println(i3);
        
        //自动拆箱
        int a = i2;
        System.out.println(a);
    }

```

|                     方法名                      |                说明                 |
| :---------------------------------------------: | :---------------------------------: |
| **`public static Sting toBinaryString(int i)`** |             得到二进制              |
| **`public static String toOctalString(int i)`** |             得到八进制              |
|  **`public static String toHexString(int i)`**  |            得到十六进制             |
|   **`public static int parselnt(String s)`**    | 将字符串类型的整数转成int类型的整数 |

八种包装类型中，除了`Character`都有对应的`ParseXxx`的方法进行类型转换



键盘录入一整行数据，读入空格，遇到回车停止

```java
public class test1 {
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        String s = sc.nextLine();

        Integer i = Integer.parseInt(s);

        System.out.println(i);
        System.out.println(i + 1);
    }
}
```

# Arrays


|                            方法名                            |             说明             |
| :----------------------------------------------------------: | :--------------------------: |
|          **`public static String toString(数组)`**           |  **把数组拼接成一个字符串**  |
|    **`public static int binarySearch(数组, 查找的元素)`**    |    **二分查找法查找元素**    |
|     **`public static int[] copyOf(原数组, 新数组长度)`**     |         **拷贝数组**         |
| **`public static int[] copyOfRange(原数组, 起始索引, 结束索引)`** |    **拷贝数组(指定范围)**    |
|          **`public static void fill(数组, 元素)`**           |         **填充数组**         |
|             **`public static void sort(数组)`**              | **按照默认方式进行数组排序** |
|        **`public static void sort(数组, 排序规则)`**         |      按照指定的规则排序      |

都是静态方法，直接`Array.sort(数组)`使用

`public static void sort(数组, 排序规则)`细节：

只能给引用数据类型排序，基本数据类型使用包装类

使用插入排序+二分查找：

> o1-o2升序排列
>
> o2-o1降序排列

```java
import java.util.Arrays;
import java.util.Comparator;

public class ArraysDemo {
    public static void main(String[] args) {
        Integer[] arr = {0, 2, 4, 6, 8, 1, 3, 5, 7, 9};
        //匿名内部类,Comparator是java.util包中接口
        Arrays.sort(arr, new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                return o1 - o2;
            }
        });
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}
```

# lambda表达式

**函数式编程**是一种思想，忽略面向对象的复杂语法

强调做什么，而不是谁去做

* 简化匿名内部类的书写
* 只能简化函数式接口的匿名内部类的写法
* 函数式接口：有且仅有一个抽象方法的接口，接口上可以加`@FunctionalInterface`注解

```java
import java.util.Arrays;

public class ArraysDemo {
    public static void main(String[] args) {
        Integer[] arr = {0, 2, 4, 6, 8, 1, 3, 5, 7, 9}; 
        Arrays.sort(arr, (Integer o1, Integer o2) -> {
                    return o1 - o2;
                }
        );
        // 使用Lambda表达式代替Comparator
        for (int i = 0; i < arr.length; i++) {
            System.out.print(arr[i] + " ");
        }
    }
}

```

省略规则

1. 参数类型可以省略不写
2. 只有一个参数，参数类型可以省略不写，`()`可以省略
3. 方法体只有一行，`大括号，分号，return`可以不写，但必须同时省略不写

```java
Arrays.sort(arr, (o1, o2) -> o1 - o2);
```

# 泛型

泛型：是JDK5中引入的特性，可以在编译阶段约束操作的数据类型，并进行检查
泛型的格式：<数据类型>
注意：泛型只能支持引用数据类型

* **没有泛型的情况：**
  在没有泛型的情况下，Java 集合会被认为是可以存储任意对象类型的集合。这是因为集合在不指定类型的情况下，默认使用 `Object` 类型。这种方式在存储数据时非常灵活，但在取出数据时会带来一些问题。

例如，假设我们有一个没有使用泛型的 `ArrayList`：

```java
public class Main {
    public static void main(String[] args) {
        ArrayList list = new ArrayList();
        list.add("Hello");
        list.add(123);
        list.add(45.67);

        // 由于集合没有指定类型，所有添加到集合中的数据都会被认为是Object类型
        for (Object obj : list) {
            System.out.println(obj);
        }
    }
}
```

在这个例子中，我们可以往 `ArrayList` 中添加不同类型的元素，因为集合认为所有元素都是 `Object` 类型java。

然而，这种灵活性带来了一个明显的坏处：我们在获取数据时，无法直接使用这些对象的特有行为，因为我们不知道它们的实际类型，需要进行类型转换（类型强制转换）。

### 好处

* 统一数据类型。

* 把运行时期的问题提前到了编译期间，避免了强制类型转换可能出现的异常，因为在编译阶段类型就能确定下来。

### 泛型的擦除

**Java中的泛型是伪泛型**

编译成字节码文件后，泛型消失。

泛型类型信息在编译后会被移除，转而使用 `Object` 或其上界类型进行替代。这种实现方式确保了泛型的向后兼容性，

### 泛型类

使用场景：当一个类中，某个变量的数据类型不确定时，就可以定义带有泛型的类

格式：

```java
修饰符 class 类名<类型>{
}
```

```java
public class MyArrayList<E> {
    Object[] obj = new Object[10];
    int size;

    public boolean add(E e) {
        obj[size] = e;
        size++;
        return true;
    }

    public E get(int index) {
        return (E) obj[index];
    }

    @Override
    public String toString() {
        return Arrays.toString(obj);
    }
}
```



### 泛型方法

方法中形参类型不确定时

1. 使用类名后面定义的泛型（所有方法都能用）
2. 在方法申明上定义自己的泛型（只有本方法能用）

格式：

```java
修饰符<类型>返回值类型 方法名(类型 变量名){
}
```

```java
public class ListUtil {
    public static <E> void addAll(ArrayList<E> list, E e1, E e2, E e3) {
        list.add(e1);
        list.add(e2);
        list.add(e3);
    }
}
```

```java
public class ListUtil {
    public static <E> void addAll(ArrayList<E> list, E...e) {
        for (E element : e) {
            list.add(element);
        }
    }
}
```

### 泛型接口

格式：

```java
修饰符 interface 接囗名<类型>{
}
```

重点：如何使用一个带泛型的接口？

方法一：实现类给出具体类型

```java
public class MyArrayList implements List<string>{
}
```

```java
myArrayList list2 = new myArrayList();
```

方法二：实现类延续泛型，创建对象时再确定

```java
public class MyArrayList <E> implements List<E>{
}
```

```java
myArrayList<String> list2 = new myArrayList<>();
```

### 继承和通配符

有两种限定通配符：

> ⼀种是< ? extends T > 它通过确保类型必须是 T 的⼦类来设定类型的上界，
>
> 另⼀种是< ? super T >它通过确保类型必须是 T 的⽗类来设定类型的下界。

泛型类型必须⽤限定内的类型来进⾏初始化，否则会导致编译错误。

< ? > 表示了非限定通配符，因为 < ? > 可以⽤任意类型来替代

泛型不具备继承性，但是数据具备继承性

**泛型通配符的基本用法**

1. **不确定的类型：`?`**

   `?` 表示不确定的类型，可以用于表示任何类型。相当于`E`

   ```java
   public void printList(List<?> list) {
       for (Object elem : list) {
           System.out.println(elem);
       }
   }
   ```

2. **限定类型的上界：`? extends E`**

   `? extends E` 表示可以传递类型 `E` 或 `E` 的子类。

   ```java
   public void printNumbers(List<? extends Number> list) {
       for (Number num : list) {
           System.out.println(num);
       }
   }
   ```

3. **限定类型的下界：`? super E`**

   `? super E` 表示可以传递类型 `E` 或 `E` 的父类。

   ```java
   public void addNumbers(List<? super Integer> list) {
       list.add(new Integer(10));
   }
   ```

**应用场景**

1. **定义泛型类、方法和接口**

   当类型不确定时，可以定义泛型类、方法和接口。

   ```java
   public class GenericBox<T> {
       private T content;
   
       public void setContent(T content) {
           this.content = content;
       }
   
       public T getContent() {
           return content;
       }
   }
   ```

2. **使用泛型通配符限定类型范围**

   当类型不确定，但能确定是在某个继承体系中时，可以使用泛型通配符。

# 树（前置知识）

## 平衡二叉树

**平衡二叉树（左旋）**

确定支点：从添加的节点开始，不断的往父节点找不平衡的节点
以不平衡的点作为支点把支点左旋降级，变成左子节点，晋升原来的右子节点
以不平衡的点作为支点，将根节点的右侧往左拉
原先的右子节点变成新的父节点，并把多余的左子节点出让，给已经降级的根节点当右子节点
<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/4a77056476a611aac8d7afd78072e9ea.png" alt="img" style="zoom: 50%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/b2b9b3f580a2dd599bd72bd0a89b5d78.png" alt="img" style="zoom:50%;" />
**平衡二叉树（右旋）**
以不平衡的点作为支点,就是将根节点的左侧往右拉,原先的左子节点变成新的父节点，并把多余的右子节点出让，给已经降级的根节点当左子节点
<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/97ba3964cf78c96bad3a0dc8bba6f4ea.png" alt="img" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/a5d042c136e6e5a9757195e21784fc33.png" alt="img" style="zoom: 67%;" />
数据结构（平衡二叉树）需要旋转的四种情况

1. 左左

2. 左右

3. 右右

4. 右左

1.左左：（1次旋转）
   当根节点左子树的左子树有节点插入，导致二叉树不平衡
   <img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/df0174f06a98efdc8628993078fb489a_720.png" alt="img" style="zoom: 33%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/f8246b76fb15b75ec054f6eca0b06cd6.png" alt="img" style="zoom:33%;" />
2.左右：（2次旋转）
   当根节点左子树的右子树有节点插入，导致二叉树不平衡
   <img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/bac56f3e9a7b7631a276c2d7144db584.png" alt="img" style="zoom: 67%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/a185ecc3594dfcb81396c80e9175a4d0.png" alt="img" style="zoom: 50%;" />

![img](https://raw.githubusercontent.com/betteryuxuan/Image/main/c3da591fabc1ee2e9e1f3c6412188e4c.png)
   3.右右：（1次旋转）
   当根节点右子树的右子树有节点插入，导致二叉树不平衡
   <img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/ba02579cc2ad7c74f286366156b76399.png" alt="img" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/ceac421669f840f532b5b779b804ec7d.png" alt="img" style="zoom:50%;" />
   4.右左：（2次旋转）
   当根节点右子树的左子树有节点插入，导致二叉树不平衡
   <img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/0888401fb6ad760b5cf9500b31ce367d.png" alt="img" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/c355b48363727e9678e1517bd4070b68.png" alt="img" style="zoom:50%;" />

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/ce0f85f2210a35499df93a0f1a4ff290.png" alt="img" style="zoom:50%;" />

## 红黑树

> * 红黑树是一种自平衡的二叉查找树，是计算机科学中用到的一种数据结构
> * 1972年出现，当时被称之为平衡二叉B树。后来，1978年被修改为如今的"红黑树"
> * 它是一种特殊的二叉查找树，红黑树的每一个节点上都有存储位表示节点的颜色
> * 每一个节点可以是红或者黑；红黑树不是高度平衡的，它的平衡是通过"红黑规则"进行实现的数据结构(红黑树)红黑

规则

1. 每一个节点或是红色的，或者是黑色的

2. 根节点必须是黑色

3. 如果一个节点没有子节点或者父节点，则该节点相应的指针属性值为Nil，这些Nil视为叶节点，每个叶节点(Nil)是黑色的④如果某一个节点是红色，那么它的子节点必须是黑色(不能出现两个红色节点相连的情况)

4. 对每一个节点，从该节点到其所有后代叶节点的简单路径上，均包含相同数目的黑色节点添加节点规则

   

    默认颜色：添加节点默认是红色的（效率高）
      ![img](https://raw.githubusercontent.com/betteryuxuan/Image/main/fabfb87364711555b9193523dbb361d6_720.png)

# 集合

# 单列集合

## Collection

在Java集合框架中，单列集合的顶层接口是 `java.util.Collection`。这个接口是所有单列集合接口和类的根接口，定义了一些集合的基本操作方法。`Collection` 接口下有两个主要子接口：`List` 和 `Set`，它们又有各自的具体实现类。

**集合体系结构**：

![list](https://raw.githubusercontent.com/betteryuxuan/Image/main/List.jpg)

List：添加的元素有序，可重复，有索引

Set：添加的元素无序，不重复，无索引

### 常用方法

Collection是单列集合的祖宗**接口**，它的功能是全部单列集合都可以继承使用的。

|               方法名称                |               说明               |
| :-----------------------------------: | :------------------------------: |
|       `public boolean add(E e)`       |   把给定的对象添加到当前集合中   |
|         `public void clear()`         |       清空集合中所有的元素       |
|     `public boolean remove(E e)`      | 把**给定的对象**在当前集合中删除 |
| `public boolean contains(Object obj)` | 判断当前集合中是否包含给定的对象 |
|      `public boolean isEmpty()`       |       判断当前集合是否为空       |
|          `public int size()`          | 返回集合中元素的个数/集合的长度  |

1. `add`添加元素
   **细节1**：

   如果我们要往List系列集合中添加数据，那么方法永远返回true，因为List系列的是允许元素重复的。
   **细节2：**

   如果我们要往Set系列集合中添加数据，如果当前要添加元素不存在，方法返回true，表示添加成功。
   如果当前要添加的元素已经存在，方法返回false，表示添加失败。
   因为set系列的集合不允许重复

2. `remove`

   删除成功返回ture，删除失败返回`false`

3. `contains`
   `contains` 方法的实现依赖于对象的 `equals` 方法来判断对象是否相等。
   如果存的是自定义对象，没有重写`equals`方法，那么默认使用`obiect`类中的`equals`方法进行判断

   **这种默认的 `equals` 方法比较的是对象的引用地址，即判断对象是否是同一个对象。**
   **如果需要判断内容相同，就需要在自定义的`Javabean`类中重写`equals`方法**

   ```java
   System.out.println(coll.contains(s4));
   ```

### 三种遍历方法

**迭代器：在遍历的过程中需要删除元素，使用迭代器**
**增强for、Lambda：仅仅想遍历，那么使用增强for或Lambda表达式**

#### for循环

```java
for (int i = 0; i < list.size(); i++) {
    System.out.print(list.get(i) + "，");
}
```

#### 迭代器

> 迭代器（Iterator）是一种设计模式，用于提供一种顺序访问聚合对象（如列表、集合、映射等）中的各个元素的方法，而不需要暴露其内部表示。
>
> 在Java中，迭代器是通过`java.util.Iterator`接口来实现的。迭代器提供了一系列用于遍历集合元素的方法，包括检查是否有下一个元素、获取下一个元素、移除当前元素等。
>
> 迭代器通常通过调用集合对象的`iterator()`方法来获取，然后使用`hasNext()`方法检查是否有下一个元素，使用`next()`方法获取下一个元素，使用`remove()`方法删除当前元素。

* 迭代器在Java中的类是Iterator，迭代器是集合专用的遍历方式。

* Collection集合获取迭代器

|         方法名称         |                  说明                   |
| :----------------------: | :-------------------------------------: |
| `Iterator<E> iterator()` | 返回迭代器对象，默认指向当前集合的0索引 |

```java
Iterator<String> it = list.iterator();
```

|      方法名称       |                           说明                            |
| :-----------------: | :-------------------------------------------------------: |
| `boolean hasNext()` | 判断当前位置是否有元素，有元素返回true，没有元素返回false |
|     `E next()`      |    获取当前位置的元素，并将迭代器对象移向下一个位置。     |

迭代器遍历相关的三个方法：

```java
Iterator<E> iterator() //获取一个迭代器对象
boolean hasNext() // 判断当前指向的位置是否有元素
E next() // 获取当前指向的元素并移动指针
```
示例代码：
```java
import java.util.ArrayList;
import java.util.Iterator;
import java.util.List;

public class test01 {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("aaa");
        list.add("bbb");
        list.add("ccc");

        Iterator<String> it = list.iterator();
        while (it.hasNext()) {
            String str = it.next();
            System.out.println(str);
        }
    }
}
```

* **注意事项**
  1. **到达最后报错 NoSuchElementException**：当使用 `next()` 方法获取下一个元素时，如果当前位置没有元素（即已经到达迭代器的末尾），则会抛出 `NoSuchElementException` 异常。因此，在使用 `next()` 方法之前，应该先使用 `hasNext()` 方法检查是否有下一个元素。
  2. **迭代器遍历完毕，指针不会复位**：迭代器在遍历集合时会维护一个内部的指针指向当前位置，当遍历完成后，这个指针会停留在最后一个元素的位置上，并不会自动复位到集合的起始位置。如果需要重新遍历集合，需要重新获取迭代器对象。
  3. **循环中只能用一次 next 方法**：在同一个循环中，应该只调用一次 `next()` 方法来获取当前位置的元素。如果需要多次访问当前元素，应该将其存储在变量中以便重复使用，而不是反复调用 `next()` 方法。并且可能会造成第一个问题，两个`next()`，但第一个已经到达末尾。
  4. **迭代器遍历时，不能用集合的方法进行增加或者删除**：在使用迭代器遍历集合的过程中，不能直接使用集合的方法来增加或删除元素，否则会抛出 `ConcurrentModificationException` 异常。如果确实需要删除集合中的元素，应该使用迭代器提供的 `remove()` 方法来进行删除操作。至于添加元素，目前的 Java 标准库中迭代器并没有提供相应的方法，所以在遍历过程中添加元素是不被允许的。

[Java迭代器Iterator和Iterable有什么区别？](https://javabetter.cn/collection/iterator-iterable.html)

#### 增强for

> 增强for循环（也称为foreach循环）是Java语言的一种语法糖，它可以用于遍历数组和实现了 `Iterable` 接口的集合类。增强for循环在简化迭代器的代码书写方面发挥了重要作用，因为它隐藏了迭代器的细节，让代码更加简洁易读。

所有的单列集合，数组才能用增强for进行遍历。

格式：

```java
for(元素的数据类型 变量名 : 数组或集合)
```

```java
for (String s : list) {
    System.out.println(s);
}
```

注意：

修改增强for中的变量，不会改变集合中原本的数据

**Lambda表达式形式**

得益于JDK 8开始的新技术Lambda表达式，提供了一种更简单、更直接的遍历集合的方式。

| 方法名称                                           | 说明                       |
| -------------------------------------------------- | -------------------------- |
| `default void forEach(Consumer<? super T> action)` | 结合 Lambda 表达式遍历集合 |

匿名内部类方式：

```java
 list.forEach(new Consumer<String>() {
            @Override
            public void accept(String s) {
               // System.out.println(s); s表示集合中每一个元素
            }
        });
```
Lambda表达式方式：

```java
list.forEach(s -> System.out.println(s));
```

## List

### 常用方法

List集合的特有方法

* Collection的方法List都继承了

* List集合有索引，所以多了很多索引操作的方法

| 方法名称                         | 说明                                     |
| -------------------------------- | ---------------------------------------- |
| `void add(int index, E element)` | 在此集合中的指定位置插入指定的元素。     |
| `E remove(int index)`            | 删除指定索引处的元素，返回被删除的元素。 |
| `E set(int index, E element)`    | 修改指定索引处的元素，返回被修改的元素。 |
| `E get(int index)`               | 返回指定索引处的元素                     |

针对 List 系列集合中的两个删除方法，分别是：

1. 直接删除元素：使用 `remove(Object o)` 方法来删除指定的元素。
2. 通过索引进行删除：使用 `remove(int index)` 方法来删除指定索引处的元素。

如果在调用这两个方法时出现了重载现象，即有多个方法的参数类型与实参类型一致，那么优先调用参数类型更加具体的方法。

关于手动装箱，将基本数据类型转换为对应的包装类，可以使用自动装箱和拆箱的特性来简化代码。例如，将整数 1 转换为 Integer 对象，可以直接写成 `Integer.valueOf(1)` 或者直接写成 `1`，Java 编译器会自动将其转换为 `Integer` 类型。

```java
List<Integer> list = new ArrayList<>();
list.add(1);
list.remove(Integer.valueOf(1)); // 通过手动装箱删除元素 remove(Object o)
```

列表迭代器：在遍历的过程中需要添加元素，使用列表迭代器

List独有的。

### ArrayList

`ArrayList` 实现了 `List` 接口，因此它可以用于任何需要 `List` 接口的地方，可以作为 `List` 类型的对象来使用。

**ArrayList集合底层原理**：

1. 利用空参创建的集合，在底层创建一个默认长度为0的数组
2. 添加第一个元素时，底层会创建一个新的长度为10的数组
3. 存满时，会扩容1.5倍
4. 如果一次添加多个元素，1.5倍还放不下，则新创建数组的长度以实际为准

* 第一次添加一个元素的情况

![ArrayList1](https://raw.githubusercontent.com/betteryuxuan/Image/main/ArrayList1.png)

* 添加了10个后添加第11个元素需要扩容情况

![ArrayList2](https://raw.githubusercontent.com/betteryuxuan/Image/main/ArrayList2.png)

### LinkedList

底层数据结构是双链表，查询慢，增删快，但是如果操作的是首尾元素，速度也是极快的。
LinkedList本身多了很多直接操作首尾元素的特有API。

|            方法签名             |               说明               |
| :-----------------------------: | :------------------------------: |
| **`public void addFirst(E e)`** |    在该列表开头插入指定的元素    |
| **`public void addLast(E e)`**  |  将指定的元素追加到此列表的末尾  |
|    **`public E getFirst()`**    |     返回此列表中的第一个元素     |
|    **`public E getLast()`**     |    返回此列表中的最后一个元素    |
|  **`public E removeFirst()`**   |  从此列表中删除并返回第一个元素  |
|   **`public E removeLast()`**   | 从此列表中删除并返回最后一个元素 |

LinkedList底层原理

![LinkedList](https://raw.githubusercontent.com/betteryuxuan/Image/main/LinkedList.png)

迭代器底层

![diedaiqi1](https://raw.githubusercontent.com/betteryuxuan/Image/main/diedaiqi1.png)

在以后如何避免并发修改异常：
在使用迭代器或者是增强for遍历集合的过程中，不要使用集合的方法去添加或者删除元素即可。

## Set

Set接口中的方法上基本上与Collection的API一致

|               方法名称                |               说明               |
| :-----------------------------------: | :------------------------------: |
|       `public boolean add(E e)`       |   把给定的对象添加到当前集合中   |
|         `public void clear()`         |       清空集合中所有的元素       |
|     `public boolean remove(E e)`      | 把**给定的对象**在当前集合中删除 |
| `public boolean contains(Object obj)` | 判断当前集合中是否包含给定的对象 |
|      `public boolean isEmpty()`       |       判断当前集合是否为空       |
|          `public int size()`          | 返回集合中元素的个数/集合的长度  |

**无序**：存取顺序不一致
**不重复**：可以去除重复
**无索引**：没有带索引的方法，所以不能使用普通for循环遍历，也不能通过索引来获取元素



Hashset：<u>无序</u>、不重复、无索引
LinkedHashset：<u>存取有序</u>、不重复、无索引
Treeset：<u>可排序</u>、不重复、无索引



### HashSet

**Hashset底层原理**：

* Hashset集合底层采取哈希表存储数据

* 哈希表 是一种对于增删改查数据性能都较好的结构

**哈希表组成**：

* JDK8之前：数组+链表
* JDK8开始：数组+链表+红黑树

**哈希值**：

* 根据hashcode方法算出来的int类型的整数

* 该方法定义在Object类中，所有对象都可以调用，默认使用地址值进行计算

* 一般情况下，如果集合中存储的是自定义对象，必须要重写hashcode方法，利用对象内部的属性值计算哈希值

**对象的哈希值特点**：

* 如果没有重写hashcode方法，不同对象计算出的哈希值是不同的
* 如果已经重写hashcode方法，不同的对象只要属性值相同，计算出的哈希值就是一样的
*  在小部分情况下，不同的属性值或者不同的地址值计算出来的哈希值也有可能一样（哈希碰撞）

**底层原理**：

1. 创建一个默认长度16，默认加载因子0.75的数组，数组名table

2. 根据元素的哈希值跟数组的长度计算出应存入的位置

3. 判断当前位置是否为null，如果是null 直接存入

4. 如果位置不为null，表示有元素，则调用equals方法比较属性值

5. **一样：不存    不一样：存入数组，形成链表**
   **JDK8以前：新元素存入数组，老元素挂在新元素下面**
   **JDK8以后：新元素直接挂在老元素下面**

   当链表长度大于8而且数组长度大于等于64，自动转为红黑树

数组+链表+红黑树：![hashset](https://raw.githubusercontent.com/betteryuxuan/Image/main/hashset.png)

**Hashset为什么存和取的顺序不一样?**

HashSet 内部使用哈希表来存储元素，哈希表的内部实现不保证元素的存储顺序与插入顺序一致。元素被添加到 HashSet 中时，它们首先被哈希化，然后根据哈希值放置在内部数组的不同位置。由于哈希表的存储机制，元素的存储顺序取决于它们的哈希值及哈希函数的映射结果，而不是它们被插入的顺序。

**Hashset为什么没有索引?**

同一个索引可能会存储多个元素，为了解决冲突，HashSet 使用了一种称为链表或更高效的方法（Java 8 中的红黑树）来存储具有相同哈希码的元素，使得查找操作的时间复杂度保持在接近常量的水平。

**HashSet是利用什么机制保证数据去重的?**

`HashCode`方法`equals`方法

* 存储`String`或`Integer`时，不需要重写`HashCode`方法`equals`方法，因为这些类已经提供了适当的 `hashCode` 和 `equals` 实现。

  1. **`String` 类**：
     - `String` 类在 Java 中已经重写了 `hashCode` 和 `equals` 方法。
     - `hashCode` 方法基于字符串的字符内容计算哈希码，使得相同的字符串具有相同的哈希码。
     - `equals` 方法用于比较字符串的内容是否相等。
  2. **`Integer` 类**：
     - `Integer` 类同样重写了 `hashCode` 和 `equals` 方法。
     - `hashCode` 方法返回整数值本身作为哈希码，因为整数值本身可以唯一标识一个 `Integer` 对象。
     - `equals` 方法比较两个 `Integer` 对象的数值是否相等。

#### LinkedHashSet

**有序、不重复、无索引**

有序指的是保证存储和取出的元素顺序一致

**原理**：底层数据结构是依然哈希表，只是每个元素又额外的多了一个双向链表的机制记录存储的顺序。

> 如果要数据去重，默认使用HashSet
> 如果要求去重且存取有序，才使用LinkedHashSet

### TreeSet

**不重复、无索引、可排序**

可排序：按照元素的默认规则(有小到大)排序

Treeset集合底层是基于**红黑树**的数据结构实现排序的，增删改查性能都较好。

**Treeset集合默认的规则**：

对于数值类型：Integer，Double，默认按照从小到大的顺序进行排序。

对于字符、字符串类型：按照字符在ASCII码表中的数字升序进行排序

#### 两种比较方式

> 使用原则：默认使用第一种，如果第一种不能满足当前需求，就使用第二种
>
> 同时存在使用第二种

**方式一**

**默认排序/自然排序**：Javabean类实现Comparable接口指定比较规则

红黑树在添加元素时，使用 `compareTo` 方法来确定元素的位置

**返回值**：

负数：认为要添加的元素是小的，存左边

正数：认为要添加的元素是大的，存右边

0：认为要添加的元素已经存在，舍弃



this：表示当前要添加的元素

o：表示已经在红黑树存在的元素

```java
import java.util.Objects;
public class Student implements Comparable<Student> {
    private String name;
    private int age;

    public Student() {
    }

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public int compareTo(Student o) {
        return this.age - o.age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String toString() {
        return name + " " + age;
    }
}
```

**方式二**

**比较器排序**：创建Treeset对象时候，传递比较器`Comparator`指定规则

<small>按长度排序的字符串列表，如果长度相同，则按字典顺序排列：</small>

```java
TreeSet<String> list = new TreeSet<>(new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        int i = o1.length() - o2.length();
        return i == 0 ? o1.compareTo(o2) : i;
    }
});
```

```java
     public static void main(String[] args) {
        TreeSet<String> list = new TreeSet<>((o1, o2) -> {
            int i = o1.length() - o2.length();
            return i == 0 ? o1.compareTo(o2) : i;
    });
```

## 使用场景

1. 如果想要集合中的元素可重复
   用`ArrayList`集合，基于数组的  (用的最多)
2. 如果想要集合中的元素可重复，而且当前的增删操作明显多于查询
   用`LinkedList`集合，基于链表的
3. 如果想对集合中的元素去重
   用`Hashset`集合，基于哈希表   (用的最多)
4. 如果想对集合中的元素去重，而且保证存取顺序
   用`LinkedHashset`集合，基于哈希表和双链表，效率低于Hashse
5. 如果想对集合中的元素进行排序
   用`Treeset`集合，基于红黑树。后续也可以用List集合实现排序

## Collections工具类

`java.util.Collections`：是集合工具类

作用：`collections`不是集合，而是集合的工具类。

主要用于处理 `List` 和 `Set` 集合

| **方法名称**                                                 |           **说明**           |
| :----------------------------------------------------------- | :--------------------------: |
| **`public static <T> boolean addAll(Collection<T> c, T... elements)`** |       **批量添加元素**       |
| **`public static void shuffle(List<?> list)`**               |  **打乱List集合元素的顺序**  |
| **`public static <T> void sort(List<T> list)`**              |           **排序**           |
| **`public static <T> void sort(List<T> list, Comparator<? super T> c)`** |  **根据指定的规则进行排序**  |
| **`public static <T> int binarySearch(List<? extends Comparable<? super T>> list, T key)`** |   **以二分查找法查找元素**   |
| **`public static <T> void copy(List<? super T> dest, List<? extends T> src)`** |     **拷贝集合中的元素**     |
| **`public static <T> void fill(List<? super T> list, T obj)`** |  **使用指定的元素填充集合**  |
| **`public static <T> T max(Collection<? extends T> coll)`**  |        **获取最大值**        |
| **`public static <T> T min(Collection<? extends T> coll)`**  |        **获取最小值**        |
| **`public static void swap(List<?> list, int i, int j)`**    | **交换集合中指定位置的元素** |

## Queue

Queue，也就是队列，通常遵循先进先出（FIFO）的原则，新元素插入到队列的尾部，访问元素返回队列的头部。

### ArrayDeque

从名字上可以看得出，ArrayDeque 是一个基于数组实现的双端队列，为了满足可以同时在数组两端插入或删除元素的需求，数组必须是循环的，也就是说数组的任何一点都可以被看作是起点或者终点。

### LinkedList

LinkedList 一般应该归在 List 下，只不过，它也实现了 Deque 接口，可以作为队列来使用。等于说，LinkedList 同时实现了 Stack、Queue、PriorityQueue 的所有功能。

```java
public class LinkedList<E>
    extends AbstractSequentialList<E>
    implements List<E>, Deque<E>, Cloneable, java.io.Serializable
{}
```

换句话说，LinkedList 和 ArrayDeque 都是 Java 集合框架中的双向队列（deque），它们都支持在队列的两端进行元素的插入和删除操作。不过，LinkedList 和 ArrayDeque 在实现上有一些不同：

- 底层实现方式不同：LinkedList 是基于链表实现的，而 ArrayDeque 是基于数组实现的。
- 随机访问的效率不同：由于底层实现方式的不同，LinkedList 对于随机访问的效率较低，时间复杂度为 O(n)，而 ArrayDeque 可以通过下标随机访问元素，时间复杂度为 O(1)。
- 迭代器的效率不同：LinkedList 对于迭代器的效率比较低，因为需要通过链表进行遍历，时间复杂度为 O(n)，而 ArrayDeque 的迭代器效率比较高，因为可以直接访问数组中的元素，时间复杂度为 O(1)。
- 内存占用不同：由于 LinkedList 是基于链表实现的，它在存储元素时需要额外的空间来存储链表节点，因此内存占用相对较高，而 ArrayDeque 是基于数组实现的，内存占用相对较低。

因此，在选择使用 LinkedList 还是 ArrayDeque 时，需要根据具体的业务场景和需求来选择。如果需要在双向队列的两端进行频繁的插入和删除操作，并且需要随机访问元素，可以考虑使用 ArrayDeque；如果需要在队列中间进行频繁的插入和删除操作，可以考虑使用 LinkedList。

在使用 LinkedList 作为队列时，可以使用 offer() 方法将元素添加到队列的末尾，使用 poll() 方法从队列的头部删除元素。另外，由于 LinkedList 是链表结构，不支持随机访问元素，因此不能使用下标访问元素，需要使用迭代器或者 poll() 方法依次遍历元素

### PriorityQueue

PriorityQueue 是一种优先级队列，它的出队顺序与元素的优先级有关，执行 remove 或者 poll 方法，返回的总是优先级最高的元素。

# 双列集合

双列集合的特点

1. 双列集合一次需要存一对数据，分别为键和值
2. 键不能重复，值可以重复
3.  键和值是一一对应的，每一个键只能找到自己对应的值
4. 键+值这个整体，我们称之为“键值对”或者'键值对对象’，在Java中叫做“Entry对象"

## Map

### Map常见API
Map是双列集合的顶层接口，它的功能是全部双列集合都可以继承使用的

| 方法签名                              | 说明                                 |
| ------------------------------------- | ------------------------------------ |
| `V put(K key, V value)`               | 添加元素                             |
| `V remove(Object key)`                | 根据键删除键值对元素                 |
| `void clear()`                        | 移除所有的键值对元素                 |
| `boolean containsKey(Object key)`     | 判断集合是否包含指定的键             |
| `boolean containsValue(Object value)` | 判断集合是否包含指定的值             |
| `boolean isEmpty()`                   | 判断集合是否为空                     |
| `int size()`                          | 集合的长度，也就是集合中键值对的个数 |

1. put

   在添加数据的时候，如果键不存在，那么直接把键值对对象添加到map集合当中,方法返回null
   在添加数据的时候，如果键是存在的，那么会把原有的键值对对象覆盖，会把被覆盖的值进行返回。

   ```java
   public class test {
       public static void main(String[] args) {
           Map <String,Integer> m = new HashMap<>();
           m.put("a",1);
           m.put("b",2);
           m.put("c",3);
           int ret = m.put("c",4);
   
           System.out.println(ret); // 3
           System.out.println(m);
       }
   }
   ```

### 三种遍历方式

* 键找值

  ```java
  public class test {
      public static void main(String[] args) {
          Map<String, Integer> map = new HashMap<>();
          map.put("a", 1);
          map.put("b", 2);
          map.put("c", 3);
  
          Set<String> keys = map.keySet();
          for (String s : keys) {
              String value = hashMap.get(key);
              System.out.println(s + "=" + value);
          }
      }
  }
  ```

  ```java
  HashMap<String, String> hashMap = new HashMap<>();
  hashMap.put("沉默", "cenzhong");
  hashMap.put("王二", "wanger");
  hashMap.put("陈清扬", "chenqingyang");
  
  for (String key : hashMap.keySet()) {
      System.out.println(key + " 对应值为：" + map.get(s));
  }
  ```

* 键值对

  导包`import java.util.Map.Entry;`或`Map.Entry<String, Integer> entries`

  ```java
  import java.util.Map.Entry;
  
  Set<Entry<String, Integer>> entries = map.entrySet();
  for (Entry<String, Integer> entry : entries) {
      System.out.println(entry);
      // System.out.println(entry.getKey() + "=" + entry.getValue());
  }
  ```

* Lambda表达式

  ```java
          map.forEach(new BiConsumer<String, Integer>() {
              @Override
              public void accept(String s, Integer integer) {
                  System.out.println(s + "=" + integer);
              }
          });
  ```

   ```java
        map.forEach((s, integer) -> System.out.println(s + "=" + integer));
   ```

## HashMap

**特点**：

* HashMap是Map里面的一个实现类，
* 没有额外特有方法，直接使用Map里面的方法就可以了
* 特点都是由键决定的：无序（存储顺序与插入顺序不一致）、不重复、无索引
* HashMap跟HashSet底层原理是一模一样的，都是哈希表结构（数组+链表+红黑树）

依赖hashcode方法和equals方法保证键的唯一：
**如果<u>键</u>存储的是自定义对象，需要重写hashcode和equals方法**
如果<u>值</u>存储自定义对象，不需要重写hashcode和equals方法

### LinkedHashMap

* 由键决定：有序（存储顺序与插入顺序一致）、 不重复、无索引。
* 原理：底层数据结构是依然哈希表，只是每个键值对元素又额外的多了一个双链表的机制记录存储的顺序

## TreeMap

* TreeMap底层原理是红黑树结构的

* 由键决定特性：不重复、无索引、可排序

* 可排序：对**<u>键</u>**进行排序，

* 注意：默认按照键的从小到大进行排序，也可以自己规定键的排序规则


**代码书写两种排序规则**：

* 实现Comparable接口，指定比较规则。
* 创建集合时传递Comparator比较器对象，指定比较规则。

```java
import java.util.HashMap;
import java.util.Map;
import java.util.StringJoiner;
import java.util.function.BiConsumer;

public class Test {
    public static void main(String[] args) {
        String s = "aababcabcdabcde";
        Map<Character, Integer> map = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (map.containsKey(ch)) {
                int count = map.get(ch);
                count++;
                map.put(ch, count);
            } else {
                map.put(ch, 1);
            }
        }
        StringJoiner sb =new StringJoiner("","","");

        map.forEach(new BiConsumer<Character, Integer>() {
            @Override
            public void accept(Character character, Integer integer) {
               sb.add(character+"").add("(").add(integer+"").add(")");
            }
        });
        System.out.println(sb);
    }
}
```



## 使用

**默认使用**：HashMap
**存取有序**：LinkedmashMap
**结果排序**：TreeMap

键：表示要统计的内容
值：表示次数

# 可变参数

> 可变参数（Varargs）允许一个方法接受多个参数而不必定义多个方法重载。这使得编写更加灵活和简洁的代码成为可能。可变参数通过在参数类型后面加上省略号 (`...`) 来实现。

本质上就是一个数组
作用：在形参中接收多个数据
格式：数据类型...参数名称

1. 在方法的形参中最多只能写一个可变参数
2. 在方法当中，如果出了可变参数以外，还有其他的形参，可变参数要写在最后

```java
public void methodName(int fixedParam, String... varArgs) {
    // 方法体
}
```

# 不可变集合

* List、set、Map接口中，都存在`of`方法可以创建不可变集合
* 三种方式的细节
  List：直接用
  Set：元素不能重复
  Map：元素不能重复、键值对数量最多是10个，超过10个用ofEntries方法

**创建不可变集合**：

| **方法名称**                                                 | **说明**                                 |
| ------------------------------------------------------------ | ---------------------------------------- |
| **`static <E> List<E> of(E... elements)`**                   | **创建一个具有指定元素的 List 集合对象** |
| **`static <E> Set<E> of(E... elements)`**                    | **创建一个具有指定元素的 Set 集合对象**  |
| **`static <K,V> Map<K,V> of(K k1, V v1, K k2, V v2, K... kv)`** | **创建一个具有指定元素的 Map 集合对象**  |

注意：这个集合不能添加，不能删除，不能修改。

```java
//获取到所有的键值对对象(Entry对象)
Set<Map.Entry<String, String>>entries = hm.entryset();
//把entries变成一个数组
Map.Entry[] arr1 = new Map.Entry[0];
//toArray方法在底层会比较集合的长度跟数组的长度两者的大小
//如果集合的长度 > 数组的长度 :数据在数组中放不下，此时会根据实际数据的个数，重新创建数组
//如果集合的长度<= 数组的长度:数据在数组中放的下，此时不会创建新的数组，而是直接用
Map.Entry[l arr2 = entries.toArray(arr1);
//不可变的map集合
Map map = Map.ofEntries(arr2);
```

```java
Map<Object, Object> map = Map.ofEntries(hm.entrySet().toArray(new Map.Entry[0]));
```

```java
Map<String,String>map = Map.copyof(hm);
```

**长度的获取**：

- `length`（不带括号）：用于数组。
- `length()`（带括号）：用于字符串。
- `size()`（带括号）：用于集合框架中的List、Set、Map等。

# Stream流

Stream流的使用步骤：

1. 先得到一条Stream流(流水线)，并把数据放上去
2. 使用中间方法对流水线上的数据进行操作
3. 使用终结方法对流水线上的数据进行操作

## 获取Stream流对象

|   获取方式   |                     方法名                      |           说明           |
| :----------: | :---------------------------------------------: | :----------------------: |
|   单列集合   |          `default Stream<E> stream()`           |  Collection中的默认方法  |
|   双列集合   |                       无                        |   无法直接使用stream流   |
|     数组     | `public static <T> Stream<T> stream(T[] array)` | Arrays工具类中的静态方法 |
| 一堆零散数据 |  `public static <T> Stream<T> of(T... values)`  |  Stream接口中的静态方法  |


单列集合
```java
        Stream<String> stream = list.stream();
        stream.forEach(s -> System.out.println(s));
```
双列集合
```java
        Set<String> keySet = map.keySet();
        Stream<String> stream2 = keySet.stream();
        stream2.forEach(s -> System.out.println(s));
```

```java
        Set<Map.Entry<String, Integer>> entries = map.entrySet();
        Stream<Map.Entry<String, Integer>> stream3 = entries.stream();
        stream3.forEach(s-> System.out.println(s));
```
数组
```java
        Arrays.stream(arr).forEach(s -> System.out.println(s));
```

stream接口中静态方法of的细节
`Stream.of(T... values)` 方法接受一个可变参数列表 `values`，可以传递多个零散的引用类型元素或一个引用类型数组。

- 如果传递的是引用数据类型数组，数组中的每个元素都会成为流中的一个元素。
- 如果传递的是基本数据类型数组，整个数组会被视为流中的一个单一元素。

## 中间方法

|                          名称                          |                  说明                  |
| :----------------------------------------------------: | :------------------------------------: |
| **`Stream<T> filter(Predicate<? super T> predicate)`** |                  过滤                  |
|          **`Stream<T> limit(long maxSize)`**           |             获取前几个元素             |
|              **`Stream<T> skip(long n)`**              |             跳过前几个元素             |
|               **`Stream<T> distinct()`**               | 元素去重，依赖`hashCode`和`equals`方法 |
| **`static <T> Stream<T> concat(Stream a, Stream b)`**  |         合并a和b两个流为一个流         |
|       **`Stream<R> map(Function<T, R> mapper)`**       |           转换流中的数据类型           |

**注意**：

1. 中间方法，返回新的Stream流，原来的Stream流只能使用一次，建议使用链式编程
2. 修改Stream流中的数据，不会影响原来集合或者数组中的数据

## 终结方法

|                名称                 |            说明            |
| :---------------------------------: | :------------------------: |
| **`void forEach(Consumer action)`** |            遍历            |
|         **`long count()`**          |            统计            |
|           **`toArray()`**           | 收集流中的数据，放到数组中 |
| **`collect(Collector collector)`**  | 收集流中的数据，放到集合中 |

1. **`toArray()`**

   1. 直接使用`toArray()`方法：

   ```java
   Object[] arr1 = list.stream().toArray();
   ```

   将流中的元素按照其出现顺序，收集到一个`Object`类型的数组中。

   2. 使用`toArray(IntFunction)`方法：

   ```java
   String[] arr2 = list.stream().toArray(new IntFunction<String[]>() {
       @Override
       public String[] apply(int value) {
           return new String[value];
       }
   });
   ```

   `apply`的形参：流中数据的个数，要跟数组的长度保持一致

   `apply`的返回值：具体类型的数组

   允许指定数组的长度，并根据需要为其分配内存。

   它的作用是根据流中元素的数量创建一个指定类型的数组，然后将流中的元素按照其出现顺序放入数组中。

   在这个例子中，我们实现了`IntFunction`接口，并覆盖了其中的`apply()`方法。这个方法接受一个整数参数，表示数组的长度。在`apply()`方法中，我们根据这个长度创建了一个`String`类型的数组，然后返回它。这个数组最终用于收集流中的元素

2. `collect`

   ```java
   public class Test {
       public static void main(String[] args) {
   	    ArrayList<String> list2 = new ArrayList<>();
           Collections.addAll(list2, "张无忌-男-15", "周芷若-女-14", "赵敏-女-13", "张强-男-20", "张三丰_男-100", "张翠山-男-40", "张良-男-35", "王二麻子-男-37", "谢广坤-男-41");
           List<String> collected = list2.stream().filter(new Predicate<String>() {
               @Override
               public boolean test(String s) {
                   return "男".equals(s.split("-")[1]);
               }
           }).collect(Collectors.toList());
           collected.forEach(s -> System.out.println(s));
   
       }
   }
   ```

   ```java
   // 使用Stream API对字符串列表进行处理，生成一个Map对象
   Map<String, Integer> map2 = list.stream()
           
           // 过滤出以"男"结尾的字符串
           .filter(s -> "男".equals(s.split("-")[1]))
           
           // 将符合条件的字符串按照"-"分割，并以分割后的第一个部分作为键，第三个部分作为值
           .collect(Collectors.toMap(
                   s -> s.split("-")[0],  // 以"-"分割后的第一个部分作为键
                   s -> Integer.parseInt(s.split("-")[2]) // 将以"-"分割后的第三个部分转换为整数作为值
           ));
   ```

# 方法引用

把已经有的方法拿过来用，当做函数式接口中抽象方法的方法体

`::`方法引用符

1. 被引用的方法必须已经存在
2. 被引用方法的形参和返回值需要跟抽象方法保持一致
3. 被引用方法的功能要满足当前需求 
4. 被引用方法的功能需要满是当前的要求

## 引用静态方法 

格式：`类名::静态方法`
范例：`Integer::parseInt`

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.function.Function;

public class Test {
    public static void main(String[] args) {
        ArrayList<String> list1 = new ArrayList<>();
        Collections.addAll(list1, "1", "2", "3", "4");
        //使用匿名内部类来实现Function接口，将String转换为Integer
//        list1.stream().map(new Function<String, Integer>() {
//            @Override
//            public Integer apply(String s) {
//                int i = Integer.parseInt(s);
//                return i;
//            }
//        }).forEach(s-> System.out.println(s));

        
        // 使用流的map方法，将list1中的每个String元素转换为Integer
        // Integer::parseInt是一个方法引用，等同于上面注释掉的代码段
        list1.stream()
            .map(Integer::parseInt) // 将每个String元素转换为Integer
            .forEach(s -> System.out.println(s));
    }
}
```

## 引用成员方法

> 1. 其他类的成员方法
>
> 2. 本类的成员方法
>
> 3. 父类的成员方法

格式：`对象::成员方法`

其他类：`其他类对象::方法名`
本类：`this::方法名`
父类：`super::方法名`

在静态方法中无法使用 `this` 关键字，因此只能创建类的对象引用方法。

## 引用构造方法

格式：`类名::new`
范例：`Student::new`

## 其他调用方式

### 使用类名引用成员方法

格式：`类名::成员方法`
范例：`String::substring`

**第一个参数**：表示被引用方法的调用者，决定了可以引用哪些类中的方法，在stream流当中，第一个参数一般都表示流里面的每一个数据。假设流里面的数据是字符串，那么使用这种方式进行方法引用，只能引用string这个类中的方法
**第二个参数到最后一个参数**：跟被引用方法的形参保持一致，如果没有第二个参数，说明被引用的方法需要是无参的成员方法

### 引用数组的构造方法

格式：`数据类型[]::new`
范例：`int[]::new`

技巧：

1. 现在有没有一个方法符合我当前的需求
2. 如果有这样的方法，这个方法是否满足引用的规则
3. 静态： `类名::方法名`
   成员方法： 三种
   构造方法： `类名::new`

# 异常

对于异常情况，Java使用了一种称为异常处理(exception handing)的错误捕获机制。

![exception](https://raw.githubusercontent.com/betteryuxuan/Image/main/exception.png)

Error：代表的系统级别错误（属于严重问题）
系统一旦出现问题，sun公司会把这些错误封装成Error对象:
Error是给sun公司自己用的，不是给我们程序员用的。
因此我们开发人员不用管它，

* Exception：叫做异常，代表程序可能出现的问题

  我们通常会用Exception以及他的子类来封装程序出现的问题

  RuntimeException及其子类，编译阶段不会出现异常提醒。

* **编译时异常**：
  没有继承RuntimeExcpetion的异常，直接继承于Excpetion。编译阶段就会出现异常提醒的。(如:日期解析异常)

* **运行时异常**：

  RuntimeException本身和子类。
  编译阶段没有错误提示，运行时出现的异常(如:数组索引越界异常)

> Error 类： ⼀般是指与虚拟机相关的问题，如：系统崩溃、虚拟机错误、内存空间不⾜、⽅法调⽤栈溢出等。这类
>
> 错误将会导致应⽤程序中断，仅靠程序本身⽆法恢复和预防；
>
> Exception 类：分为运⾏时异常和受检查的异常
>
> 运⾏时异常：如：空指针异常、指定的类找不到、数组越界、⽅法传递参数错误、数据类型转换错误。可以编译通过，但是⼀运⾏就停⽌了，程序不会⾃⼰处理；
>
> 受检查异常：要么⽤ try … catch… 捕获，要么⽤ throws 声明抛出，交给⽗类处理。

| 特征     | 编译时异常                     | 运行时异常                                              |
| :------- | :----------------------------- | :------------------------------------------------------ |
| 检测时机 | 在编译阶段检测到               | 在运行时检测到                                          |
| 处理方式 | 必须显式处理或声明抛出         | 通常不需要显式处理或声明抛出                            |
| 继承关系 | 继承自 Exception               | 继承自 RuntimeException                                 |
| 典型示例 | IOException、ParseException 等 | NullPointerException、ArrayIndexOutOfBoundsException 等 |

## 作用

1. 异常是用来查询bug的关键参考信息
2. 异常可以作为方法内部的一种特殊返回值，以便通知调用者底层的执行情况

## 处理方式

1. JVM默认的处理方式
2. 自己处理
3. 抛出异常

格式：

```java
try {
    可能出现异常的代码;
} catch(异常类名 变量名) {
    异常的处理代码;
}
```

```java
public class test02 {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5, 6};
        try {
            System.out.println(arr[10]);
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("数组越界");
        }
        System.out.println("这里是执行的");
    }
}
```

目的：当代码出现异常时，可以让程序继续往下执行。

* **如果try中没有遇到问题，怎么执行?**

  catch 块将不会被执行。

* **如果try中可能会遇到多个问题，怎么执行?**

  添加多个 catch 块来捕获不同类型的异常。

  父类异常写在下面

* **如果try中遇到的问题没有被捕获，怎么执行?**

  异常将会被抛到 JVM，导致程序终止执行，并输出异常信息。

* **如果try中遇到了问题，那么try下面的其他代码还会执行吗?**

  try 块中异常发生点后面的代码将不会执行。程序会立即跳转到与异常匹配的 catch 块中执行相应的异常处理逻

## 常见成员方法

|            方法名称             |               说明                |
| :-----------------------------: | :-------------------------------: |
|  `public String getMessage()`   | 返回此 Throwable 的详细消息字符串 |
|   `public String toString()`    |    返回此可抛出对象的简短描述     |
| `public void printStackTrace()` |   将异常的错误信息输出到控制台    |

`printStackTrace`仅仅打印信息，不会结束程序

## 抛出异常

**throws**：
写在方法定义处，表示声明一个异常，告诉调用者，使用本方法可能会有哪些异常

```java
public void 方法()throws 异常类名1,异常类名2...{
}
```

**throw**：
写在方法内，结束方法，手动抛出异常对象，交给调用者，方法中下面的代码不再执行了

```java
public void 方法() {
throw new NullPointerException();
}
```

编译时异常：必须要写
运行时异常：可以不写。

## finally

finally关键字用来创建在 try 代码块后面执行的代码块。

无论是否发生异常，finally 代码块中的代码总会被执行。

在 finally 代码块中，可以运行清理类型等收尾善后性质的语句。

finally 代码块出现在 catch 代码块最后，语法如下：

```java
try{
  // 程序代码
}catch(异常类型1 异常的变量名1){
  // 程序代码
}catch(异常类型2 异常的变量名2){
  // 程序代码
}finally{
  // 程序代码
}
```

**问题一，finally是不是⼀定会被执行到？**

> 不⼀定
>
> 1. 在执行 finally 块之前 JVM 强制退出
> 2. 执行线程被杀死
> 3. 无限循环
> 4. 调用了System.exit()。

**问题二、try-catch-finally中，如果catch中return了，finally还会执⾏吗？**

> 如果在 catch 块中执行了 return 语句，finally 块仍然会在返回之前执行。
>
> 对于基本数据类型，因为返回的是数值本身而不是引用，finally 块中对返回值的修改不会影响到实际的返回值。
>
> 但是对于引用类型，finally 块中对引用对象的修改会影响到返回值。这是因为引用类型的返回值是对象的地址，而不是对象本身。因此，如果在 finally 块中修改了引用对象的状态，这些修改会在返回之后仍然生效。

**问题三、try-catch-finally 中那个部分可以省略？**

> catch 可以省略。try 只适合处理运⾏时异常，try+catch 适合处理运⾏时异常+普通异常。也就是说，如果你只⽤
> try 去处理普通异常却不加以 catch 处理，编译是通不过的，因为编译器硬性规定，普通异常如果选择捕获，则必
> 须⽤ catch 显示声明以便进⼀步处理。⽽运⾏时异常在编译时没有如此规定，所以 catch 可以省略，你加上 catch
> 编译器也觉得⽆可厚⾮

## 自定义异常

1. 定义异常类
2. 写继承关系
3. 空参构造
4. 带参构造

- 所有异常都必须是 Throwable 的子类。
- 如果希望写一个检查性异常类，则需要继承 Exception 类。
- 如果你想写一个运行时异常类，那么需要继承 RuntimeException 类。

```java
public class CustomException extends Exception {
    // 自定义异常类的代码
}
```

![exceptionex](https://raw.githubusercontent.com/betteryuxuan/Image/main/exceptionex.png)

意义：

> 自定义异常的意义在于提高程序的可读性和可维护性，使得控制台输出的错误信息更加具有意义和可理解性。通过自定义异常，为不同的错误场景定义具有描述性的异常类，使异常信息能够清晰地传达出问题的类型和原因，有助于开发人员快速定位和解决问题。

# File

## 路径

File对象就表示一个路径，可以是文件的路径、也可以是文件夹的路径
这个路径可以是存在的，也允许是不存在的



相对路径

> 相对路径是相对于当前工作目录或者某个基准目录的路径描述。换句话说，相对路径描述的是一个文件或目录相对于另一个文件或目录的位置关系。

绝对路径

> 绝对路径是从文件系统的根目录开始的完整路径描述。无论当前工作目录在哪里，绝对路径都能够准确地描述一个文件或目录的位置。

## 构造方法

| 方法名称                                   | 说明                                               |
| ------------------------------------------ | -------------------------------------------------- |
| `public File(String pathname)`             | 根据文件路径创建文件对象                           |
| `public File(String parent, String child)` | 根据父路径名字符串和子路径名字符串创建文件对象     |
| `public File(File parent, String child)`   | 根据父路径对应文件对象和子路径名字符串创建文件对象 |

```java
import java.io.File;

public class Test {
    public static void main(String[] args) {

        String str = "C:\\Users\\HP\\Desktop\\ggg.txt";
        File f1 = new File(str);
        System.out.println(f1);

        String parent = "C:\\Users\\HP\\Desktop\\";
        String child = "ggg.txt";
        File f2 = new File(parent, child);
        System.out.println(f2);

        File f3 = new File(parent);
        File f4 = new File(f3, child);
        System.out.println(f3);
        System.out.println(f4);

    }
}
```

## **判断、获取**

|               方法名称                |                 说明                 |
| :-----------------------------------: | :----------------------------------: |
|  **`public boolean isDirectory()`**   |  判断此路径名表示的File是否为文件夹  |
|     **`public boolean isFile()`**     |   判断此路径名表示的File是否为文件   |
|     **`public boolean exists()`**     |    判断此路径名表示的File是否存在    |
|      **`public long length()`**       |      返回文件的大小（字节数量）      |
| **`public String getAbsolutePath()`** |          返回文件的绝对路径          |
|     **`public String getPath()`**     |       返回定义文件时使用的路径       |
|     **`public String getName()`**     |        返回文件的名称，带后缀        |
|   **`public long lastModified()`**    | 返回文件的最后修改时间（时间毫秒值） |

1. `length`

   * 返回的是字节
   * 无法获取文件夹大小

2. `length`

   ```java
   import java.io.File;
   import java.text.SimpleDateFormat;
   import java.util.Date;
   
   public class Test {
       public static void main(String[] args) {
   
           String str = "C:\\Users\\HP\\Desktop\\ggg.txt";
           File f1 = new File(str);
           System.out.println(f1);
   
           long d = f1.lastModified();
           Date date = new Date(d);
           SimpleDateFormat sdf = new SimpleDateFormat("yyyy年MM月dd日 HH:mm:ss");
           System.out.println(sdf.format(date));
   
       }
   }
   ```


## **创建 、删除**

|               方法名称               |         说明         |
| :----------------------------------: | :------------------: |
| **`public boolean createNewFile()`** | 创建一个新的空的文件 |
|     **`public boolean mkdir()`**     |    创建单级文件夹    |
|    **`public boolean mkdirs()`**     |    创建多级文件夹    |
|    **`public boolean delete()`**     |  删除文件、空文件夹  |

1. `createNewFile`创建一个新的空的文件

   * 细节1：如果当前路径表示的文件是不存在的，则创建成功，方法返回true

     ​	     如果当前路径表示的文件是存在的，则创建失败，方法返回false

   * 细节2：如果父级路径是不存在的，那么方法会有异常`IOException`

   * 细节3：`createNewFile`方法创建的一定是文件，如果路径中不包含后缀名，则创建一个没有后缀的文件

2. `mkdir(make Directory)`

   * 细节1：windows当中路径是唯一的，如果当前路径已经存在，则创建失败，返回false
   * 细节2：`mkdir`方法只能创建单级文件夹，无法创建多级文件夹

3. `mkdirs(make Directory)`创建多级文件夹

   细节：既可以创建单级的，又可以创建多级的文件夹

4. delete    

   * 如果删除的是文件：直接删除，不走回收站
   * 如果删除的是文件夹：
     * 空文件夹，直接删除，不走回收站
     * 有内容的文件夹，删除失败

## **获取并遍历**

|            方法名称             |           说明           |
| :-----------------------------: | :----------------------: |
| **`public File[] listFiles()`** | 获取当前该路径下所有内容 |

* 当调用者File表示的路径不存在时，返回null
* 当调用者File表示的路径是文件时，返回null
* **当调用者File表示的路径是一个空文件夹时，返回一个长度为0的数组**
* **当调用者File表示的路径是一个有内容的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回**
* **当调用者File表示的路径是一个有隐藏文件的文件夹时，将里面所有文件和文件夹的路径放在File数组中返回，包含隐藏文件**
* 当调用者File表示的路径是需要权限才能访问的文件夹时，返回null

|                       方法名称                       |                     说明                     |
| :--------------------------------------------------: | :------------------------------------------: |
|        **`public static File[] listRoots()`**        | 列出可用的文件系统根（获取系统中所有的盘符） |
|             **`public String[] list()`**             | 获取当前该路径下所有内容（仅仅能获取名字 ）  |
|  **`public String[] list(FilenameFilter filter)`**   |   利用文件名过滤器获取当前该路径下所有内容   |
|           **`public File[] listFiles()`**            |           获取当前该路径下所有内容           |
|   **`public File[] listFiles(FileFilter filter)`**   |   利用文件名过滤器获取当前该路径下所有内容   |
| **`public File[] listFiles(FilenameFilter filter)`** |   利用文件名过滤器获取当前该路径下所有内容   |

```java
import java.io.File;
import java.io.FilenameFilter;
import java.util.Arrays;

public class Test {
    public static void main(String[] args) {
        File f2 = new File("p:\\aaa");

        String[] arr3 = f2.list(new FilenameFilter() {
            @Override
            public boolean accept(File dir, String name) {
                File src = new File(dir, name);
                return src.isFile() && name.endsWith(".txt");
            }
        });

        System.out.println(Arrays.toString(arr3));
    }
}
```

## 练习

> 找文件夹下某后缀文件，包括文件夹中的

递归

```java
public static void findmd() {
    // 获取所有根目录
    File arr[] = File.listRoots();
    // 遍历所有根目录
    for (File f : arr) {
        // 调用findmd(File src)方法，从根目录开始递归查找.md文件
        findmd(f);
    }
}

public static void findmd(File src) {
    // 获取目录下的所有文件和子目录
    File[] files = src.listFiles();
    // 如果目录不为空
    if (files != null) {
        // 遍历目录下的所有文件和子目录
        for (File file : files) {
            // 如果是文件
            if (file.isFile()) {
                // 检查文件是否以".md"结尾，如果是则打印文件名
                if (file.getName().endsWith(".md"))
                    System.out.println(file.getName());
            } else {
                // 如果是目录，递归调用findmd(File src)方法，继续查找该子目录下的.md文件
                findmd(file);
            }
        }
    }
}
```

> 计算当前目录大小

```java
public static long getCapticpy(File src) {
    // 获取目录下的所有文件和子目录
    File[] files = src.listFiles();
    // 初始化总字节数
    long sum = 0;
    // 如果目录不为空
    if (files != null) {
        // 遍历目录下的所有文件和子目录
        for (File f : files) {
            // 如果是文件，累加文件长度到总字节数中
            if (f.isFile()) {
                sum += f.length();
            } else {
                // 如果是目录，递归调用getCapticpy方法，将返回的字节数累加到总字节数中
                sum += getCapticpy(f);
            }
        }
    }
    // 返回总字节数
    return sum;
}
```



# IO流

存储和读取数据的解决方案

用于读写文件中的数据(可以读写文件，或网络中的数据)

**按流向分**：

* 输出流：程序
* 输入流：文件

**按操作文件的类型分**：

* 字节流：可以操作所有类型的文件
* 字符流：只能操作纯文本文件

纯文本文件：用windows系统自带的记事本打开并且能读懂的文件。
例如：txt文件，md文件，xml文件，lrc文件等

## 体系

![Buffered](https://raw.githubusercontent.com/betteryuxuan/Image/main/Buffered.png)

## 字节流

### FileOutputStream

操作本地文件的字节输出流，可以把程序中的数据写到本地文件中。

**一、构造方法**

| **构造方法**                                        |                           **描述**                           |
| :-------------------------------------------------- | :----------------------------------------------------------: |
| **`FileOutputStream(String name)`**                 | **使用指定的文件名创建一个 `FileOutputStream` 对象。如果文件不存在，会创建一个新文件。** |
| **`FileOutputStream(String name, boolean append)`** | **使用指定的文件名和 `append` 标志创建 `FileOutputStream` 对象。如果 `append` 为 `true`，字节将被写入文件末尾。** |
| **`FileOutputStream(File file)`**                   | **使用指定的 `File` 对象创建 `FileOutputStream` 对象。如果文件不存在，会创建一个新文件。** |
| **`FileOutputStream(File file, boolean append)`**   | **使用指定的 `File` 对象和 `append` 标志创建 `FileOutputStream` 对象。如果 `append` 为 `true`，字节将被写入文件末尾。** |
| **`FileOutputStream(FileDescriptor fdObj)`**        |        **创建一个文件输出流以写入指定的文件描述符。**        |

**二、书写步骤**

1. 创建字节输出流对象
2. 写数据
3. 释放资源

**细节**：

1. 创建字节输出流对象
   细节1：参数是字符串表示的路径或者是File对象都是可以的
   细节2：如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的。
   细节3：如果文件已经存在，则会清空文件
2. 写数据
   细节：write方法的参数是整数，但是实际上写到本地文件中的是整数在ASCII上对应的字符
3. 释放资源
   细节：每次使用完流之后都要释放资源

```java
import java.io.FileOutputStream;
import java.io.IOException;

public class Test {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("..\\5.20\\a.txt");
        fos.write(97);
        fos.close();
    }
}
```

**三、write用法**

| 方法名称                                 | 说明                         |
| ---------------------------------------- | ---------------------------- |
| `void write(int b)`                      | 一次写一个字节数据           |
| `void write(byte[] b)`                   | 一次写一个字节数组数据       |
| `void write(byte[] b, int off, int len)` | 一次写一个字节数组的部分数据 |

```java
public class Test {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("..\\5.20\\a.txt");
        String str = "abcd";
        byte[] bytes = str.getBytes();
        fos.write(bytes);
        fos.close();
    }
}
```

`void write(byte[] b, int off, int len)`：

`off`：起始索引：字节数组`b`中的起始偏移量。表示从数组的哪个索引位置开始写入数据。（`off`必须是非负整数，且不能超过数组`b`的长度。）

`len`：写入字节数：这是要写入的字节数。从`off`位置开始，连续写入`len`个字节。

**四、换行与续写**

**1.换行**
写出一个换行符即可
windows：`\r\n`
Linux：`\n`
Mac：`\r`

> 在windows操作系统当中，java对回车换行进行了优化。
> 虽然完整的是\r\n，但是我们写其中一个\r或者\n,
> java也可以实现换行，因为java在底层会补全。

**2.续写**

**`FileOutputStream(String name, boolean append)`** 

**`FileOutputStream(File file, boolean append)`**

**如果 `append` 为 `true`，字节将被写入文件末尾。**

```java
	FileOutputStream fos = new FileOutputStream("..\\5.20\\a.txt",true);
```

### FileInputStream

操作本地文件的字节输入流，可以把本地文件中的数据读取到程序中来。

**一、构造方法**

| **构造方法**                             | **参数描述**                           |
| ---------------------------------------- | -------------------------------------- |
| **`FileInputStream(File file)`**         | **要读取的文件对象。**                 |
| **`FileInputStream(String name)`**       | **要读取的文件的路径名。**             |
| **`FileInputStream(FileDescriptor fd)`** | **文件描述符对象，表示已打开的文件。** |

**二、书写步骤**

1. **创建字节输入流对象**
2. **读数据**
3. **释放资源**

**细节**：

1. * 如果文件不存在，就直接报错。
2. * 一次读一个字节，读出来的是数据在ASCII上对应的数字
   * 读到文件末尾了，read方法返回-1。
3. 每次使用完流必须要释放资源。

**三、循环读取**

```java
import java.io.FileInputStream;
import java.io.IOException;

public class Demo {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("..\\5.20\\a.txt");
        int b;
        while ((b = fis.read()) != -1) {
            System.out.print((char) b);
        }
        fis.close();
    }
}
```

**四、read**

| 方法名称                         | 说明                       |
| -------------------------------- | -------------------------- |
| `public int read()`              | 一次读取一个字节数据。     |
| `public int read(byte[] buffer)` | 一次读取一个字节数组数据。 |

一般创建1024的整数倍

**五、文件拷贝**

`public int read()`：

```java
public class Demo2 {
    public static void main(String[] args) throws IOException {
        // 创建文件输入流，用于读取原始文件
        FileInputStream fis = new FileInputStream("E:\\MyMarkdown\\链表排序.md");
        // 创建文件输出流，用于写入复制后的文件
        FileOutputStream fos = new FileOutputStream("E:\\MyMarkdown\\链表排序_copy.md");
        
        int b;
        // 循环读取原始文件的字节，写入到复制后的文件中
        while((b = fis.read()) != -1){
            fos.write(b);
        }
        
        // 关闭输入流和输出流
        fos.close();
        fis.close();
    }
}
```

`public int read(byte[] buffer)`：

```java
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Test02 {
    public static void main(String[] args) throws IOException {
        // 创建 FileInputStream 对象，用于读取源文件
        FileInputStream fis = new FileInputStream("..\\5.20\\a.txt");
        
        // 创建 FileOutputStream 对象，用于写入目标文件
        FileOutputStream fos = new FileOutputStream("..\\5.20\\acopy.txt");
        
        // 使用 try-with-resources 语句，确保在结束时自动关闭流
        try (fos; fis) {
            int len;
            byte[] bytes = new byte[1024];
            
            // 循环读取源文件，并将读取的数据写入目标文件
            while ((len = fis.read(bytes)) != -1) {
                fos.write(bytes, 0, len);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### try-catch处理

![trycatch](https://raw.githubusercontent.com/betteryuxuan/Image/main/trycatch.png)

`AutoCloseable` 接口的定义：

```java
package java.lang;

public interface AutoCloseable {
    void close() throws Exception;
}
```

`AutoCloseable` 接口只包含一个 `close()` 方法，该方法没有参数，并且可能会抛出 `Exception` 异常。类实现了 `AutoCloseable` 接口的意思就是它承诺可以在不再需要时释放资源，以确保资源的正常管理和释放。

使用 `AutoCloseable` 接口可以让资源管理更加简单和安全，在 `try-with-resources` 语句中使用，无需手动编写 `finally` 块来关闭资源。在 `try-with-resources` 语句中，当 `try` 块结束时，自动调用 `close()` 方法释放资源。



## 字符流

字符流的底层其实就是字节流

**字符流 = 字节流 + 字符集**

**特点**：

* 输入流：一次读一个字节，遇到中文时，一次读多个字节
* 输出流：底层会把数据按照指定的编码方式进行编码，变成字节再写到文件中

**使用场景**

对于纯文本文件进行读写操作

### FileReader

**一、构造方法**

`FileReader` 构造方法

| 构造方法                             | 说明                                                     |
| ------------------------------------ | -------------------------------------------------------- |
| `public FileReader(File file)`       | 创建字符输入流关联本地文件。                             |
| `public FileReader(String pathname)` | 创建字符输入流关联本地文件。参数是文件路径的字符串表示。 |

如果文件不存在，就直接报错

**二、书写步骤**

1. 创建字符输入流对象

​	如果文件不存在，就直接报错

2. 读取数据

| 方法签名                         | 方法功能             | 细节                                                         |
| -------------------------------- | -------------------- | ------------------------------------------------------------ |
| `public int read()`              | 读取数据             | 按字节读取，遇到多字节字符会一次读取多个字节并解码为整数     |
| `public int read(char[] buffer)` | 读取数据到字符数组中 | 按字节读取，遇到多字节字符会一次读取多个字节并解码为字符并填充到字符数组中 |

如果读取到文件末尾，则返回 -1。

3. 释放资源

`public int read()`：

默认也是一个字节一个字节的读取的，如果遇到中文就会一次读取多个，在读取之后，方法的底层还会进行解码并转成十进制。

返回值就是十进制

```java
public class Test01 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("a.txt");
        int i;
        while ((i = fr.read()) != -1) {
            System.out.print((char)i);
        }
    }
}
```

`public int read(char[] buffer)`：

读取数据，解码，强转三步合并了，把强转之后的字符放到数组当中

相当于空参的`read`+ 强转类型转换

```java
public class Test01 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("a.txt");
        int i;
        char[] chars = new char[2];
        int len;
        while ((len = fr.read(chars)) != -1) {
            System.out.print(new String(chars, 0, len));
        }
    }
}
```

### FIleWriter

**一、构造方法**

| 构造方法                                             | 说明                               |
| ---------------------------------------------------- | ---------------------------------- |
| `public FileWriter(File file)`                       | 创建字符输出流关联本地文件。       |
| `public FileWriter(String pathname)`                 | 创建字符输出流关联本地文件。       |
| `public FileWriter(File file, boolean append)`       | 创建字符输出流关联本地文件，续写。 |
| `public FileWriter(String pathname, boolean append)` | 创建字符输出流关联本地文件，续写。 |

**二、成员方法**

| 成员方法                                    | 说明                   |
| ------------------------------------------- | ---------------------- |
| `void write(int c)`                         | 写出一个字符           |
| `void write(String str)`                    | 写出一个字符串         |
| `void write(String str, int off, int len)`  | 写出一个字符串的一部分 |
| `void write(char[] cbuf)`                   | 写出一个字符数组       |
| `void write(char[] cbuf, int off, int len)` | 写出字符数组的一部分   |

**三、步骤**

1. 创建字符输出流对象

   细节1：参数是字符串表示的路径或者File对象都是可以的
   细节2：如果文件不存在会创建一个新的文件，但是要保证父级路径是存在的
   细节3：如果文件已经存在，则会清空文件，如果不想清空可以打开续写开关

2. 写数据

   如果write方法的参数是整数，但是实际上写到本地文件中的是整数在字符集上对应的字符

3. 释放资源

   细节：每次使用完流之后都要释放资源

```java
public class Test02 {
    public static void main(String[] args) throws IOException {
        FileWriter fw = new FileWriter("a.txt", true);
        
        fw.write(25105);
        fw.write("？？？");
        char[] chars = {'a', 'b', '你'};
        fw.write(chars);
        
        fw.close();
    }
}
```

### 原理分析

**一、字符输入流**

1. 创建字符输入流对象

   底层：关联文件，并创建缓冲区(长度为8192的字节数组)

2. 读取数据

   底层：判断缓冲区中是否有数据可以读取

   > * **缓冲区没有数据**：就从文件中获取数据，装到缓冲区中，每次尽可能装满缓冲区，
   >   如果文件中也没有数据了，返回-1。
   > * **缓冲区有数据**：就从缓冲区中读取
   >   空参的read方法：一次读取一个字节，遇到中文一次读多个字节，把字节解码并转成十进制返回
   >   有参的read方法：把读取字节，解码，强转三步合并了，强转之后的字符放到数组中

**二、字符输出流**

| 成员方法                  | 说明                               |
| ------------------------- | ---------------------------------- |
| **`public void flush()`** | 将缓冲区中的数据，刷新到本地文件中 |
| **`public void close()`** | 释放资源/关流                      |

flush刷新：刷新之后，还可以继续往文件中写出数据

close关流：断开通道，无法再往文件中写出数据

## 使用场景

字节流：拷贝任意类型的文件

字符流：读取纯文本文件中的数据，往纯文本文件中写出数据

**1.文件夹拷贝**

```java
public class Demo1 {
    public static void main(String[] args) throws IOException {
        File src = new File("C:\\Users\\HP\\Desktop\\111");d
        File dest = new File("C:\\Users\\HP\\Desktop\\222");

        copydir(src, dest);

    }

    private static void copydir(File src, File dest) throws IOException {
        dest.mkdirs();
        File[] files = src.listFiles();
        for (File file : files) {
            if (file.isFile()) {
                FileInputStream fis = new FileInputStream(file);
                FileOutputStream fos = new FileOutputStream(new File(dest, file.getName()));
                byte[] bytes = new byte[1024];
                int len;
                while ((len = fis.read(bytes)) != -1) {
                    fos.write(bytes, 0, len);
                }
                fos.close();
                fis.close();
            } else {
                copydir(file, new File(dest, file.getName()));
            }
        }
    }
}
```

**2.文件加密**

对每个字节异或2，当要通过加密文件获取源文件时，在进行一次异或操作即可

```java
public class Demo2 {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("C:\\Users\\HP\\Desktop\\111\\哈哈\\4月27日 日报.pdf");
        FileOutputStream fos = new FileOutputStream("C:\\Users\\HP\\Desktop\\111\\哈哈\\copy_4月27日 日报.pdf");
        int b;
        while ((b = fis.read()) != -1) {
            fos.write(b ^ 2);
        }
        fos.close();
        fis.close();
    }
}
```

**3.修改文件中数据**

```java
public class demo1 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("a.txt");
        StringBuilder sb = new StringBuilder();
        int ch;
        while((ch = fr.read()) != -1) {
            sb.append((char) ch);
        }
        fr.close();
        Integer[] array = Arrays.stream(sb.toString()
                        .split("-"))
                .map(new Function<String, Integer>() {
                    @Override
                    public Integer apply(String s) {
                        return Integer.parseInt(s);
                    }
                })
                .sorted()
                .toArray(new IntFunction<Integer[]>() {
                    @Override
                    public Integer[] apply(int value) {
                        return new Integer[value];
                    }
                });
        FileWriter fw = new FileWriter("a.txt");
        for (int i = 0; i < array.length; i++) {
            if(i == array.length - 1)
                fw.write(array[i] + "");
            else
                fw.write(array[i] + "-");
        }
        System.out.println(fw);
        fw.close();
    }
}
```

## 缓冲流

![Buffered](https://raw.githubusercontent.com/betteryuxuan/Image/main/Buffered.png)

## 字节缓冲流

### BufferedInputStream

### BufferedOutputStream

**一、构造方法**

**原理**：底层自带了长度为8192字节的缓冲区提高性能

| 方法名称                                       | 说明                                     |
| ---------------------------------------------- | ---------------------------------------- |
| `public BufferedInputStream(InputStream is)`   | 把基本流包装成高级流，提高读取数据的性能 |
| `public BufferedOutputStream(OutputStream os)` | 把基本流包装成高级流，提高写出数据的性能 |

**二、读写原理**

![bufferedInOutput](https://raw.githubusercontent.com/betteryuxuan/Image/main/bufferedInOutput.png)

**三、示例**

1. 一个字节拷贝文件

```java
public class Demo5 {
    public static void main(String[] args) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\CODE\\javacode\\5.26\\ggg.txt"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("E:\\CODE\\javacode\\5.26\\copy_ggg.txt"));
        int b;
        while ((b = bis.read()) != -1) {
            bos.write(b);
        }
        bos.close();
        bis.close();
    }
}
```

2. 一个字符数组拷贝文件

```java
package com.feng.test01;

import java.io.*;

public class Demo5 {
    public static void main(String[] args) throws IOException {
        BufferedInputStream bis = new BufferedInputStream(new FileInputStream("E:\\CODE\\javacode\\5.26\\ggg.txt"));
        BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("E:\\CODE\\javacode\\5.26\\copy_ggg.txt"));
        byte[] bytes = new byte[1024];
        int len;
        while ((len = bis.read(bytes)) != -1) {
            bos.write(bytes, 0, len);
        }
        bos.close();
        bis.close();
    }
}
```

## 字符缓冲流

### BufferedReader

### BufferedWriter

**一、构造方法**

原理：底层自带了长度为 8192 字符（16kb）的缓冲区提高性能

| 方法名称                          | 说明               |
| --------------------------------- | ------------------ |
| `public BufferedReader(Reader r)` | 把基本流变成高级流 |
| `public BufferedWriter(Writer w)` | 把基本流变成高级流 |

**二、特有方法**

| 方法名称                   | 说明                                            |
| -------------------------- | ----------------------------------------------- |
| `public String readLine()` | 读取一行数据，如果没有数据可读了，会返回 `null` |
| `public void newLine()`    | 跨平台的换行                                    |

1. `readLine`

   方法在读取的时候，一次读一整行，遇到回车换行结束，不会把回车换行读到内存当中

```java
public class Demo6 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new FileReader("E:\\CODE\\javacode\\5.26\\ggg.txt"));
        String s;
        while ((s = br.readLine()) != null) {
            System.out.println(s);
        }
        br.close();
    }
}
```

2. `newLine`

```java
public class Demo7 {
    public static void main(String[] args) throws IOException {
        BufferedWriter bw=new BufferedWriter(new FileWriter("E:\\CODE\\javacode\\5.26\\aaa.txt"));
        bw.write("abc");
        bw.newLine();
        bw.write("def");
        bw.newLine();
        bw.close();
    }
}
```

## 转换流

### InputStreamReader

### OutputStreamWriter

**一、体系**

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Stranmreader.png" alt="Stranmreader" style="zoom: 50%;" />

是字符流和字节流之间的桥梁

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/yuanli1.png" alt="yuanli1" style="zoom: 50%;" />

**二、示例**

1. 读取gbk文件

```java
public class Demo8 {
    public static void main(String[] args) throws IOException {
       /* InputStreamReader isr = new InputStreamReader(new FileInputStream("C:\\Users\\HP\\Desktop\\111\\111.txt"), "gbk");
        int ch;
        while ((ch = isr.read()) != -1) {
            System.out.print((char) ch);
        }
        isr.close();*/

        FileReader fr = new FileReader("C:\\Users\\HP\\Desktop\\111\\111.txt", Charset.forName("gbk"));
        int ch;
        while ((ch = fr.read()) != -1) {
            System.out.print((char) ch);
        }
        fr.close();
    }
}

```

2.  写出gbk编码文件数据

```java
public class Demo9 {
    public static void main(String[] args) throws IOException {
        /*OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream("..\\5.26\\b.txt"), "GBK");
        osw.write("你好你好");
        osw.close();*/

        FileWriter fw = new FileWriter("..\\5.26\\c.txt", Charset.forName("GBK"));
        fw.write("你好你好你好你好");
        fw.close();
    }
}
```

3. gbk读入程序，UTF写入文件

```java
public class Demo10 {
    public static void main(String[] args) throws IOException {
        FileReader fr = new FileReader("..\\5.26\\c.txt", Charset.forName("GBK"));
        FileWriter fw = new FileWriter("..\\5.26\\5.26e.txt", Charset.forName("UTF-8"));
        int b;
        while ((b = fr.read()) != -1) {
            fw.write(b);
        } fw.close();
        fr.close();
    }
}
```

4. 利用字节流读取文件中的数据，每次读一整行，而且不能出现乱码
   1. 字节流在读取中文的时候，是会出现乱码的，但是字符流可以搞定
   2. 字节流里面是没有读一整行的方法的，只有字符缓冲流才能搞定

```java
public class Demo11 {
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(new FileInputStream("E:\\CODE\\javacode\\5.26\\b.txt")));
        String line;
        while ((line = br.readLine()) != null) {
            System.out.println(line);
        }
        br.close();
    }
}
```

## 序列化流

![xuliehua](https://raw.githubusercontent.com/betteryuxuan/Image/main/xuliehua.png)

### ObjectOutputStream（序列化流）

**序列化流/对象操作输出流**

**一、构造方法**

| 构造方法                                      | 说明                   |
| --------------------------------------------- | ---------------------- |
| `public ObjectOutputStream(OutputStream out)` | 把基本流包装成高级流。 |

将基本输出流包装成高级输出流 `ObjectOutputStream`。它接受一个 `OutputStream` 参数，用于指定要写入的目标流。

**二、成员方法**

| `public final void writeObject(Object obj)` | 把对象序列化（写出）到文件中去。 |
| ------------------------------------------- | -------------------------------- |

参数 `obj` 是要写入的对象，它必须是可序列化的，否则将抛出 `java.io.NotSerializableException` 异常。序列化的过程会将对象转换为字节流，以便于输出到文件中。

**三、`Serializable`接口**

> **`需要让`Javabean`类实现`Serializable`接口：**
>
> `Serializable`接口里面是没有抽象方法，标记型接口
> 一旦实现了这个接口，那么就表示当前的`Student`类可以被序列化

### ObjectInputSrteam（反序列化流）

**反序列化流 /对象操作输入流**

> 可以把序列化到本地文件中的对象，读取到程序中来

**一、构造方法**

| `public ObjectInputStream(InputStream in)` | 把基本流变成高级流 |
| ------------------------------------------ | ------------------ |

**二、成员方法**

| `public Object readObject()` | 把序列化到本地文件中的对象，读取到程序中来。 |
| ---------------------------- | -------------------------------------------- |

**三、细节**

1. 使用序列化流将对象写到文件时，需要让`Javabean`类实现`Serializable`接口。否则，会出现`NotSerializableException`异常
2. 序列化流写到文件中的数据是不能修改的，一旦修改就无法再次读回来了
3. 序列化对象后，修改了`Javabean`类，再次反序列化，会出问题，会抛出`InvalidclassException`异常
   解决方案：给`Javabean`类添加`serialVersionUID`(列号、版本号)
4. 如果一个对象中的某个成员变量的值不想被序列化，如何实现?
   解决方案：给该成员变量加`transient`关键字修饰，该关键字标记的成员变量不参与序列化过程

`transient`（瞬态关键字）：不会把当前属性序列化到本地文件当中

```java
public class Student implements Serializable {
    @Serial
    private static final long serialVersionUID = -4657785577835691959L;
    private String name;
    private int age;
    private transient Strign address;
}
```

## 打印流

![printStram](https://raw.githubusercontent.com/betteryuxuan/Image/main/printStram.png)

打印流通常指的是`PrintStream`和`PrintWriter`两个类。

**特点1**：打印流只操作文件目的地，不操作数据源。

**特点2**：<u>打印流有特有的写出方法，可以实现数据的原样写出</u>。

例如，当你打印整数`97`时，它会以字符串形式写入到文件中；当你打印布尔值`true`时，也会以字符串形式写入到文件中。

**特点3**：<u>打印流还有特有的写出方法，可以实现自动刷新和自动换行。</u>每次调用打印方法时，它会自动写出数据、换行并刷新输出流。这使得打印一次数据等同于写出数据、换行、刷新输出流。

### PrintStream

字节打印流

**一、构造方法**

| 构造方法签名                                                 | 说明                                           |
| ------------------------------------------------------------ | ---------------------------------------------- |
| `public PrintStream(OutputStream/File/String)`               | 关联字节输出流。                               |
| `public PrintStream(String fileName, Charset charset)`       | 关联文件，并指定字符编码。                     |
| `public PrintStream(OutputStream out, boolean autoFlush)`    | 关联字节输出流，并启用自动刷新。               |
| `public PrintStream(OutputStream out, boolean autoFlush, String encoding)` | 关联字节输出流，指定字符编码，并启用自动刷新。 |

字节流底层没有缓冲区，开不开自动刷新都一样

**三、成员方法**

| 方法签名                                            | 说明                                                     |
| --------------------------------------------------- | -------------------------------------------------------- |
| `public void write(int b)`                          | 常规方法：按照之前的规则，将指定的字节写出               |
| `public void println(Xxx xx)`                       | **特有方法**：打印任意数据，自动换行、自动刷新           |
| `public void print(Xxx xx)`                         | **特有方法**：打印任意数据，不会换行                     |
| `public void printf(String format, Object... args)` | **特有方法**：带有占位符的打印语句，格式化输出，不会换行 |

```java
public class Demo13 {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps = new PrintStream(new FileOutputStream("E:\\CODE\\javacode\\5.26\\c.txt"), true, Charset.forName("UTF-8"));
        ps.write(97);
        ps.println(97);
        ps.print(true);
        ps.println('c');
        ps.printf("%d 位同学参加了 %s ", 2000, "爱国运动");
        ps.close();
    }
}
```

### PrintWriter

**字符打印流**

**一、构造方法**

| 构造方法                                                     | 说明                         |
| ------------------------------------------------------------ | ---------------------------- |
| `public PrintWriter(Writer/File/String)`                     | 关联字节输出流/文件/文件路径 |
| `public PrintWriter(String fileName, Charset charset)`       | 指定字符编码                 |
| `public PrintWriter(Writer w, boolean autoFlush)`            | 自动刷新                     |
| `public PrintWriter(OutputStream out, boolean autoFlush, Charset charset)` | 指定字符编码且自动刷新       |

字符流底层有缓冲区，**想要自动刷新需要开启**

**二、成员方法**

| 成员方法                                            | 说明                                         |
| --------------------------------------------------- | -------------------------------------------- |
| `public void write(...)`                            | 常规方法：规则跟之前一样，写出字节或者字符串 |
| `public void println(Xxx xx)`                       | 特有方法：打印任意类型的数据并且换行         |
| `public void print(Xxx xx)`                         | 特有方法：打印任意类型的数据，不换行         |
| `public void printf(String format, Object... args)` | 特有方法：带有占位符的打印语句               |

`println`自动刷新与否取决于创建对象时选择没

如果使用带有自动刷新选项的构造方法（例如，在构造方法的第二个参数中传入 `true`），则 `println` 方法会在输出后自动刷新。

如果不启用自动刷新，则需要手动调用 `flush` 方法来刷新输出流。

**三、示例**

```java
public class Demo13 {
    public static void main(String[] args) throws FileNotFoundException {
        PrintStream ps = new PrintStream(new FileOutputStream("E:\\CODE\\javacode\\5.26\\c.txt"), true, Charset.forName("UTF-8"));
        ps.write(97);
        ps.println(97);
        ps.print(true);
        ps.println('c');
        ps.printf("%d 位同学参加了 %s ", 2000, "爱国运动");
        ps.close();
    }
}
```

**四、sout**

标准输出流是在虚拟机启动时由虚拟机创建的，并且默认指向控制台。标准输出流是特殊的打印流，用于向控制台打印输出。

由于标准输出流是在虚拟机启动时创建的，而且在系统中是唯一的，因此在一般情况下<u>**不应关闭标准输出流**</u>。关闭标准输出流可能会导致后续的打印操作失败或抛出异常。

```java
public class Demo14{
    public static void main(String[] args) {
        PrintStream ps = System.out;
        ps.println("abc");
        ps.close();
        ps.println("def");
        ps.println("ghm");
    }
 }
```

## 压缩流

![yasuo](https://raw.githubusercontent.com/betteryuxuan/Image/main/yasuo.png)

### ZipInputStream（解压缩流）

> 解压ZIP文件的本质：将每一个 `ZipEntry`（ZIP 文件中的每个条目，包括文件和目录）按照其在 ZIP 文件中的层级结构复制到本地目标文件夹中。

**一、构造方法**

| 构造方法                                             | 描述                                                 |
| ---------------------------------------------------- | ---------------------------------------------------- |
| `ZipOutputStream(OutputStream out)`                  | 创建一个新的压缩输出流，将数据写入指定的基本输出流。 |
| `ZipOutputStream(OutputStream out, Charset charset)` | 创建一个新的压缩输出流，并指定字符集。               |

**二、常用方法**

| 常用方法                                 | 描述                                    |
| ---------------------------------------- | --------------------------------------- |
| `void putNextEntry(ZipEntry e)`          | 开始写入一个新的ZIP文件条目。           |
| `void write(byte[] b, int off, int len)` | 从指定的字节数组写入数据到当前ZIP条目。 |
| `void closeEntry()`                      | 关闭当前ZIP条目。                       |
| `void finish()`                          | 完成ZIP输出流并写入ZIP文件结束记录。    |
| `void close()`                           | 关闭ZIP输出流并释放资源。               |

> `ZipEntry` 是 Java 中 `java.util.zip` 包的一个类，它用于表示 ZIP 文件中的一个条目（即 ZIP 文件中的一个单独的文件或目录）。`ZipEntry` 对象包含了 ZIP 条目的元数据，如名称、大小、压缩方法、CRC 校验和等，但不包含实际的文件数据。

**三、压缩示例**

```java
import java.io.*;
import java.util.zip.ZipEntry;
import java.util.zip.ZipInputStream;

public class ZipStreamDemo1 {

    // 主函数
    public static void main(String[] args) throws IOException {
        // 指定源ZIP文件和目标解压目录
        File src = new File("E:\\TestCode\\周题.zip");
        File dest = new File("E:\\TestCode\\周题");
        // 创建目标解压目录
        dest.mkdirs();
        // 解压ZIP文件
        unzip(src, dest);
    }

    // 解压方法
    public static void unzip(File src, File dest) throws IOException {
        // 创建ZIP输入流
        ZipInputStream zip = new ZipInputStream(new FileInputStream(src));
        ZipEntry entry;
        // 遍历ZIP文件中的条目
        while ((entry = zip.getNextEntry()) != null) {
            System.out.println(entry);
            // 如果是目录，则创建目录
            if (entry.isDirectory()) {
                File file = new File(dest, entry.getName());
                file.mkdirs();
            } else {
                // 如果是文件，则创建文件并写入数据
                FileOutputStream fos = new FileOutputStream(new File(dest, entry.getName()));
                byte[] bytes = new byte[1024];
                int len;
                while ((len = zip.read(bytes)) != -1) {
                    fos.write(bytes, 0, len);
                }
                fos.close();
                zip.closeEntry();
            }
        }
        // 关闭输入流
        zip.close();
    }
}
```

### ZipOutputStream（压缩流）

> 压缩本质：把每一个(文件/文件夹)看成`ZipEntry`对象放到压缩包中

应该是src.getName().split("\\.")[length-2]+".zip"

**一、构造方法**

| 构造方法                         | 描述                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| `ZipInputStream(InputStream in)` | 创建一个新的解压缩输入流，从指定的基本输入流中读取ZIP文件数据。 |

**二、常用方法**

| 常用方法                               | 描述                                      |
| -------------------------------------- | ----------------------------------------- |
| `ZipEntry getNextEntry()`              | 读取ZIP文件中的下一个条目。               |
| `int read(byte[] b, int off, int len)` | 从当前ZIP条目中读取数据到指定的字节数组。 |
| `void closeEntry()`                    | 关闭当前ZIP条目（通常不需要直接调用）。   |
| `void close()`                         | 关闭ZIP输入流并释放资源。                 |

**三、示例**

1. 压缩单个文件

```java
public class ZipStreamDemo2 {
    public static void main(String[] args) throws IOException {
        // 源文件路径
        File src =new File("E:\\TestCode\\testFile.txt");
        // 压缩文件存储路径
        File dest =new File("E:\\TestCode");
        // 执行文件压缩
        toZip(src, dest);
    }


    private static void toZip(File src, File dest) throws IOException {
        // 创建压缩输出流
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(new File(dest, "testFile.zip")));
        // 创建一个zip文件实体并添加到压缩流中
        ZipEntry entry = new ZipEntry(src.getName());
        zos.putNextEntry(entry);
        // 读取源文件并写入压缩流
        FileInputStream fis = new FileInputStream(src);
        int b;
        while ((b = fis.read()) != -1) {
            zos.write(b);
        }
        // 关闭当前zip条目并压缩流
        zos.closeEntry();
        zos.close();
    }
}
```

2. 压缩文件夹

哪里似乎出错了

 ```java
public class ZipStreamDemo3 {
    // 主函数，程序入口
    public static void main(String[] args) throws IOException {
        File src = new File("E:\\TestCode\\111");
        File dest = new File(src.getParent(), src.getName() + ".zip");
        ZipOutputStream zos = new ZipOutputStream(new FileOutputStream(dest));
        toZip(src, zos, src.getName());  // 调用toZip方法将文件压缩
        zos.close();  // 关闭ZipOutputStream
    }

    // 将文件或目录压缩成zip格式
    private static void toZip(File src, ZipOutputStream zos, String name) throws IOException {
        File[] files = src.listFiles();
        for (File file : files) {
            if (file.isFile()) {
                ZipEntry entry = new ZipEntry(name + "/" + file.getName());
                zos.putNextEntry(entry);  // 开始写入新的ZipEntry
                FileInputStream fis = new FileInputStream(file);
                byte[] bytes = new byte[1024];
                int len;
                while ((len = fis.read(bytes)) != -1) {
                    zos.write(bytes, 0, len);  // 逐字节将文件数据写入ZipOutputStream
                }
                fis.close();  // 关闭文件输入流
                zos.closeEntry();  // 关闭当前ZipEntry
            } else {
                toZip(src, zos, name + "/" + file.getName());  // 递归压缩子目录或文件
            }
        }
    }
}
 ```



## Commons-io

> Apache Commons IO是Apache软件基金会下的一个子项目，主要提供了一些实用的IO功能，用于简化Java IO操作。Commons IO包括了一些非常有用的类和方法，能够帮助开发者更高效地进行文件和流的操作。

**一、使用步骤**

1. 在项目中创建一个文件夹：lib
2. 将jar包复制粘贴到lib文件夹
3. 右键点击jar包，选择 Add as Library->点击OK
4. 在类中导包使用

**二、常用方法**

1. FileUtils类（文件/文件夹相关）

| 方法                                                         |                     说明                     |
| :----------------------------------------------------------- | :------------------------------------------: |
| `static void copyFile(File srcFile, File destFile)`          |                   复制文件                   |
| `static void copyDirectory(File srcDir, File destDir)`       |     复制文件夹（最外层文件夹里所有内容）     |
| `static void copyDirectoryToDirectory(File srcDir, File destDir)` | 复制文件夹（最外层文件夹加文件夹里所有内容） |
| `static void deleteDirectory(File directory)`                |                  删除文件夹                  |
| `static void cleanDirectory(File directory)`                 |                  清空文件夹                  |
| `static String readFileToString(File file, Charset encoding)` |          读取文件中的数据变成字符串          |
| `static void write(File file, CharSequence data, String encoding)` |                   写出数据                   |

2. IOUtils类（流相关）

| 方法                                                         | 说明       |
| ------------------------------------------------------------ | ---------- |
| `public static int copy(InputStream input, OutputStream output)` | 复制文件   |
| `public static int copyLarge(Reader input, Writer output)`   | 复制大文件 |
| `public static String readLines(Reader input)`               | 读取数据   |
| `public static void write(String data, OutputStream output)` | 写出数据   |

**三、示例**

```java
package com.feng.myzipstream;

import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;

public class ZipStreamDemo4 {
    public static void main(String[] args) throws IOException {
        File src = new File("E:\\TestCode\\111\\新建文件夹\\testFile.txt");
        File dest = new File("E:\\TestCode\\111\\新建文件夹\\copy_testFile.txt");
        FileUtils.copyFile(src,dest);
    }
}
```

## hutool

**一、常用方法**

| 类名                    | 说明                              |
| ----------------------- | --------------------------------- |
| **`IOUtils`**           | **流操作工具类**                  |
| **`FileUtils`**         | **文件读写和操作的工具类**        |
| **`FileTypeUtils`**     | **文件类型判断工具类**            |
| **`WatchMonitor`**      | **目录、文件监听**                |
| **`ClassPathResource`** | **针对ClassPath中资源的访问封装** |
| **`FileReader`**        | **封装文件读取**                  |
| **`FileWriter`**        | **封装文件写入**                  |

## 配置文件

> properties是一个双列集合集合，拥有Map集合所有的特点。
>
> 重点：有一些特有的方法，可以把集合中的数据，按照键值对的形式写到配置文件当中
> 也可以把配置文件中的数据，读取到集合中来。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240611215508482.png" alt="image-20240611215508482" style="zoom:50%;" />

### 读写方法

**Properties中特有的读写方法**

写入文件

```java
public class Test01 {
    public static void main(String[] args) throws IOException {
        //创建集合对象
        Properties prop = new Properties();

        prop.put("aaa", "111");
        prop.put("aaa", "111");
        prop.put("bbb", "222");

        FileOutputStream fos = new FileOutputStream("E:\\CODE\\javacode\\properties\\a.properties");
        prop.store(fos,"test");//第二个参数是文件注释
        fos.close();
    }
}

```

读取文件

```java
public class Test02 {
    public static void main(String[] args) throws IOException {
        Properties prop = new Properties();
        FileInputStream fis = new FileInputStream("E:\\CODE\\javacode\\properties\\a.properties");
        prop.load(fis);
        fis.close();

        Set<Object> keys = prop.keySet();
        for (Object key : keys) {
            Object value = prop.get(key);
            System.out.println(key + "=" + value);
        }
    }
}
```

### 好处

1. 可以把软件的设置永久化存储
2. 如果我们要修改参数，不需要改动代码，直接修改配置文件就可以了



## 编码

### 标准ASCII字符集

美国制定，包含美国使用的英文字符等，共128个字符

每一个字符对应一个码点，码点转化为二进制，首位是`0`

![521.1](https://raw.githubusercontent.com/betteryuxuan/Image/main/521.1.png)

### GBK

> 国标，汉字编码字符集
>
> 包含两万多个汉字等字符，`GBK`中一个中文字符编码成两个字节形式存储
>
> * 汉字第一个字节的第一位必须是`1`，所以能代表`2^15=32768`个
>
> * **`GBK`兼容`ASCII`字符集**

英文和ASCII一致

![gbk](https://raw.githubusercontent.com/betteryuxuan/Image/main/gbk.png)

### Unicode字符集

>  （ 统一码，万国码)
>
>  国际组织制定，容纳世界上所有文字，符号的字符集
>
>  Unicode Transfer Format

* **UTF-32**

四个字节一个字符

* **UTF-16**

2-4个字节一个字符

* **UTF-8** 

可变长编码方案，四个长度区：1，2，3，4

英文数字，字符占一个字节，汉字字符占用三个字节

![utf](https://raw.githubusercontent.com/betteryuxuan/Image/main/utf.png)

### 总结

![bianma](https://raw.githubusercontent.com/betteryuxuan/Image/main/bianma.png)

* 字符编码时使用字符集和解码使用时字符集需要一致
* 英文数字一般不会乱码

### 乱码

**原因**：

1. 读取数据时未读完整个汉字
2. 编码和解码时的方式不统一 

**如何不产生乱码**：

1. 不要用字节流读取文本文件
2. 编码解码时使用同一个码表，同一个编码方式

### 编码与解码

| **方法名称**                                     | **说明**                   |
| ------------------------------------------------ | -------------------------- |
| **`public byte[] getBytes()`**                   | **使用默认方式进行编码。** |
| **`public byte[] getBytes(String charsetName)`** | **使用指定方式进行编码。** |

| **`public String(byte[] bytes)`**                     | **使用默认方式进行解码。** |
| ----------------------------------------------------- | -------------------------- |
| **`public String(byte[] bytes, String charsetName)`** | **使用指定方式进行解码。** |

```java
package com.feng.test03;

import java.io.UnsupportedEncodingException;

public class Test03 {
    public static void main(String[] args) throws UnsupportedEncodingException {
        String s = "ni好";
        byte[] bytes1 = s.getBytes();
        byte[] bytes2 = s.getBytes("GBK");
        System.out.println(new String(bytes1));
        System.out.println(new String(bytes2, "GBK"));
    }
}
```

# 多线程

线程是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位。

进程是程序的基本执行实体

理解： 应用软件中互相独立，可以同时运行的功能

## 并发并行

并发：在同一时刻，有多个指令在单个CPU上交替执行 
并行： 在同一时刻，有多个指令在多个CPU上同时执行

## 实现方式

**一、继承Thread类的方式进行实现**

1. **自己定义一个类继承`Thread`**
2. **重写`run`方法**
3. **创建子类的对象，并启动线程**

```java
dpublic class MyThread extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(getName() + "Hello");
        }
    }
}

public class Test01 {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();

        t1.setName("线程1");
        t2.setName("线程2");

        t1.start();
        t2.start();
    }
}
```

**二、实现Runnable接口的方式**

1. **自己定义一个类实现Runnable接口**
2. **重写里面的run方法**
3. **创建自己的类的对象**
4. **创建一个Thread类的对象，并开启线程进行实现**

```java
public class MyRun implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println(Thread.currentThread().getName() + "Hello");
        }
    }
}

public class Test02 {
    public static void main(String[] args) {
        MyRun mr = new MyRun();
        Thread t1 = new Thread(mr);
        Thread t2 = new Thread(mr);

        t1.setName("线程1");
        t2.setName("线程2");

        t1.start();
        t2.start();
    }
}
```

**三、利用Callable接口和Future接口方式实现**

1. 创建一个类`MyCallable`实现`Callable`接口
2. 重写`call` (是有返回值的，表示多线程运行的结果)
3. 创建`MyCallable`的对象(表示多线程要执行的任务)
4. 创建`FutureTask`的对象(作用管理多线程运行的结果)
5. 创建`Thread`类的对象，并启动(表示线程)

```java
public class MyCallable implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        int sum = 0;
        for (int i = 1; i <= 100; i++) {
            sum += i;
        }
        return sum;
    }
}

public class Test03 {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建MyCallable的对象（表示多线程要执行的任务）
        MyCallable mc = new MyCallable();
        // 将可调用的任务包装成FutureTask（作用管理多线程运行的结果）
        FutureTask<Integer> ft = new FutureTask(mc);
        // 创建线程并启动
        Thread t1 = new Thread(ft);
        t1.start();
        // 获取任务执行结果并打印
        System.out.println(ft.get());
    }
}
```

**四、对比**

![thread](https://raw.githubusercontent.com/betteryuxuan/Image/main/thread.png)

Callable有返回值

## 成员方法

| **方法名称**                               | **说明**                                   |
| ------------------------------------------ | ------------------------------------------ |
| **`String getName()`**                     | **返回此线程的名称**                       |
| **`void setName(String name)`**            | **设置线程的名字(构造方法也可以设置名字)** |
| <u>**`static Thread currentThread()`**</u> | **获取当前线程的对象**                     |
| <u>**`static void sleep(long time)`**</u>  | **让线程休眠指定的时间，单位为毫秒**       |
| **`void setPriority(int newPriority)`**    | **设置线程的优先级**                       |
| **`final int getPriority()`**              | **获取线程的优先级**                       |
| **`final void setDaemon(boolean on)`**     | **设置为守护线程**                         |
| **`public static void yield()`**           | **出让线程/礼让线程**                      |
| **`public static void join()`**            | **插入线程/插队线程**                      |

1. `setName`

   1>如果我们没有给线程设置名字，线程也是有默认的名字的
   格式：Thread-x(X号，从0开始的)
   2>如果我们要给线程设置名字，可以用<u>set</u>方法进行设置，也可以<u>构造方法</u>设置

   自己定义的继承Thread的类构造方法要调用父类 `Thread` 的构造方法

2. `currentThread`

   当JVM虚拟机启动之后，会自动的启动多条线程
   其中有一条线程就叫做main线程，作用是调用main方法，并执行里面的代码

3. `sleep`

   哪条线程执行到这个方法，那么哪条线程就会在这里停留对应的时间
   方法的参数：就表示睡眠的时间，单位毫秒
   当时间到了之后，线程会自动的醒来，继续执行下面的其他代码

4. `setPriority`

   线程的优先级是指操作系统调度线程执行的相对重要程度。

   Java中线程的优先级范围从1到10，默认值为5。

   使用 `setPriority(int newPriority)` 方法设置线程的优先级，其中 `newPriority` 参数表示新的线程优先级，取值范围为1到10，数值越大表示优先级越高。

5. **守护线程`setDaemon`**

   **守护线程**是一种在后台提供服务的线程，当所有非守护线程结束时，守护线程会随之自动退出。

   守护线程通常用于在程序运行期间执行一些辅助性的任务，例如垃圾回收器、后台数据保存等。

6. **礼让线程`yield`**

   提示线程调度器当前线程愿意放弃其当前的使用时间片，使其他等待的线程有机会运行。

   它的作用并不是停止当前线程或确保其他线程会立即执行，而是告诉线程调度器：如果有其他相同优先级的线程在等待运行，那么现在是一个切换的好时机。

```java
public class Yield {
    public static void main(String[] args) {
        Runnable mr = new Runnable() {
            public void run() {
                for (int i = 0; i < 5; i++) {
                    System.out.println(Thread.currentThread().getName() + " is running");
                    Thread.yield();  // 提示线程调度器当前线程愿意让出CPU时间片
                }
            }
        };

        Thread thread1 = new Thread(mr, "Thread 1");
        Thread thread2 = new Thread(mr, "Thread 2");

        thread1.start();
        thread2.start();
    }
}
```

* **不可预测的行为**：`Thread.yield()` 是一个提示，不是强制，具体是否让出时间片由线程调度器决定，不同的JVM实现和操作系统可能有不同的行为。
* **优先级**：它只会让给相同或更高优先级的线程，低优先级的线程可能仍然不会被调度。

7. 插入线程`join`

`join()` 方法允许一个线程等待另一个线程完成执行。它是一个实例方法，当在一个线程对象上调用时，调用线程将等待该线程完成。

```java
public class Join {
    public static void main(String[] args) throws InterruptedException {
        Runnable printTask = new Runnable() {
            public void run() {
                for (int i = 1; i <= 10; i++) {
                    System.out.println(Thread.currentThread().getName() + i);
                    }
                }
        };
        Thread thread1 = new Thread(printTask);
        thread1.start();
        
        thread1.join();
        
        for (int i = 1; i <= 10; i++) {
            System.out.println(Thread.currentThread().getName() + i);
        }
    }
}
```

## 生命周期

![StreamLifeTime](https://raw.githubusercontent.com/betteryuxuan/Image/main/StreamLifeTime.png)

- 新建状态:

  使用 **new** 关键字和 **Thread** 类或其子类建立一个线程对象后，该线程对象就处于新建状态。它保持这个状态直到程序 **start()** 这个线程。

- 就绪状态:

  当线程对象调用了start()方法之后，该线程就进入就绪状态。就绪状态的线程处于就绪队列中，要等待JVM里线程调度器的调度。

- 运行状态:

  如果就绪状态的线程获取 CPU 资源，就可以执行 **run()**，此时线程便处于运行状态。处于运行状态的线程最为复杂，它可以变为阻塞状态、就绪状态和死亡状态。

- 阻塞状态:

  如果一个线程执行了sleep（睡眠）、suspend（挂起）等方法，失去所占用资源之后，该线程就从运行状态进入阻塞状态。在睡眠时间已到或获得设备资源后可以重新进入就绪状态。可以分为三种：

  - 等待阻塞：运行状态中的线程执行 wait() 方法，使线程进入到等待阻塞状态。
  - 同步阻塞：线程在获取 synchronized 同步锁失败(因为同步锁被其他线程占用)。
  - 其他阻塞：通过调用线程的 sleep() 或 join() 发出了 I/O 请求时，线程就会进入到阻塞状态。当sleep() 状态超时，join() 等待线程终止或超时，或者 I/O 处理完毕，线程重新转入就绪状态。

- 死亡状态:

  一个运行状态的线程完成任务或者其他终止条件发生时，该线程就切换到终止状态。

## 同步代码块

同步代码块是在Java中用来同步多个线程对共享资源访问的一种机制。它可以确保在同一时间只有一个线程可以访问被同步的代码块，从而避免多个线程同时修改共享资源而导致的数据不一致或竞态条件问题。

**一、格式**

```java
synchronized(锁){
	操作共享数据的代码
} 
```

**二、特点**

1. 锁默认打开，有一个线程进去了，锁自动关闭
2. 与里面的代码全部执行完毕，线程出来，锁自动打开

**三、写法**

1. 循环
2. 同步代码块
3. 判断共享数据是否到了末尾(到了末尾)
4. 判断共享数据是否到了末尾(没有到末尾，执行核心逻辑)

**四、示例**

```java
package com.feng.threadcase5;

public class MyThread extends Thread{
    static int ticket = 0;

    static Object obj = new Object();

    @Override
    public void run() {
        while (true) {
            synchronized (MyThread.class) {
                if (ticket < 100) {
                    try {
                        Thread.sleep(10);
                    } catch (InterruptedException e) {
                        throw new RuntimeException(e);
                    }
                    ticket++;
                    System.out.println(getName() + "卖出了第" + ticket + "张票");
                } else {
                    break;
                }
            }
        }
    }
}

package com.feng.threadcase5;

public class Test {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        MyThread t2 = new MyThread();
        MyThread t3 = new MyThread();

        t1.start();
        t2.start();
        t3.start();
    }
}
```

## 同步方法

在Java中，可以通过在方法声明中加入`synchronized`关键字来将其声明为同步方法。这样做可以确保在同一时间只有一个线程能够执行该方法的代码，从而防止并发访问导致的数据不一致问题。

**一、语法**

```java
修饰符 synchronized 返回值类型 方法名(方法参数) {
    // 方法体
}
```

**二、特点**

1. **锁住所有代码**：当一个线程进入一个同步方法时，它会获得一个锁，并锁住该方法内的所有代码，直到该方法执行完毕。这期间，其他线程无法进入该同步方法。

2. **锁对象的指定**：
   - **非静态方法**：对于非静态同步方法，锁对象是`this`，即调用该方法的对象实例。
   - **静态方法**：对于静态同步方法，锁对象是这个类对应的`Class`对象（类的字节码文件对象），而不是某个具体的实例对象。

**三、锁对象**

- **非静态方法**：锁对象是`this`，意味着如果你创建了类的多个实例，每个实例的方法调用都不会互相干扰，因为它们各自锁定的是不同的`this`对象。

- **静态方法**：锁对象是类的`Class`对象。由于这个锁是类级别的，即使是类的不同实例调用这个静态同步方法，也只有一个能够进入，其他线程必须等待。

**四、示例**

```java
public class SynchronizedExample {
    // 非静态同步方法，锁对象是this
    public synchronized void synchronizedMethod() {
        // 同步代码块
    }

    // 静态同步方法，锁对象是SynchronizedExample.class
    public static synchronized void synchronizedStaticMethod() {
        // 同步代码块
    }
}
```

在使用同步方法时，需要注意的是，过度同步可能导致性能问题，因为它会阻止多线程并发执行。因此，通常只在必要时才使用同步方法，或者在方法中只对关键部分进行同步（使用同步块）。此外，对于静态同步方法要特别小心，因为它们会锁定整个类，从而可能阻止其他线程访问该类的任何其他静态同步方法。

```java
package com.feng.threadcase6;

public class MyRunnable implements Runnable {
    int ticket = 0;

    @Override
    public void run() {
        while (true) {
            if (method())
                break;
        }
    }

    public synchronized boolean method() {
        if (ticket == 100) {
            return true;
        } else {
            ticket++;
            System.out.println(Thread.currentThread().getName() + "正在抢第" + ticket + "张票");
        }
        return false;
    }
}

package com.feng.threadcase6;

public class Test {
    public static void main(String[] args) {
        MyRunnable mr = new MyRunnable();

        Thread t1= new Thread(mr);
        Thread t2= new Thread(mr);

        t1.setName("窗口1");
        t2.setName("窗口2");

        t1.start();
        t2.start();
    }
}

```

## lock锁

在Java中，`java.util.concurrent.locks.Lock`接口提供了一种更为灵活的线程同步机制，相对于内置的`synchronized`关键字，它提供了更多的控制和更广泛的锁定操作。

| 方法          | 描述   |
| ------------- | ------ |
| void lock()   | 获得锁 |
| void unlock() | 释放锁 |

`Lock`是一个接口，因此不能直接实例化。

Java提供了`java.util.concurrent.locks.ReentrantLock`类作为`Lock`接口的一个实现。

`ReentrantLock`是一个互斥锁，它具有与使用`synchronized`方法和块相同的内存语义，但功能更强大。

**格式：**

```java
import java.util.concurrent.locks.Lock;  
import java.util.concurrent.locks.ReentrantLock;  
  
public class LockExample {  
    private Lock lock = new ReentrantLock();  
  
    public void doSomething() {  
        lock.lock(); // 手动上锁  
        try {  
            // 访问或修改共享资源的代码  
            // ...  
        } finally {  
            lock.unlock(); // 手动释放锁，确保在finally块中执行以处理异常  
        }  
    }  
}
```

**示例：**

```java
package com.feng.threadcase5;

import java.util.concurrent.locks.Lock;
import java.util.concurrent.locks.ReentrantLock;

public class MyThread extends Thread{
    static int ticket = 0;

    static Lock lock = new ReentrantLock();

    @Override
    public void run() {
        while (true) {
            lock.lock();
            try {
                if (ticket != 100) {
                    ticket++;
                    System.out.println(getName() + "卖出了第" + ticket + "张票");
                } else {
                    break;
                }
            } catch (Exception e) {
                throw new RuntimeException(e);
            } finally {
                lock.unlock();
            }
        }
    }
}

```

## 死锁

**一、定义**

死锁是指两个或多个线程因争夺资源而造成的相互等待的状态，导致所有线程都无法继续执行的情况。

**二、条件**

死锁的产生通常需要满足以下四个条件：互斥条件、请求与保持条件、不可剥夺条件、循环等待条件。

**三、避免方法**

- 加锁顺序：确保所有线程以相同的顺序获取锁，避免形成循环等待。
- 使用tryLock()：设置超时时间，避免长时间等待。
- 资源分配图：对资源进行建模，预测和避免潜在的死锁。
- 避免长时间持有锁：减少持有锁的时间，降低死锁风险。
- 使用Lock对象的tryLock()方法：尝试获取锁并根据返回值决定如何处理，避免无限等待。

## 等待唤醒机制

**一、常用方法**

| 方法名称           | 说明                             |
| ------------------ | -------------------------------- |
| `void wait()`      | 当前线程等待，直到被其他线程唤醒 |
| `void notify()`    | 随机唤醒单个线程                 |
| `void notifyAll()` | 唤醒所有线程                     |

**二、阻塞队列**

![blockingQueue](https://raw.githubusercontent.com/betteryuxuan/Image/main/blockingQueue.png)

```java
public class Cook extends Thread{
    ArrayBlockingQueue<String>queue;

    public Cook(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run(){
        while (true) {
            try {
                String food = queue.take();
                System.out.println("吃了一碗" + food);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }

        }
    }
}
```

```java
public class Foodie extends Thread{
    ArrayBlockingQueue<String> queue;

    public Foodie(ArrayBlockingQueue<String> queue) {
        this.queue = queue;
    }

    @Override
    public void run() {
        while (true) {
            try {
                queue.put("面条");
                System.out.println("放了一碗面条");
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

```java
public class ThreadDemo {
    public static void main(String[] args) {
        ArrayBlockingQueue<String> queue = new ArrayBlockingQueue<>(1);

        Cook c = new Cook(queue);
        Foodie f = new Foodie(queue);

        c.start();
        f.start();
    }
}
```

## 六大状态

![Snipaste_2024-06-10_09-37-47](https://raw.githubusercontent.com/betteryuxuan/Image/main/Snipaste_2024-06-10_09-37-47.png)

![7status](https://raw.githubusercontent.com/betteryuxuan/Image/main/7status.png)

| 状态         | 描述                                       | 相关方法         |
| ------------ | ------------------------------------------ | ---------------- |
| 新建状态     | 线程对象已创建但尚未启动                   | 创建线程对象     |
| 就绪状态     | 线程已经被创建且处于等待CPU调度执行的状态  | start方法        |
| 阻塞状态     | 线程被阻塞，等待获取一个监视器锁（同步锁） | 无法获得锁对象   |
| 等待状态     | 线程无限期地等待另一个线程的通知           | wait方法         |
| 计时等待状态 | 线程在指定时间内等待另一个线程的通知       | sleep方法·       |
| 结束状态     | 线程执行完毕，终止运行                     | 全部代码执行完毕 |

## 线程池

**一、核心原理**

1. 创建一个池子，池子中是空的
2. 提交任务时，池子会创建新的线程对象，任务执行完毕，线程归还给池子，下回再次提交任务时，不需要创建新的线程，直接复用已有的线程即可
3. 但是如果提交任务时，池子中没有空闲线程，也无法创建新的线程，任务就会排队等待

**二、代码实现**

1. 创建线程池
2. 提交任务
3. 所有的任务全部执行完毕，关闭线程池

**三、方法**

`Executors`：线程池的工具类，通过调用方法返回不同类型的线程池对象

| 方法名称                                                     | 说明                     |
| ------------------------------------------------------------ | ------------------------ |
| `public static ExecutorService newCachedThreadPool()`        | 创建一个没有上限的线程池 |
| `public static ExecutorService newFixedThreadPool(int nThreads)` | 创建有上限的线程池       |

**四、示例**

```java
public class MyThreadPoolDemo {
    public static void main(String[] args) throws InterruptedException {
        //1.创建线程池对象
        ExecutorService pool1 = Executors.newCachedThreadPool();
	   //2.提交任务
        pool1.submit(new MyRunnable());
	    //3.销毁线程池
        pool1.shutdown();
    }
}
```

## 自定义线程池

**一、任务拒绝策略**

|               **任务拒绝策略**               |                           **说明**                           |
| :------------------------------------------: | :----------------------------------------------------------: |
|     **`ThreadPoolExecutor.AbortPolicy`**     | **默认策略**：丢弃任务并抛出`RejectedExecutionException`异常 |
|    **`ThreadPoolExecutor.DiscardPolicy`**    |          丢弃任务，但是不抛出异常 这是不推荐的做法           |
| **`ThreadPoolExecutor.DiscardOldestPolicy`** |      抛弃队列中等待最久的任务 然后把当前任务加入队列中       |
|  **`ThreadPoolExecutor.CallerRunsPolicy`**   |           调用任务的`run()`方法绕过线程池直接执行            |

**二、ThreadPoolExecutor参数说明**

`ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor()`
(核心线程数量，最大线程数量，空闲线程最大存活时间，任务队列，创建线程工厂，任务的拒绝策略

| 参数            | 说明                 | 要求                                |
| --------------- | -------------------- | ----------------------------------- |
| corePoolSize    | 核心线程数量         | 不能小于0                           |
| maximumPoolSize | 最大线程数量         | 不能小于0，最大数量 >= 核心线程数量 |
| keepAliveTime   | 空闲线程最大存活时间 | 不能小于0                           |
| unit            | 时间单位             | 用`TimeUnit`指定                    |
| workQueue       | 任务队列             | 不能为`null`                        |
| threadFactory   | 创建线程工厂         | 不能为`null`                        |
| handler         | 任务的拒绝策略       | 不能为`null`                        |

示例：

```java
ThreadPoolExecutor pool1 = new ThreadPoolExecutor(
    corePoolSize: 3,          // 核心线程数量，不能小于0
    maximumPoolSize: 6,       // 最大线程数量，不能小于0，最大数量 >= 核心线程数量
    keepAliveTime: 60,        // 空闲线程最大存活时间，不能小于0
    TimeUnit.SECONDS,         // 时间单位
    new ArrayBlockingQueue<>(capacity: 3), // 任务队列
    Executors.defaultThreadFactory(),      // 创建线程工厂
    new ThreadPoolExecutor.AbortPolicy()   // 任务的拒绝策略（内部类）
);
```

**三、线程池大小**

![threadpool](https://raw.githubusercontent.com/betteryuxuan/Image/main/threadpool.png)

对于 I/O 密集型任务，由于任务的主要时间花在等待 I/O 上，而不是 CPU 计算上，通常需要更多的线程来保持 CPU 的高利用率

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
        //实例化
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

1. 获取任意一个类中的所有信息
2. 结合配置文件动态创建对象

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

