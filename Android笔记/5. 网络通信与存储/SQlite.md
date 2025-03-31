# SQL

> SQL本质上是一种编程语言，它的学名叫作"结构化查询语言”(全称为structured QueryLanguage，简称SQL)。不过SQL语言并非通用的编程语言，它专用于数据库的访问和处理，更像是一种操作命令，所以常说SQL语句而不说SQL代码。

## 数据类型

SQLite 主要支持以下几种数据类型：

- **`INTEGER`**：用于存储整型数据，通常用于存储布尔值（`0` 表示 `false`，`1` 表示 `true`）。
- **`REAL`**：用于存储浮点数。
- **`TEXT`**：用于存储字符串数据。
- **`BLOB`**：用于存储二进制大对象。

## 创建表格

`CREATE IF NOT EXISTS 表格名称;`

```sql
CREATE TABLE IF NOT EXISTS table_name (
    column1 datatype constraints,
    column2 datatype constraints,
    ...
);
```

## 删除表格

`DROP TABLE IF EXISTS user_info`

## 修改表格

`ALTER TABLE user info ADD COLUMN phone VARCHAR;`

# 数据库管理器SQLiteDatabase

> QLiteDatabase是SQLite的数据库管理类，它提供了若干操作数据表的API

* 管理类，用于数据库层面的操作

1. `openDatabase`：打开指定路径的数据库，
2. `isOpen`：判断数据库是否已打开。
3. `close`：关闭数据库。
4. `getVersion`：获取数据库的版本号。
5. `setVersion`：设置数据库的版本号，

* 事务类，用于事务层面的操作。

1. `beginTransaction`：开事务。
2. `setTransactionSuccessful`：设置事务的成功标志。
3. `endTransaction`：结束事务。

* 数据处理类，用于数据表层面的操作。

1. `execSQL`：执行拼接好的SQL控制语句。
2. `delete`：删除符合条件的记录。
3. `update`：更新符合条件的记录。
4. `insert`：插入一条记录。
5. `query`：执行查询操作，返回结果集的游标。
6. `rawQuery`：执行拼接好的SQL查询语句，返回结果集的游标。

## 数据库的创建与删除

使用`openOrCreateDatabase`和`deleteDatabase`

![image-20240731161828251](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240731161828251.png)

```java
public class MainActivity extends AppCompatActivity implements View.OnClickListener {

    private String mDatabaseName;
    private TextView tv1;
    private Button btn1;
    private Button btn2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
       
        btn1 = findViewById(R.id.button1);
        btn2 = findViewById(R.id.button2);
        tv1 = findViewById(R.id.textView1);
        btn1.setOnClickListener(this);
        btn2.setOnClickListener(this);
        
        //生成数据库完整路径
        mDatabaseName = getFilesDir() + "/test.db";
    }

    @Override
    public void onClick(View v) {
        String desc;
        if (v.getId() == R.id.button1) {
            //创建或打开数据库，不存在就创建
            SQLiteDatabase db = openOrCreateDatabase(mDatabaseName, Context.MODE_PRIVATE, null);
            desc = String.format("数据库%s创建%s", db.getPath(), (db == null) ? "失败" : "成功");
            tv1.setText(desc);
        } else if (v.getId() == R.id.button2) {
            boolean result = deleteDatabase(mDatabaseName);
            desc = String.format("数据库%s删除%s", mDatabaseName, (result) ? "成功" : "失败");
            tv1.setText(desc);
        }
    }
}
```

# SQLiteOpenHelper

SQLiteOpenHelper类：用于创建或打开数据库的辅助类
SQLiteOpenHelper类是一个抽象类，包含两个重要的方法：

* `onCreate(SQLiteDatabase db)`：新建数据库时调用

*  `onUpgrade(SQLiteDatabase db,int oldVersion,int newVersion)`：数据库版本升级时调用

![image-20240730201510116](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240730201510116.png)

## 使用步骤

1. 新建一个继承自SQLiteOpenHelper的数据库操作类，提示重写`onCreate`和`onUpgrade`两个方法
2. 封装保证数据库安全的必要方法。
3. 提供对表记录进行增加、删除、修改、查询的操作方法。

### 新建数据库操作类

实现效果：

![Screenshot_2024-07-31-19-13-42-327_com.example.sq](https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-31-19-13-42-327_com.example.sq.jpg)

```java
public class UserDBHelper extends SQLiteOpenHelper {
    private static final String DB_NAME = "user.db";
    private static final int DB_VERSION = 1;

    public UserDBHelper(@Nullable Context context, @Nullable String name, @Nullable SQLiteDatabase.CursorFactory factory, int version) {
        super(context, DB_NAME, null, DB_VERSION);
        // 该类的构造方法由于参数太多，我们可以在`super()`中传入自己设置的固定参数
        // context: 上下文对象，用于访问应用环境
        // DB_NAME: 数据库名称
        // null: 游标工厂，可以为null，表示使用默认的游标工厂
        // DB_VERSION: 数据库版本号，用于管理数据库的版本和升级
    }

    // 创建数据库，执行建表语句
    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "";
        db.execSQL(sql);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    }
}
```

进一步补充：

```java
public class UserDBHelper extends SQLiteOpenHelper {
    private static final String DB_NAME = "user.db";
    private static final String TABLE_NAME = "user_info";
    private static final int DB_VERSION = 1;
    private static UserDBHelper mHelper = null;
    private SQLiteDatabase mRDB = null;
    private SQLiteDatabase mWDB = null;


    private UserDBHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
        // 该类的构造方法由于参数太多，我们可以在`super()`中传入自己设置的固定参数
        // context: 上下文对象，用于访问应用环境
        // DB_NAME: 数据库名称
        // null: 游标工厂，可以为null，表示使用默认的游标工厂
        // DB_VERSION: 数据库版本号，用于管理数据库的版本和升级
    }

    //利用单例模式获取数据库帮助器的唯一实例
    public static UserDBHelper getInstance(Context context) {
        if (mHelper == null) {
            mHelper = new UserDBHelper(context);
        }
        return mHelper;
    }

    // 打开数据库的读链接
    public SQLiteDatabase openReadLink() {
        if (mRDB == null || !mRDB.isOpen()) {
            mRDB = mHelper.getReadableDatabase();
        }
        return mRDB;
    }

    // 打开数据库的写链接
    public SQLiteDatabase openWriteLink() {
        if (mWDB == null || !mWDB.isOpen()) {
            mWDB = mHelper.getWritableDatabase();
        }
        return mWDB;
    }

    //关闭数据库链接
    public void closeLink() {
        if (mWDB != null && mWDB.isOpen()) {
            mWDB.close();
            mWDB = null;
        }
        if (mRDB != null && mRDB.isOpen()) {
            mRDB.close();
            mRDB = null;
        }
    }

    // 创建数据库，执行建表语句
    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "CREATE TABLE IF NOT EXISTS " + TABLE_NAME + " (" +
                "id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, " +
                "name TEXT NOT NULL, " +
                "age INTEGER NOT NULL, " +
                "height REAL NOT NULL, " +
                "weight REAL NOT NULL);";
        db.execSQL(sql);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    }
}
```

### 增删改查方法

```java
public class UserDBHelper extends SQLiteOpenHelper {
    private static final String DB_NAME = "user.db";
    private static final String TABLE_NAME = "user_info";
    private static final int DB_VERSION = 1;
    private static UserDBHelper mHelper = null;
    private SQLiteDatabase mRDB = null;
    private SQLiteDatabase mWDB = null;


    private UserDBHelper(Context context) {
        super(context, DB_NAME, null, DB_VERSION);
        // 该类的构造方法由于参数太多，我们可以在`super()`中传入自己设置的固定参数
        // context: 上下文对象，用于访问应用环境
        // DB_NAME: 数据库名称
        // null: 游标工厂，可以为null，表示使用默认的游标工厂
        // DB_VERSION: 数据库版本号，用于管理数据库的版本和升级
    }

    //利用单例模式获取数据库帮助器的唯一实例
    public static UserDBHelper getInstance(Context context) {
        if (mHelper == null) {
            mHelper = new UserDBHelper(context);
        }
        return mHelper;
    }

    // 打开数据库的读链接
    public SQLiteDatabase openReadLink() {
        if (mRDB == null || !mRDB.isOpen()) {
            mRDB = mHelper.getReadableDatabase();
        }
        return mRDB;
    }

    // 打开数据库的写链接
    public SQLiteDatabase openWriteLink() {
        if (mWDB == null || !mWDB.isOpen()) {
            mWDB = mHelper.getWritableDatabase();
        }
        return mWDB;
    }

    //关闭数据库链接
    public void closeLink() {
        if (mWDB != null && mWDB.isOpen()) {
            mWDB.close();
            mWDB = null;
        }
        if (mRDB != null && mRDB.isOpen()) {
            mRDB.close();
            mRDB = null;
        }
    }


    // 创建数据库，执行建表语句
    @Override
    public void onCreate(SQLiteDatabase db) {
        String sql = "CREATE TABLE IF NOT EXISTS " + TABLE_NAME + " (" +
                "id INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL, " +
                "name TEXT NOT NULL, " +
                "age INTEGER NOT NULL, " +
                "height REAL NOT NULL, " +
                "weight REAL NOT NULL);";
        db.execSQL(sql);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    }

    public long insert(User user) {
        ContentValues values = new ContentValues();
        values.put("name", user.getName());
        values.put("age", user.getAge());
        values.put("height", user.getHeight());
        values.put("weight", user.getWeight());
        return mWDB.insert(TABLE_NAME, null, values);
    }

    public long delete(String name) {

        return mWDB.delete(TABLE_NAME, "name = ?", new String[]{name});
    }

    public long update(User user) {
        ContentValues values = new ContentValues();
        values.put("name", user.getName());
        values.put("age", user.getAge());
        values.put("height", user.getHeight());
        values.put("weight", user.getWeight());
        return mWDB.update(TABLE_NAME, values, "name = ?", new String[]{user.getName()});
    }

    public List<User> queryAll() {
        List<User> list = new ArrayList<>();
        Cursor cursor = mRDB.query(TABLE_NAME, null, null, null, null, null, null);
        while(cursor.moveToNext()){
            User user = new User();
            user.setId(cursor.getInt(0));
            user.setName(cursor.getString(1));
            user.setAge(cursor.getInt(2));
            user.setHeight(cursor.getLong(3));
            user.setWeight(cursor.getFloat(4));
            list.add(user);
        }
        return list;
    }
}
```

### 使用 SQLite 数据库

```java
public class SecondActivity extends AppCompatActivity implements View.OnClickListener {

    private EditText editName, editAge, editHeight, editWeight;
    private Button btnAdd, btnDelete, btnUpdate, btnQuery;
    private SQLiteDatabase db;
    private UserDBHelper mDBHelper;
    private TextView textView2;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_second);

        textView2 = findViewById(R.id.textView2);

        editName = findViewById(R.id.editName);
        editAge = findViewById(R.id.editAge);
        editHeight = findViewById(R.id.editHeight);
        editWeight = findViewById(R.id.editWeight);

        btnAdd = findViewById(R.id.buttonAdd);
        btnDelete = findViewById(R.id.buttonDelete);
        btnUpdate = findViewById(R.id.buttonUpdate);
        btnQuery = findViewById(R.id.buttonQuery);

        btnAdd.setOnClickListener(this);
        btnDelete.setOnClickListener(this);
        btnUpdate.setOnClickListener(this);
    }

    @Override
    protected void onStart() {
        super.onStart();
        //获得数据库帮助器示例
        mDBHelper = UserDBHelper.getInstance(this);
        //打开数据库帮助器读写连接
        mDBHelper.openWriteLink();
        mDBHelper.openReadLink();
    }

    @Override
    protected void onStop() {
        super.onStop();
        // 关闭数据库帮助器读写连接
        mDBHelper.closeLink();
    }

    @Override
    public void onClick(View v) {
        String name = editName.getText().toString();
        int age = Integer.parseInt(editAge.getText().toString());
        long height = Long.parseLong(editHeight.getText().toString());
        float weight = Float.parseFloat(editWeight.getText().toString());


        if (v.getId() == R.id.buttonAdd) {
            User user = new User(name, age, height, weight);
            if (mDBHelper.insert(user) > 0) {
                Toast.makeText(this, "添加成功", Toast.LENGTH_SHORT).show();
            }
        } else if (v.getId() == R.id.buttonDelete) {
            if (mDBHelper.delete(name) > 0) {
                Toast.makeText(this, "删除成功", Toast.LENGTH_SHORT).show();
            }
        } else if (v.getId() == R.id.buttonUpdate) {
            User user = new User(name, age, height, weight);
            if (mDBHelper.update(user) > 0) {
                Toast.makeText(this, "修改成功", Toast.LENGTH_SHORT).show();
            }
        }
    }

    public void query(View view) {
        List<User> userList = mDBHelper.queryAll();
        StringBuilder sb = new StringBuilder();
        for (User user : userList) {
            sb.append(user.toString());
            sb.append("\n");
        }
        textView2.setText(sb.toString());
        Toast.makeText(this, "查询成功", Toast.LENGTH_SHORT).show();
    }
}
```

### 版本更新

> `onUpgrade` 方法是 `SQLiteOpenHelper` 类中的一个回调方法，用于处理数据库版本升级时的逻辑。每当你更新数据库的版本号时（通过改变 `DB_VERSION`），这个方法都会被调用，以便你可以对数据库进行必要的升级操作。

```java
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    // 这里编写数据库升级的代码
}
```

```java
@Override
public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    if (oldVersion < 2) {
        // 在版本 2 中添加一个新列
        String sql = "ALTER TABLE " + TABLE_NAME + " ADD COLUMN password VARCHAR";
        db.execSQL(sql);
    }
    // 如果有更多的版本升级逻辑，可以继续添加 else if 或其他条件
}
```

当改变 `SQLiteOpenHelper` 的 `DB_VERSION` 常量并重新启动应用时，`onUpgrade` 方法将会被调用。

```java
private static final int DB_VERSION = 2; // 更新版本号
```





# 相关知识点

## ContentValues 类

`ContentValues`类用于存储一组键值对，其中每个键（`key`）对应一个值（`value`），通常用于插入或更新数据库中的记录。

在`SQLiteDatabase`类中，`insert`方法的签名如下：

```java
public long insert(String table, String nullColumnHack, ContentValues values)
```

**`values`**：包含要插入的数据的`ContentValues`对象。每个键是列名，每个值是对应列的值。

```java
// 创建一个ContentValues对象
ContentValues values = new ContentValues();
values.put("name", "John Doe"); // 设置列“name”的值
values.put("age", 30);          // 设置列“age”的值
```

`getXxx()`--`put()`

## Cursor

> 数据结构，用于访问数据库查询结果的数据集合。它主要用于从`ContentProvider`中读取数据，也可以用于直接与SQLite数据库交互。`Cursor`对象提供了一种机制，可以按行逐条读取数据，并从中获取具体的字段值。

1. **特点**

> **数据集合**：`Cursor`表示数据库查询结果中的行集合，每行表示一条记录。
>
> **定位**：可以使用`Cursor`的方法来定位到特定的行。常用的方法包括`moveToFirst()`, `moveToNext()`, `moveToLast()`, 和 `moveToPosition(int position)`。
>
> **列名和数据类型**：在访问数据时，你需要知道每一列的名称或索引，以便正确地获取数据。你还需要知道列的数据类型，以便适当地转换数据类型。
>
> **随机访问**：`Cursor`支持在数据集中随机访问，允许你通过行号（索引）来获取数据。

2. **主要方法**

- **移动游标**
  - `moveToFirst()`: 将游标移动到结果集的第一行。如果结果集为空，则返回`false`。
  - `moveToNext()`: 将游标移动到下一行。如果已到达结果集的末尾，则返回`false`。
- **读取数据**
  - `getString(int columnIndex)`: 获取指定列索引的字符串数据。
  - `getInt(int columnIndex)`: 获取指定列索引的整数数据。
- **列信息**
  - `getColumnIndex(String columnName)`: 根据列名获取列索引。
  - `getColumnName(int columnIndex)`: 根据列索引获取列名。
  - `getColumnCount()`: 获取结果集中列的数量。
  - `getCount()`: 获取结果集中的行数。
- **关闭游标**
  - `close()`: 关闭`Cursor`对象，释放相关资源。应在完成对`Cursor`的操作后调用。

![image-20240730200227483](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240730200227483.png)





---

---

==***感谢您的阅读
如有错误烦请指正***==

---

>  参考：
>
>  1. [67-SQL基本语法_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV19U4y1R7zV?p=67&vd_source=b59429d4ff39742daea8fe7f3aac55a4)
