# 一、Properties

**定义：**properties是一个双列集合集合，拥有Map集合所有的特点（一般不会当集合使用）。

**重点：**有一些特有的方法，可以把集合中的数据，按照键值对的形式写到配置文件当中也可以把配置文件中的数据，读取到集合中来。

**核心作用：**Properties是用来代表属性文件的，通过Properties可以读写属性文件里的内容

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240611215508482.png" alt="image-20240611215508482"  />

## 1.1 构造方法

| **构造器**            | 说明                                 |
| --------------------- | ------------------------------------ |
| `public Properties()` | 用于构建Properties集合对象（空容器） |

## 1.2 从Properties文件中获取

| **常用方法**                               | 说明                                         |
| ------------------------------------------ | -------------------------------------------- |
| `public void load(InputStream is)`         | 通过字节输入流，读取属性文件里的键值对数据   |
| `public void load(Reader reader)`          | 通过字符输入流，读取属性文件里的键值对数据   |
| `public String getProperty(String key)`    | 根据键获取值（其实就是get方法的效果）        |
| `public Set<String> stringPropertyNames()` | 获取全部键的集合（其实就是keySet方法的效果） |

```java
public class PropertiesTest {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();

        properties.load(new FileReader("..\\properties\\a.properties"));

        for (String s : properties.stringPropertyNames()) {
            System.out.println(s + " = " + properties.getProperty(s));
        }
    }
}
```
## 1.3  向Properties文件中存储


| 方法声明                                              | 说明                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| `public Object setProperty(String key, String value)` | 保存键值对数据到`Properties`对象中。如果键已经存在，会用新值替换旧值，并返回旧值；如果键不存在，返回`null`。 |
| `public void store(OutputStream os, String comments)` | 把键值对数据，通过字节输出流写出到属性文件里去               |
| `public void store(Writer w, String comments)`        | 把键值对数据，通过字符输出流写出到属性文件里去。`comments`参数用于在文件头部添加注释。 |

```java
public class PropertiesTest02 {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();

        properties.setProperty("abc","123");
        properties.setProperty("def","456");

        properties.store(new FileWriter("..\\properties\\a.properties"),"注释");
    }
}
```

# 二、xml

## 2.1 XML

XML（全称EXtensible Markup Language，可扩展标记语言）

本质是一种数据格式，可以用来存储复杂的数据结构和数据关系。

## 2.2 特点

- XML中的`<标签名>`称为一个标签或一个元素，一般是成对出现的。
- XML中的标签名可以自己定义(可扩展)，但必须要正确的嵌套。
- XML中只能有一个根标签。
- XML中的标签可以有属性。

如果一个文件中放置的是XML格式的数据，这个文件就是XML文件，文件后缀一般为`.xml`。

## 2.3 规则

**1. 大小写敏感**

在XML文档中，大小写是有区别的。例如，“A”和“a”是不同的标记。编写XML元素时，前后标记的大小写必须保持一致。最好养成一致的习惯，比如：

- 全部小写
- 全部大写
- 大写第一个字母

这样可以减少因为大小写不匹配而产生的文档错误。

**2. 只有一个根元素**

良好格式的XML文档必须有一个根元素。

根元素是紧接着声明后面的第一个元素，其他元素都是这个根元素的子元素。

根元素完全包括文档中的所有其他元素。

**3. 属性值使用引号**

所有属性值必须加引号（可以是单引号，也可以是双引号，建议使用双引号），否则将被视为错误。

**4. 所有的标记必须有相应的结束标记**

所有标记必须成对出现。有一个开始标记，就必须有一个结束标记。

**5. 所有的空标记也必须被关闭**

在XML中，所有的空标记也必须关闭。可以使用自闭合标记，如：

```xml
<emptyElement />
```

**示例XML文档**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rootElement>
    <childElement attribute="value">Content</childElement>
    <emptyElement />
</rootElement>
```

## 2.3 抬头声明

在XML（可扩展标记语言）中，抬头声明通常指的是XML声明。这个声明位于XML文档的最开始，用于指定该文档是XML文档，以及它使用的XML版本和字符编码。

```xml
<?xml version="1.0" encoding="UTF-8"?>
```

在这个例子中：

- `<?xml` 是声明开始的标记。
- `version="1.0"` 指定了XML的版本，目前最常用的是1.0版本。
- `encoding="UTF-8"` 指定了文档使用的字符编码，这里使用的是UTF-8编码。UTF-8是一种通用的、兼容多种语言的字符编码。
- `?>` 是声明结束的标记。

## 2.4 特殊字符

- `&lt;` 替代 `<` （小于号）
- `&gt;` 替代 `>` （大于号）
- `&amp;` 替代 `&` （和号）
- `&apos;` 替代 `'` （单引号）
- `&quot;` 替代 `"` （双引号）

## 2.5 **CDATA区段**

在XML文档中，所有文本都会被解析器解析。解析器会将XML中的标签和文本内容解析成文档对象模型（DOM）或其他数据结构。

然而，有时需要在XML文档中包含一些特殊字符（如`<`和`&`），这些字符在正常情况下会被解析器认为是标签或实体引用。为了在XML文档中<u>插入这些特殊字符而不被解析器解析</u>，我们可以使用CDATA区段。CDATA区段中的文本内容会被解析器忽略，直接按原样处理。

CDATA区段允许您在XML文档中插入一段不会被解析器解析的文本。在CDATA区段中，您可以直接包含通常会被视为特殊字符的字符（如`<`和`&`），而无需使用它们的实体引用。CDATA区段的格式如下：

```xml
<![CDATA[
    ... 在这里可以包含任何字符，包括 < 和 & ...
]]>
```

**示例：**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<message>
    <header>
        <sender>user1</sender>
        <receiver>user2</receiver>
        <timestamp>2024-06-13T12:34:56</timestamp>
    </header>
    <body>
        <![CDATA[
            Hello, how are you? Here is some special content: <tag> & special characters!
        ]]>
    </body>
</message>
```

## 2.4 作用和应用场景

- **配置文件**：经常用作系统的配置文件。
- **数据传输**：作为一种特殊的数据结构，在网络中进行传输。

**示例：**

1. 配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<config>
    <database>
        <host>localhost</host>
        <port>3306</port>
        <username>root</username>
        <password>rootpass</password>
    </database>
</config>
```

2. 数据传输示例

```xml
<?xml version="1.0" encoding="UTF-8"?>
<message>
    <header>
        <sender>user1</sender>
        <receiver>user2</receiver>
        <timestamp>2024-06-13T12:34:56</timestamp>
    </header>
    <body>
        <content>Hello, how are you?</content>
    </body>
</message>
```

# 三、区别

* **格式差异**：TXT是纯文本格式，只包含文本信息；XML是标记语言格式，用于定义文档结构；Properties是键值对格式，用于存储配置信息。
* **用途不同**：TXT主要用于存储纯文本信息；XML用于数据交换、配置管理、Web服务等；Properties主要用于存储配置信息，如数据库连接、系统设置等。
* **扩展性和可读性**：XML具有出色的扩展性和可读性，可以自定义标记以适应不同需求；TXT和Properties在扩展性和可读性方面相对较弱，但因其简单性而易于理解和使用。
* **平台兼容性**：TXT、XML和Properties文件都具有较好的平台兼容性，可在多种操作系统和平台上使用。

---

如有错误烦请指正

感谢您的阅读
