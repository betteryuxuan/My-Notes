#  一、数据类型

## 1.1 变量声明与类型推导

- **变量声明**
   使用 `val` 声明不可变变量（相当于常量）；使用 `var` 声明可变变量。

  ```kotlin
  val a = 10          // 类型自动推断为 Int，不可变
  var b: Double = 5.0 // 显示声明为 Double，可变变量
  ```

- **类型推导**
   Kotlin 编译器会根据初始值自动推导出变量的类型，无需手动指定

## 1.2 数值类型

Kotlin 提供了多种数值类型，主要分为整数类型和浮点数类型：

- **整数类型**

  - `Byte`：8 位，范围 -128 到 127
  - `Short`：16 位，范围 -32768 到 32767
  - `Int`：32 位，默认类型（范围约为 -21 亿到 21 亿）
  - `Long`：64 位，声明时需要在数值后加 `L`

  ```kotlin
  val num: Int = 100
  val bigNum = 8000000000L  // 需要使用 L 后缀
  ```

- **浮点数类型**

  - `Float`：32 位，声明时尾部加 `f` 或 `F`
  - `Double`：64 位，默认浮点数类型

  ```kotlin
  val pi = 3.14            // 自动推导为 Double
  val floatPi = 3.14f      // 明确为 Float 类型
  ```

## 1.3 字符类型与布尔类型

- **字符类型**
   使用 `Char` 表示单个字符，常量用单引号括起来：

  ```kotlin
  val letter: Char = 'A'
  ```

- **布尔类型**
   使用 `Boolean` 类型，只有 `true` 和 `false` 两个值：

  ```kotlin
  val isVisible: Boolean = true
  ```

## 1.4 字符串

- **定义字符串**
   用双引号 `"` 括起，可以直接嵌入变量或表达式（字符串模板）：

  ```kotlin
  val name = "Kotlin"
  val greeting = "Hello, $name!"
  ```

- **多行字符串**
   使用三个引号 `"""` 定义原始字符串，可以保留格式：

  ```kotlin
  val text = """
      这是一个多行字符串
      可以保留原始格式
  """.trimIndent()
  ```

* **String 模板**

  Kotlin 提供了字符串模板语法，允许在字符串中嵌入变量和复杂表达式，语法如下：

- 1. 变量插值

  在字符串中直接使用 

  ```
  $变量名
  ```

  ```kotlin
  val name = "Kotlin"
  println("Hello, $name!")
  ```

- 2. 表达式插值

  若需要插入的内容是复杂表达式，需要用花括号 `{}`包裹，如 

  ```
  ${表达式}
  ```

  ```kotlin
  val length = name.length
  println("字符串 \"$name\" 的长度是 ${name.length}")
  ```

## 1.5.数组

### 数组创建

- 创建数组使用 

  ```
  arrayOf()
  ```

  对于基本数值类型，可以使用专用函数如 

  ```
  intArrayOf()
  ```

  ```kotlin
  val intArray = arrayOf(1, 2, 3)
  val numbers = intArrayOf(1, 2, 3, 4)
  ```

### 数组访问

访问元素：通过索引访问数组元素。

```kotlin
val arr = arrayOf(1, 2, 3)
println(arr[0]) // 输出 1
```

修改元素：通过索引修改数组元素。

```kotlin
arr[0] = 10
println(arr[0]) // 输出 10
```

### 数组遍历

* **for**

```kotlin
for (i in arr) {
    println(i)
}

for (i in 0 until 3) { // 0, 1, 2
    println(i)
}

for (i in 0..2) { // 0, 1, 2
    println(i)
}

for(i in 3 downTo 1) { // 3, 2, 1
    println(i)
}
```

- **forEach**

  使用高阶函数遍历数组。

  ```kotlin
  arr.forEach { println(it) }
  ```

- **nums.indices**
```kotlin
fun twoSum(nums: IntArray, target: Int): IntArray {
    for (i in nums.indices) {
        for (j in i + 1..<nums.size) {
            if (nums[i] + nums[j] == target) {
                return intArrayOf(i, j)
            }
        }
    }
}
```

### 常用操作

**size**

```kotlin
println(arr.size) // 输出 3
```

**contains**：检查数组是否包含某个元素。

```kotlin
println(arr.contains(2)) // 输出 true
```

**slice**：获取数组的子集。

```kotlin
val subArr = arr.slice(1..2)
println(subArr) // 输出 [2, 3]
```

### 排序与搜索

**sort**：对数组进行排序。

```kotlin
val sortedArr = arr.sorted()
println(sortedArr) // 输出 [1, 2, 3]
```

**binarySearch**：在已排序的数组中进行二分查找。

```kotlin
val index = sortedArr.binarySearch(2)
println(index) // 输出 1
```

### 数组与集合的转换

**toList**：将数组转换为列表。

```kotlin
val list = arr.toList()
println(list) // 输出 [1, 2, 3]
```

**toSet**：将数组转换为集合。

```kotlin
val set = arr.toSet()
println(set) // 输出 [1, 2, 3]
```

## 1.6 类型转换

- **显式转换**
   Kotlin 不支持隐式类型转换，必须调用转换函数进行转换：

  ```kotlin
  val intVal = 10
  val longVal = intVal.toLong()  // 转换为 Long 类型
  ```

- **常用转换函数**
   包括 `toByte()`、`toShort()`、`toInt()`、`toLong()`、`toFloat()`、`toDouble()`、`toChar()` 等。

## 1.7 类型判断与智能转换

- **类型检查**
   使用 `is` 操作符判断对象类型：

  ```kotlin
  fun demo(x: Any) {
      if (x is String) {
          println(x.length) // 编译器自动将 x 当作 String 类型
      }
  }
  ```

- **强制类型转换**
   使用 `as` 操作符进行强制转换，但转换失败会抛出异常。为避免异常，可使用安全转换操作符 `as?`，转换失败返回 `null`。*

## 1.8 可空类型与空安全

- **可空类型声明**
   在类型后加上 `?` 表示该变量可以为 `null`：

  ```kotlin
  var name: String? = "Kotlin"
  name = null  // 合法
  ```

- **安全调用运算符**
   使用 `?.` 来安全调用方法或属性，避免空指针异常：

  ```kotlin
  val length = name?.length  // 若 name 为 null，结果为 null
  ```

- **非空断言**
   使用 `!!` 表示你确定该值不为 null，但若实际为 null 则会抛出异常：

  ```kotlin
  println(name!!.length)
  ```

- **Elvis 运算符**
   使用 `?:` 提供默认值：

  ```kotlin
  val len = name?.length ?: 0  // 若 name 为 null，则 len 为 0
  ```

## 1.9 特殊类型

- **Any 类型**
   Kotlin 中所有非空类型的基类，相当于 Java 的 Object 类型。
- **Unit 类型**
   表示函数没有有意义的返回值，类似 Java 中的 void，但它是一个真正的类型，其唯一值为 `Unit`。
- **Nothing 类型**
   表示永远不会返回结果的类型，常用于函数总是抛出异常的情况。

# 二、函数

## 2.1 基本函数定义

- 定义方式

   定义函数，格式如下：

  ```kotlin
  fun 函数名(参数1: 类型, 参数2: 类型, …): 返回类型 {
      // 函数体
      return 返回值
  }
  ```

  如果函数体只有一行表达式，可以省略大括号、return 以及返回类型（编译器可自动推断）：

  ```kotlin
  fun add(a: Int, b: Int) = a + b
  ```

## 2.2 参数相关

### 默认参数与具名参数

- 默认参数

  Kotlin 允许为函数参数<u>设定默认值</u>，从而在调用时可以省略对应的参数：

  ```kotlin
  fun greet(name: String = "Kotlin"): String {
      return "Hello, $name!"
  }
  // 调用
  println(greet())            // 输出 "Hello, Kotlin!"
  println(greet(name = "张三")) // 使用具名参数
  ```

* 具名参数

  ```kotlin
  fun greet(name: String, age: Int) {
      println("$name is $age years old.")
  }
  
  // 调用
  greet(name = "Alice", age = 30) // 常规顺序
  greet(age = 30, name = "Alice") // 打乱顺序
  ```

  **所有位置参数必须位于第一个具名参数之前**

  ```kotlin
  fun example(a: Int, b: Int, c: Int) { }
  
  example(1, c = 3, b = 2) // 合法
  // example(a = 1, 2, 3)   // 编译错误：位置参数在具名参数后
  ```


### 可变参数 (vararg)

- 可变参数

  使用 

  ```
  vararg
  ```

   修饰参数，表示传入的参数个数可变，内部作为数组处理：

  ```kotlin
  fun <T> asList(vararg items: T): List<T> {
      val result = ArrayList<T>()
      for (item in items) {
          result.add(item)
      }
      return result
  }
  // 调用
  val list = asList(1, 2, 3, 4)
  ```

## 2.3 单表达式函数

当函数只有一行表达式时，可使用单表达式函数写法：

```kotlin
fun square(x: Int) = x * x
```

返回值类型可由编译器推断

## 2.4 局部函数

Kotlin 允许在一个函数内部定义其他函数，这叫做局部函数（nested function），它可以访问外部函数的局部变量：

```kotlin
fun performOperation(numbers: List<Int>): Int {
    var sum = 0
    fun add(value: Int) {
        sum += value
    }
    for (num in numbers) {
        add(num)
    }
    return sum
}
```

## 2.5. Lambda 表达式

- 使用大括号包裹参数和函数体，参数与函数体之间用箭头 

  ```
  ->
  ```

   分隔：

  ```kotlin
  val multiply = { x: Int, y: Int -> x * y }
  println(multiply(3, 4)) // 输出 12
  ```

- 隐式参数 it

  如果 lambda 只有一个参数，则可以使用隐式参数 

  ```
  it
  ```

  ```kotlin
  val printMessage: (String) -> Unit = { println(it) }
  printMessage("Hello Lambda")
  ```

## 2.6 匿名函数

> **没有名称的函数**，可直接赋值给变量、作为参数传递或从其他函数返回。

1. 即时性：无需预先声明，即用即定义

2. 隐式返回：自动返回函数体最后一行表达式的结果（无需retuen关键字）

   ```kotlin
   val greeting = { "Hello, Kotlin!" } // 隐式返回字符串
   println(greeting()) // 输出：Hello, Kotlin!
   ```

### 参数处理

- 类型声明：匿名函数类型由参数和返回值决定，例如

  ```
  (String, Int) -> String
  ```

- 参数规则

  - 多参数需明确声明参数名和类型

    ```kotlin
    val add = { a: Int, b: Int -> a + b }
    ```

  - 单参数可使用`it`简化

    ```kotlin
    val square: (Int) -> Int = { it * it }
    println(square(5))
    ```

### 类型推断

若匿名函数赋值给变量时已明确类型，可省略参数类型声明，但需确保编译器能推断出类型

```kotlin
val multiply = { a: Int, b: Int -> a * b } // 显式声明参数类型
val multiplyInferred: (Int, Int) -> Int = { a, b -> a * b } // 类型推断
```

### 显式返回类型

与 `Lambda` 不同，匿名函数支持**显式声明返回类型**，适用于需要明确返回值的场景

```kotlin
val divide = fun(a: Int, b: Int): Double {
    if (b == 0) return 0.0 // 显式返回
    return a.toDouble() / b
}
```

## 2.7 高阶函数

高阶函数是将函数作为参数或返回值的函数，例如：

```kotlin
// 函数作为参数
fun operate(a: Int, b: Int, op: (Int, Int) -> Int): Int {
    return op(a, b)
}

// 匿名函数作为最后一个参数，省略了最外层括号
val sum = operate(4, 5) { x, y -> x + y }
println(sum) // 输出 9

```

```kotlin
// 函数作为返回值
fun makeMultiplier(factor: Int): (Int) -> Int {
    return { number -> number * factor }
}
// timesThree是返回的函数
val timesThree = makeMultiplier(3)
// 调用返回的函数
println(timesThree(10)) // 输出 30
```



## 2.8 内联函数

- 类似于C语言宏定义的替换

  ```
  inline
  ```

   修饰高阶函数，可以在编译时将函数体内联，从而减少函数调用开销。

  ```kotlin
  inline fun runOperation(operation: () -> Unit) {
      operation()
  }
  ```

## 2.9 函数类型与函数引用

- 函数类型

  Kotlin 中函数本身是“一等公民”，其类型写作 

  ```
  (参数类型1, 参数类型2, …) -> 返回类型
  ```

  ```kotlin
  val func: (Int, Int) -> Int = ::add  // 使用函数引用引用已有函数
  println(func(3, 7))
  ```

- 函数引用

  ```
  ::
  ```

   符号引用函数：

  ```kotlin
  fun add(a: Int, b: Int) = a + b
  val addRef = ::add
  println(addRef(5, 10)) // 输出 15
  ```

## 2.10 闭包

> 闭包是能够读取其他函数内部变量的函数，即使外部函数已执行完毕，闭包仍能访问其作用域中的变量

```java
// 匿名函数持有外部函数 doRepeat 的 count 变量，形成闭包
fun doRepeat(): () -> Unit {
    var count = 1
    return fun() { println(count++) }
}
```

**核心特性**：

1. **状态持久化**
   闭包会延长外部函数变量的生命周期，使其常驻内存。例如，多次调用 `doRepeat()` 生成的闭包会独立维护各自的 `count` 变量

   ```kotlin
   val doRepeat1 = doRepeat()  // count 初始化为1
   doRepeat1()  // 输出1 → count变为2
   doRepeat1()  // 输出2 → count变为3
   ```

2. **独立性**
   不同闭包实例持有独立的状态。例如，`doRepeat1` 和 `doRepeat2` 的 `count` 互不影响

   ```kotlin
   val doRepeat1 = doRepeat()
   val doRepeat2 = doRepeat()
   doRepeat1()  // 输出1
   doRepeat2()  // 输出1（独立计数）
   ```

3. **简化代码与避免全局污染**
   闭包可替代全局变量，例如实现斐波那契数列的状态缓存

   ```kotlin
   fun fib(): () -> Long {
       var prev = 0L
       var current = 1L
       return fun(): Long {
           val result = current
           current += prev
           prev = result
           return result
       }
   }
   ```

## 2.11 反引号中的函数名

Kotlin可以使用空格和特殊字符对函数命名，不过函数名要用一对反引号括起来。

1. 当 Java 方法名与 Kotlin 的保留字（如 `is`、`in`、`object`）同名时，Kotlin 需用反引号转义调用。

> 例如，Java 方法 `is()` 在 Kotlin 中需写为
>
> ``` foo.is(bar)
> foo.`is`(bar)
> ```

2. 可用于测试用例等场景，例如：

```kotlin
fun `test user login with invalid password`() { ... }
```

# 三、null安全与异常

## 3.1 null安全机制

### 非空类型与可空类型

- **非空类型**：在Kotlin中，默认变量不允许为null

- 声明时必须初始化且不能赋值为`null`，编译器会强制确保变量始终持有有效值

  ```kotlin
  val name: String = "Kotlin"
  ```
  
- **可空类型**：如果允许变量为null，必须在类型后加问号（?）

- 直接调用其方法或属性会触发编译错误，需通过安全操作符处理

  ```kotlin
  val name: String? = null
  ```

### ?. ?: 操作符和let 

- 安全调用操作符 `?.`

  当对象可能为null时，使用

  ```
  ?.
  ```

  访问其属性或方法可以避免NullPointerException：

  ```kotlin
  val length = name?.length  // 如果name为null，length将为null
  ```

- 空合并操作符 `?:`

  用于在对象为null时提供默认值：

  ```kotlin
  val length = name?.length ?: 0  // 如果name为null，则返回0
  ```

* let函数

  * 在 `let` 的函数体内，调用者对象使用 `it` 作为默认名称
  * `let` 的返回值是函数块的最后一行或指定的返回值

  ```java
  val name: String? = "Kotlin"
  name?.let {
      println("Name is $it")
  }
  // 如果 name 为 null，则不会执行 let 块内的代码
  ```

  ```kotlin
  val length = "yuxuan".let {
      it.length
  }
  println(length)
  ```

## 3.2 异常处理机制

### try-catch

**try-catch-finally结构**

- 与Java类似

  ```
  try-catch-finally
  ```

  ```kotlin
  try {
      // 可能抛出异常的代码
  } catch (e: Exception) {
      // 异常处理
  } finally {
      // 无论是否异常，都会执行的代码
  }
  ```

```java
var num: Int? = null    // 声明一个可为空的Int类型变量
try {
    // 使用非空断言运算符(!!)强制解包可能为null的变量
    println(num!!.plus(11)) 
} catch (e: Exception) {
    println(e)
}
```

### !!

- 非空断言操作符 `!!`

  强制将一个可空类型转为非空类型，如果对象为null，会抛出NullPointerException：

  ```kotlin
  val length = name!!.length  // 若name为null，则会抛出异常
  ```

### 先决条件函数

| 函数           | 描述                                                         |
| -------------- | ------------------------------------------------------------ |
| checkNotNull   | 如果参数为null，则抛出IllegalStateException异常，否则返回非null值 |
| require        | 如果参数为false，则抛出IllegalArgumentException异常          |
| requireNotNull | 如果参数为null，则抛出IllegalStateException异常，否则返回非null值 |
| error          | 如果参数为null，则抛出IllegalStateException异常并输出错误信息，否则返回非null值 |
| assert         | 如果参数为false，则抛出AssertionError异常，并打上断言编译器标记 |

**require()函数**

- **作用**：用于检查函数参数的先决条件。如果条件不满足，会抛出`IllegalArgumentException`。

- 示例

  ```kotlin
  fun setAge(age: Int) {
      require(age > 0) { "年龄必须为正数" }
      // 其他逻辑
  }
  ```

**assert()函数**

- **作用**：用于调试时的断言检查。如果条件为false，则抛出`AssertionError`。

- 注意：在生产环境中，断言通常会被禁用，仅在开发或测试阶段使用。

  ```kotlin
  fun test(value: Int) {
      assert(value > 0) { "值必须大于0" }
      // 测试代码
  }
  ```

**error()函数**

- 作用：用于直接抛出异常，常用于未实现的代码块或错误状态。

  ```kotlin
  fun notImplemented() {
      error("尚未实现该功能")
  }
  ```

# 四、字符串

## 4.1 定义

- **普通字符串**：使用双引号括起来，可以包含转义字符，如换行符 `\n`。

```kotlin
  val str1 = "Hello\nWorld"
  println(str1)
  // Hello
  // World
```

- **原始字符串**：使用三重引号 `"""` 括起来，支持多行且不会转义特殊字符。

```kotlin
  val str2 = """
      第1行
      第2行
  """.trimIndent()
  println(str2)
  // 输出：
  // 第1行
  // 第2行
```

## 4.2 字符串模板

Kotlin 提供字符串模板功能，可以在字符串中嵌入**变量**或**表达式**。

```kotlin
val name = "Kotlin"
val age = 5
println("语言：$name，年龄：$age 岁")
println("2 + 3 = ${2 + 3}")
// 输出：
// 语言：Kotlin，年龄：5 岁
// 2 + 3 = 5
```

## 4.3 常用操作

- **查找**：`indexOf()` 返回子字符串的起始位置，未找到则返回 -1。

```kotlin
  val text = "Kotlin 编程"
  val index = text.indexOf("编程")
  println(index) // 输出：7
```

- **替换**：`replace()` 用于替换指定的子字符串。

```kotlin
// fun replace(oldValue: String, newValue: String, ignoreCase: Boolean = false)
val text = "Hello Kotlin"
val newText = text.replace("Kotlin", "World")
println(newText) // 输出：Hello World

// fun replace(regex: Regex, replacement: String): String
val text = "请联系我：123-456-7890 或 987-654-3210。"
val regex = Regex("\\d{3}-\\d{3}-\\d{4}")
val result = text.replace(regex, "[号码已隐藏]")
println(result)
// 请联系我：[号码已隐藏] 或 [号码已隐藏]。
```

- **分割**：`split()` 根据指定分隔符将字符串分割为List集合

```kotlin
val text = "apple,banana,cherry"
val fruits = text.split(",")
println(fruits) // 输出：[apple, banana, cherry]

// 解构语法特性
val text = "apple,banana,cherry"
val (fruit1, fruit2, fruit3) = text.split(",")
println(fruit1) // 输出：apple
println(fruit2) // 输出：banana
println(fruit3) // 输出：cherry

```

- **截取**：`substring()` 截取指定范围内的子字符串。

```kotlin
val text = "Kotlin 编程"
val subText = text.substring(0, 6)
println(subText) // 输出：Kotlin

val text = "apple,banana,cherry"
val subText = text.substring(6..11)
println(subText) // 输出：banana
```

- **遍历**：可以使用索引或直接遍历字符。

```kotlin
val text = "Kotlin"
for (char in text) {
    println(char)
}

text.forEach { print("$it ") }
```

## 4.4 空安全

提供多种方法来判断字符串的空值或空白状态：

- **isNullOrEmpty()**：为空指针或长度为 0 时返回 `true`。
- **isNullOrBlank()**：为空指针、长度为 0 或全为空格时返回 `true`。
- **isEmpty()**：长度为 0 时返回 `true`。
- **isNotEmpty()**：长度大于 0 时返回 `true`。
- **isBlank()**：长度为 0 或全为空格时返回 `true`。
- **isNotBlank()**：长度大于 0 且不全为空格时返回 `true`。

```kotlin
val str: String? = "  "
println(str.isNullOrEmpty()) // 输出：false
println(str.isNullOrBlank()) // 输出：true
```

## 4.5 字符串比较

> `==`检查两个字符串中的字符是否匹配
>
> `===`检查两个变量是否指向内存堆上同一对象

**1. 使用 `==` 运算符进行内容比较**

在Kotlin中，`==`运算符用于比较两个对象的内容是否相等。当用于字符串时，它会比较字符串的字符序列是否相同。需要注意的是，`==`在Kotlin中被重载，实际调用的是`equals()`方法。

示例：

```kotlin
val str1 = "Kotlin"
val str2 = "Kotlin"
val str3 = "kotlin"

println(str1 == str2) // 输出：true
println(str1 == str3) // 输出：false
```

**2. 使用 `===` 运算符进行引用比较**

`===`运算符用于比较两个变量是否引用同一个对象实例，即比较它们的引用是否相同。这与`==`运算符的内容比较不同

```
val str1 = "Kotlin"
val str2 = "Kotlin"
val str3 = String("Kotlin".toCharArray())

println(str1 === str2) // 输出：true，因为字符串常量池的原因
println(str1 === str3) // 输出：false，因为str3是通过构造函数创建的新的String对象
```

**3. 使用 `equals()` 方法进行比较**

`equals()`方法用于比较两个对象的内容是否相等。与`==`运算符类似，但`equals()`方法提供了更多的控制，例如可以指定是否忽略大小写。

```kotlin
val str1 = "Kotlin"
val str2 = "kotlin"

println(str1.equals(str2)) // 输出：false，默认区分大小写
println(str1.equals(str2, ignoreCase = true)) // 输出：true，忽略大小写
```

# 五、标准库函数

## 5.1 作用域函数

> 可以让你在一个代码块内访问某个对象，从而减少重复引用对象名称，并能链式调用。

| 函数  | 对象引用方式 | 返回值              | 是否扩展函数       |
| ----- | ------------ | ------------------- | ------------------ |
| let   | it           | lambda 表达式的结果 | 是                 |
| run   | this         | lambda 表达式的结果 | 是                 |
| with  | this         | lambda 表达式的结果 | 否（作为参数传入） |
| apply | this         | 调用对象本身        | 是                 |
| also  | it           | 调用对象本身        | 是                 |

### let

- **特点**：

  - 将调用对象作为参数传入 lambda（默认名称为 it）。
  - 返回 lambda 表达式最后一行的值。
  - 常用于空值检查（结合 ?. 使用）或局部变量转换。

- **示例**：

  ```kotlin
  val str: String? = "Hello"
  str?.let {
      println("字符串不为空：$it")
      // 返回最后一行表达式的结果
      it.length
  }
  ```

### run

- **特点**：

  - 有两种用法：
    1. 直接调用 run { … }，不需要对象，此时 lambda 无接收者；
    2. 作为扩展函数调用，即对象.run { … }，此时 lambda 的接收者为该对象（可直接使用 this）。
  - 返回 lambda 最后一行的值。

- **示例**：

  ```kotlin
  // 直接调用
  val result = run {
      println("直接调用 run")
      100
  }
  println(result)
  // 直接调用 run
  // 100
  
  // 对象调用 run
  val person = Person("Alice", 30)
  val info = person.run {
      "姓名：$name, 年龄：$age"
  }
  println(info)
  ```

### with

- **特点**：

  - 不是扩展函数，而是一个普通函数，需要将对象作为第一个参数传入。
  - 在 lambda 内部可以直接使用 this 调用对象方法。
  - 返回 lambda 表达式最后一行的结果。

- **示例**：

  ```kotlin
  val sb = StringBuilder("Kotlin:")
  val resultString = with(sb) {
      append("标准库函数笔记")
      toString()
  }
  println(resultString)
  ```

### apply

- **特点**：

  - 是扩展函数，其 lambda 内部的接收者为调用对象（this 可直接访问对象成员）。
  - 返回调用对象本身，因此常用于对象的初始化配置。

- **示例**：

  ```kotlin
  val person = Person("Bob", 25).apply {
      // 在这里配置 person 对象
      println("初始化 person 对象：$name, $age")
  }
  
  val file: File = File("111").apply {
      setReadable(true)
      setWritable(true)
      setExecutable(false)
  }
  ```

### also

- **特点**：

  - 与 apply 类似，都是扩展函数且返回调用对象本身；
  - 不同之处在于 lambda 中的对象通过 it 传入，而不是 this。
  - 常用于在链式调用中做额外操作（例如日志记录、调试）。

- **示例**：

  ```kotlin
  val numbers = mutableListOf(1, 2, 3)
  numbers.also {
      println("操作前的列表：$it")
  }.add(4)
  println("操作后的列表：$numbers")
  ```

## 5.2 其他常用标准库函数

### takeIf 与 takeUnless

- **takeIf**：

  - 如果给定的谓词为 true，则返回对象本身，否则返回 null。
  - 常用于将条件判断嵌入调用链中。

- **示例**：

  ```kotlin
  val number = 42
  val evenNumber = number.takeIf { it % 2 == 0 }
  println(evenNumber)  // 输出 42
  
  val oddNumber = number.takeIf { it % 2 != 0 }
  println(oddNumber)  // 输出 null
  ```

- **takeUnless**：

  - 与 takeIf 相反，条件为 false时返回对象本身，为 true时返回 null。

- **示例**：

  ```kotlin
  val strValue = "Kotlin"
  val result = strValue.takeUnless { it.isEmpty() }
  println(result)  // 输出 "Kotlin"
  ```

### repeat

- **特点**：

  - 用于重复执行一个操作指定次数，常见于打印、计数等场景。

- **示例**：

  ```kotlin
  repeat(5) {
      println("Hello, Kotlin!")
  }
  ```

## 5.3 应用场景与选择

- 对非空对象进行操作时，使用 `?.let { }` 能避免显式的空检查。
- 对象的初始化配置，`apply` 是首选，因为它返回对象本身且代码风格简洁。
- 在不改变对象的情况下执行额外操作（例如日志记录），可以使用 `also`。
- 如果需要对一个代码块求值并返回结果，但又不关心对象本身，可使用 `run` 或 `with`（后者适用于需要多次调用同一对象的场景）。

# 六、集合

## 6.1 集合分类

Kotlin 中的集合主要分为两大类：

- **只读集合**：提供读取操作，不能修改内部元素，如：
  - `List<T>`
  - `Set<T>`
  - `Map<K, V>`
- **可变集合**：在只读集合基础上增加写操作（添加、删除、更新），如：
  - `MutableList<T>`
  - `MutableSet<T>`
  - `MutableMap<K, V>`

## 6.2 List 集合

### 不可变 List

- 创建

  ```
  listOf()
  ```

   函数创建不可变 List，例如：

  ```kotlin
  val list = listOf(1, 3, 5, 7)
  ```

- 访问元素

  - 索引访问：`list[0]` 或 `list.get(0)`
  - 获取首尾：`list.first()`、`list.last()`
  - 安全访问：`list.elementAtOrNull(index)`（越界时返回 `null`）

- 取子集

  ```
  subList(fromIndex, toIndex)
  ```

  （左闭右开区间）

  ```kotlin
  println(list.subList(0, 2))  // 输出：[1, 3]
  ```

### 可变 List

- 创建

  ```
  mutableListOf()
  ```

  ```kotlin
  val mList = mutableListOf("one", "two", "three")
  ```

- 常用操作

  - 添加：`mList.add("four")`
  - 删除：`mList.removeAt(0)` 或 `mList.remove("two")`
  - 更新：`mList[0] = "newOne"`
  - 随机打乱：`mList.shuffle()`
  - 过滤删除：`mList.removeAll { it.length == 3 }`

### 遍历

1. **直接 for 循环遍历元素**
   适用于只需要处理集合中每个元素的情况：

```kotlin
val list = listOf("a", "b", "c")
for (item in list) {
    println(item)
}
```

2. **通过索引遍历**
   使用 `indices` 获取索引范围，再通过索引访问对应的值：

```kotlin
for (i in list.indices) {
    println("index = $i, value = ${list[i]}")
}
```

3. **使用 withIndex() 进行遍历**
   这种方式既能同时获得元素的索引和值，又让代码看起来非常清晰：

```kotlin
for ((index, value) in list.withIndex()) {
    println("index = $index, value = $value")
}
```

4. **使用高阶函数 forEach()**

```kotlin
list.forEach { item ->
    println(item)
}
```

5. **使用函数 forEachIndexed** 

   允许在遍历集合的同时获取每个元素的索引和值

```kotlin
val fruits = listOf("Apple", "Banana", "Cherry")
fruits.forEachIndexed { index, fruit ->
    println("索引：$index，元素：$fruit")
}
```

## 6.3 Set 集合

### 基本特点

- **唯一性**：Set 中的每个元素都是唯一的。
- 创建
  - 不可变 Set：`setOf("one", "two", "three")`
  - 可变 Set：`mutableSetOf("one", "two", "three")`
- **内部实现**：通常默认采用 `LinkedHashSet`，可保持插入顺序。

### 常用操作

- 遍历：使用 `forEach` 或 `forEachIndexed`

- 集合运算：

  - 并集：`set1 union set2`
  - 交集：`set1 intersect set2`
  - 差集：`set1 subtract set2`

- **list 转 set**：

  ```java
  val list = listOf("apple", "banana", "apple", "orange")
  // 可变或不可变
  val set = list.toSet()
  val mutableSet = list.toMutableSet()
  ```

  ```java
  val list = listOf("apple", "banana", "apple", "orange")
  val distinctList = list.distinct()
  println(distinctList)  // [apple, banana, orange]
  ```

  ```java
  data class Person(val name: String, val age: Int)
  
  val people = listOf(
      Person("Alice", 20),
      Person("Bob", 25),
      Person("Alice", 30)
  )
  
  // 根据 name 去重，保留第一个出现的
  val distinctPeople = people.distinctBy { it.name }
  println(distinctPeople)
  // 输出：[Person(name=Alice, age=20), Person(name=Bob, age=25)]
  ```

## 6.4 Map 集合

### 基本特点

- **存储键值对**：键（Key）必须唯一，值（Value）可以重复。
- 创建
  - 不可变 Map：`mapOf("key1" to 1, "key2" to 2, "key3" to 3)`
  - 可变 Map：`mutableMapOf("key1" to 1, "key2" to 2)`

### 常用操作

- 访问键和值：`map.keys`、`map.values`、`map.entries`

  `getOrPut` 函数用于在映射中获取指定键的值，如果键不存在，则使用提供的默认值（通过 lambda 表达式计算）插入该键，并返回该值。

- 通过键取值：`map["key1"]`

- 过滤操作：

  - `filter { (key, value) -> … }`
  - `filterKeys { … }`
  - `filterValues { … }`

- Map 转换：利用 `associateWith {}`、`associateBy {}` 或 `associate {}` 将 List 转换成 Map

### 遍历

1. **使用 for 循环**

```kotlin
val map = mapOf("apple" to "苹果", "banana" to "香蕉")
for ((key, value) in map) {
    println("key = $key, value = $value")
}
```

2. **使用 forEach 函数**

```kotlin
val map = mapOf("apple" to "苹果", "banana" to "香蕉")
map.forEach { (key, value) ->
    println("key = $key, value = $value")
}
```

3. **遍历 Map 的 keys 或 values**

   键或值可以分别遍历：

```kotlin
// 遍历所有键
for (key in map.keys) {
    println("key = $key")
}

// 遍历所有值
for (value in map.values) {
    println("value = $value")
}
```

## 6.5 排序与聚合

- 排序
  - `sorted()`、`sortedDescending()`：自然顺序升序/降序排序。
  - `sortedBy { }`、`sortedWith()`：根据指定规则排序。
  - `reversed()`、`asReversed()`、`shuffled()`：倒序、反向视图、随机排列。
- 聚合
  - 数量统计：`count()`
  - 最大最小：`maxOrNull()`、`minOrNull()`
  - 平均、求和：`average()`、`sum()`
  - 累加：`reduce()`、`fold()`（以及它们的右侧版本）

# 七、定义类

## 7.1 构造函数

### 主构造函数

Kotlin 的类可以有一个主构造函数和一个或多个次构造函数。主构造函数是类头的一部分，位于类名之后：

```kotlin
class Person constructor(firstName: String) {
}
```

如果主构造函数没有任何注解或可见性修饰符，可以省略 `constructor` 关键字：

```kotlin
class Person(firstName: String) {
}
```

示例：
```java
// Kotlin中的主构造函数如果不使用var或val修饰，则这些参数仅作为构造函数的形参传入，
// 而不会自动成为类的属性。
class Person constructor(
    _name: String,    
    _age: Int,       
    _height: String  
) {
    // 定义name属性，并用传入的_name初始化
    // Kotlin 中的属性有默认的 getter 和 setter，但也可以自定义
    // 自定义getter: 每次获取name时，返回首字母大写后的字符串
    // 自定义setter: 设置name时，先去掉前后空格再赋值
    var name = _name
        get() = field.capitalize()
        set(value) {
            field = value.trim()
        }
    
    // 定义age属性，直接使用传入的_age初始化
    var age = _age
    
    // 定义height属性，直接使用传入的_height初始化
    var height = _height
    
    override fun toString(): String {
        return "Person(age=$age, height='$height', name='$name')"
    }
}

fun main() {
    // 创建一个Person对象，传入对应的参数
    var p1 = Person("yuxuan", 20, "175")
    println(p1.toString())
}
```

### 次构造函数

次构造函数使用关键字 `constructor` 来声明，与主构造函数不同的是，它需要用委托调用主构造函数（或者其他次构造函数）。

* 每个次构造函数必须通过委托调用其他构造函数来最终调用主构造函数（如果存在主构造函数），这确保了所有属性都能被正确初始化。

例如：

```kotlin
class Person(val name: String, val age: Int) {
    // 次构造函数，提供只传入 name 的构造方式
    constructor(name: String) : this(name, 0) {
        // 次构造函数体中可以进行额外的初始化操作
        println("使用次构造函数，仅传入 name 参数")
    }
}

fun main() {
    val person = Person("Bob")        // 使用次构造函数
}
```

* 次构造函数仅需要传入 `name`，然后通过 `: this(name, 0)` 委托调用主构造函数，并为 `age` 提供默认值 0。

## 7.2 初始化顺序

> **主构造函数参数求值 → 属性初始化与 init 块执行 → 次构造函数体执行**

**1. 主构造函数的参数求值**

当你创建一个对象时，首先会对传递给主构造函数的参数进行求值。

**2. 属性初始化与 init 块**

- 属性初始化接着会按它们在类中声明的顺序执行属性初始化语句。例如：
- **init 块**：随后会按顺序执行所有的 `init` 块中的代码。这两个步骤一起保证了所有属性在构造过程中被正确地初始化。

> 无论你使用主构造函数还是次构造函数，这一步总是会在构造函数体执行之前完成。

**3. 次构造函数的调用**

- 如果你是通过次构造函数来创建对象，次构造函数首先必须委托调用主构造函数（或者间接调用其他次构造函数），这样就会触发前面提到的属性初始化和 `init` 块的执行。
- 当主构造函数、属性初始化和所有 `init` 块执行完毕后，才会执行次构造函数体中的代码。

举例：

```kotlin
class Person(val name: String) {
    var age: Int = 0

    // 属性初始化
    var info: String = "初始化属性"

    // 第一个 init 块
    init {
        println("init 块1: name = $name")
    }

    // 第二个 init 块
    init {
        println("init 块2: info = $info")
    }

    // 次构造函数，委托调用主构造函数
    constructor(name: String, age: Int) : this(name) {
        this.age = age
        println("次构造函数体: age = $age")
    }
}

fun main() {
    // 调用次构造函数创建对象
    val p = Person("Alice", 25)
}
```

顺序如下：

1. **求值主构造函数参数**：`"Alice"` 被传入。
2. **属性初始化**：`age` 被初始化为 0，`info` 被初始化为 `"初始化属性"`。
3. init 块执行：按顺序打印：
   - `init 块1: name = Alice`
   - `init 块2: info = 初始化属性`
4. **次构造函数体执行**：`age` 被更新为 25，并打印 `次构造函数体: age = 25`

## 7.3 类的继承

Kotlin 中的类默认是 `final` 的，不能被继承。如果需要一个类可以被继承，需要使用 `open` 关键字修饰：

```kotlin
open class Base(p: Int)

class Derived(p: Int) : Base(p)
```

## **7.4 抽象类**

抽象类使用 `abstract` 关键字修饰，不能被实例化，可以包含抽象成员：

```kotlin
abstract class Polygon {
    abstract fun draw()
}
```

## **7.5 类的可见性修饰符**

Kotlin 提供了四个可见性修饰符：`private`、`protected`、`internal` 和 `public`。默认情况下，所有类都是 `public` 的：

- `private`：仅在同一个文件中可见。
- `protected`：同一个文件中或子类中可见。
- `internal`：同一个模块中可见。
- `public`：在任何地方可见。

 



## 7.6 延迟初始化

###  `lazy` 委托属性

- **适用场景**：通常用于 `val` 类型的属性，并且初始化操作可能较为耗时或者不一定需要立即执行。

- **特点**：

  - 延迟到第一次使用时执行初始化代码。
  - 默认线程安全（可以通过传递参数修改线程安全策略）。
  - 返回值会被缓存，下次访问时直接返回之前计算的值。

- **示例代码**：

  ```kotlin
  val lazyValue: String by lazy {
      println("初始化 lazyValue")
      "Hello, Lazy!"
  }
  
  fun main() {
      println("调用 lazyValue 前")
      println(lazyValue) // 第一次调用，执行初始化代码并打印 "初始化 lazyValue"
      println(lazyValue) // 第二次调用，直接返回缓存值，不再执行 lambda
  }
  ```

  **输出结果：**

  ```
  调用 lazyValue 前
  初始化 lazyValue
  Hello, Lazy!
  Hello, Lazy!
  ```

  这里，`lazyValue` 的初始化代码只会在第一次访问时执行一次，之后直接返回缓存的值。

### `lateinit` 修饰符

- **适用场景**：适用于 `var` 类型的属性，通常用于那些不能在声明时就初始化的情况（例如依赖于外部注入或后续赋值），但必须保证在使用前一定被初始化。

- **特点**：

  - 不能用于基本数据类型（如 `Int`、`Double` 等）。
  - 编译器不会强制在声明时初始化，但在访问时如果没有初始化会抛出 `UninitializedPropertyAccessException`。

- **示例代码**：

  ```kotlin
  class Example {
      lateinit var data: String
  
      fun initData() {
          data = "Hello, Lateinit!"
      }
  
      fun printData() {
          if (::data.isInitialized) {  // 检查是否已初始化
              println(data)
          } else {
              println("data 还未初始化")
          }
      }
  }
  
  fun main() {
      val example = Example()
      example.printData() // 输出 "data 还未初始化"
      example.initData()
      example.printData() // 输出 "Hello, Lateinit!"
  }
  ```

  在这个例子中，`data` 属性使用 `lateinit` 修饰，表示稍后会被初始化，并且在使用前可以通过 `::data.isInitialized` 检查其是否已赋值。

# 八、对象

## 8.1 `open` 关键字的用途
1. **允许类被继承** 
   Kotlin 中**类默认是 `final` 的**，需显式添加 `open` 才能被继承。  
   
   ```kotlin
   open class Animal { /* 父类 */ }
   class Dog : Animal() { /* 子类 */ }
   ```
   
2. **允许方法被重写** 
   父类方法默认不可重写，需添加 `open`，子类通过 `override` 重写。  
   
   ```kotlin
   open class Animal {
       open fun speak() { println("Animal sound") }
   }
   class Dog : Animal() {
       override fun speak() { println("Bark") }
   }
   ```
   
3. **允许属性被重写** 
   父类属性需用 `open` 修饰，子类通过 `override` 重写属性或自定义存取器。  
   
   ```kotlin
   open class Person(open val name: String)
   class Student(override val name: String) : Person(name)
   ```

## 8.2 继承中的构造函数规则
1. **主构造函数继承** 
   子类主构造函数需直接调用父类构造函数，父类参数若需被覆盖也需 `open`。  
   
   ```kotlin
   open class Person(open val name: String)
   class Student(name: String, val grade: Int) : Person(name)
   ```
   
2. **次构造函数继承** 
   若子类无主构造函数，次构造函数需通过 `super` 调用父类构造器。  
   
   ```kotlin
   class Student : Person {
       constructor(name: String) : super(name)
   }
   ```

## 8.3 `init` 代码块与初始化顺序
• **初始化顺序**：父类 `init` 代码块和属性初始化优先于子类。  
  ```kotlin
  open class Animal {
      init { println("Animal initialized") }
  }
  class Dog : Animal() {
      init { println("Dog initialized") }
  }
  // 输出顺序：Animal → Dog
  ```

---

## 8.4 接口与 `open` 的关联
• **接口方法默认开放**：接口方法无需 `open`，但实现类必须重写（除非接口提供默认实现）。  
  ```kotlin
  interface Speakable {
      fun speak()  // 必须重写
      fun eat() { println("Eating") }  // 可选重写
  }
  class Cat : Speakable {
      override fun speak() { println("Meow") }
  }
  ```

