 

# JAVA第七周学习笔记

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

![IOstream](https://raw.githubusercontent.com/betteryuxuan/Image/main/IOstream.png)

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
> 国际组织制定，容纳世界上所有文字，符号的字符集
>
> Unicode Transfer Format

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
   >   			     如果文件中也没有数据了，返回-1。
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

## 字节流字符流使用场景

字节流：拷贝任意类型的文件

字符流：读取纯文本文件中的数据，往纯文本文件中写出数据
