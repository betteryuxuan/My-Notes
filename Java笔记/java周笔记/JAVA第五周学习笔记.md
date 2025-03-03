# JAVA第五周学习笔记

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

| 方法名 | 说明 |
| :----: | :--: |
|  **`public static Sting toBinaryString(int i)`**      |  得到二进制    |
|  **`public static String toOctalString(int i)`**    |  得到八进制    |
|  **`public static String toHexString(int i)`**     |  得到十六进制    |
|    **`public static int parselnt(String s)`**    |   将字符串类型的整数转成int类型的整数   |

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
   <img src="D:/OneDrive/%E6%96%87%E6%A1%A3/Tencent%20Files/1790618164/nt_qq/nt_data/Pic/2024-05/Ori/0888401fb6ad760b5cf9500b31ce367d.png" alt="img" style="zoom:50%;" />

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

#### Lambda表达式
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
LinkedHashset：<u>有序</u>、不重复、无索引
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

* 该方法定义在0bject类中，所有对象都可以调用，默认使用地址值进行计算

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

使用原则：默认使用第一种，如果第一种不能满足当前需求，就使用第二种

同时存在使用第二种

##### **方式一**

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

##### **方式二**

**比较器排序**：创建Treeset对象时候，传递比较器`Comparator`指定规则

按长度排序的字符串列表，如果长度相同，则按字典顺序排列：

```java
TreeSet<String> list = new TreeSet<>(new Comparator<String>() {
    @Override
    public int compare(String o1, String o2) {
        int i = o1.length()-o2.length();
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
              System.out.println(s + "=" + map.get(s));
          }
      }
  }
  ```

* 键值对

  导包`import java.util.Map.Entry;`或`Map.Entry`

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
                  System.out.println(s+"="+integer);
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

* TreeMap跟Treeset底层原理一样，都是红黑树结构的。

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



---

如有错误烦请指正。

感谢您的阅读

