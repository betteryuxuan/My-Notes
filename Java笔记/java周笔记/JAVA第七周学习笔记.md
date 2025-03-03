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

### ObjectOutputStream（序列化）

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

### ObjectInputSrteam（反序列化）

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

### ZipInputStream（解压缩）

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

### ZipOutputStream（压缩）

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

Executors：线程池的工具类通过调用方法返回不同类型的线程池对象

| 方法名称                                                     | 说明                     |
| ------------------------------------------------------------ | ------------------------ |
| `public static ExecutorService newCachedThreadPool()`        | 创建一个没有上限的线程池 |
| `public static ExecutorService newFixedThreadPool(int nThreads)` | 创建有上限的线程池       |

**四、示例**

```java
public class MyThreadPoolDemo {
    public static void main(String[] args) throws InterruptedException {
        ExecutorService pool1= Executors.newCachedThreadPool();

        pool1.submit(new MyRunnable());


        pool1.shutdown();

    }
}
```

