# ORM

ORM（Object-Relational Mapping，对象关系映射）是一种编程技术，用于在面向对象编程语言中处理关系型数据库。它通过将数据库中的表与程序中的对象相对应，简化了数据库操作，使开发者可以用对象操作的方式处理数据，而无需编写大量的SQL语句。

1. **实体类**
   每个数据库表对应一个类，每个表的列对应类中的一个属性。
2. **对象与表的映射**
   - 类 -> 表
   - 属性 -> 列
   - 对象 -> 数据表中的一行
3. **数据库操作**
   ORM框架会将对象的增删改查操作转化为相应的SQL语句，并与数据库交互。

# Room

> **ROOM** 是 Android 平台上 Google 提供的官方 ORM 框架，隶属于 Jetpack 组件之一，专为简化本地数据库（SQLite）的操作而设计。
>
> Room 将 SQLite 的功能封装成更易用的 API，允许开发者通过注解的方式操作数据库，而无需直接编写复杂的 SQL 语句。

## 核心组件

1. Entity（实体）
   - 数据表对应的类，定义表结构。
   - 使用 `@Entity` 注解表示。
2. DAO（Data Access Object）
   - 数据访问接口，用于定义操作数据库的方法。
   - 使用 `@Dao` 注解表示。
3. Database
   - 数据库的入口，管理实体和数据访问对象。
   - 使用 `@Database` 注解表示。



![image-20241215155210456](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241215155210456.png)

## 数据实体(Entity)

> 定义实体来表示要存储的对象。每个实体都对应于关联的 Room 数据库中的一个表，并且实体的每个实例都表示相应表中的一行数据。

### 表名

使用 `@Entity` 注解并指定表名：

```java
@Entity(tableName = "users")
public class User {
    // 定义字段
}
```

> `@Entity` 默认使用类名作为表名，如果需要自定义表名，可以通过 `tableName` 属性设置。

### 主键

通过 `@PrimaryKey` 注解标注

（1）自增主键

使用 `autoGenerate = true` 自动生成主键值：

```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey(autoGenerate = true)
    private int id; // 自增主键
}
```

（2）非自增主键

不设置 `autoGenerate`，需要手动插入主键值：

```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey
    @NonNull
    private String userId; // 自定义的主键，需手动赋值
}
```

（3）联合主键

如果需要通过多个列的组合对实体实例进行唯一标识如果，可以通过 `@Entity` 的 `primaryKeys` 属性定义：

```java
@Entity(
    tableName = "users",
    primaryKeys = {"userId", "email"}
)
public class User {
    @NonNull
    private String userId;

    @NonNull
    private String email;
}
```

### 表项

使用 `@ColumnInfo` 注解自定义字段名称或其他属性：

```java
@Entity(tableName = "users")
public class User {
    @PrimaryKey(autoGenerate = true)
    private int id;

    @ColumnInfo(name = "user_name") 
    private String name;

    // 默认值
    @ColumnInfo(name = "email_address", defaultValue = "unknown@example.com")
    private String email;

    // 自定义表项名称
    @ColumnInfo(name = "created_at")
    private long createdAt;

    // 忽略字段
    @Ignore
    Bitmap picture;
}
```

`@ColumnInfo` 常用属性

1. **`name`**：定义字段在数据库中的名称
2. **`typeAffinity`**：指定字段的 SQLite 数据类型（如 `TEXT`, `INTEGER`）
3. **`defaultValue`**：为字段设置默认值
4. **`collate`**：设置排序规则

## 数据访问对象(DAO)

> 通过定义**数据访问对象 (DAO)** 与存储的数据进行交互。
>
> 在 Room 中，DAO (Data Access Object) 是用于定义数据库操作接口的核心部分。可以通过注解标注某个接口或抽象类为 DAO，并定义好所需的 CRUD 方法。
>
> 每个 DAO 都包含一些方法，这些方法可用于对应用的数据库进行抽象访问。在编译时，Room 会自动为定义的 DAO 生成实现。

### 注解标注DAO类

通过 `@Dao` 注解，将一个接口或抽象类标记为 DAO 类，通常应使用接口

```java
@Dao
public interface UserDao {
    // 定义 CRUD 方法
}
```

`@Dao` 的作用：

- 告诉 Room 这是一个用于数据访问的接口。
- 通过注解方法实现 CRUD 操作。

### 定义CRUD方法

#### @Insert

> 调用 `@Insert` 方法时，Room 会将每个传递的实体实例插入到相应的数据库表中。
>
> `@Insert` 方法的每个参数都必须是一个带有 `@Entity` 注解的Room数据实体类实例，或数据实体类实例的集合，而且每个参数都指向一个数据库。

- **单条插入**：

```java
@Insert
void insertUser(User user);
```

- **批量插入**：

```java
@Insert
void insertUsers(List<User> users);
```

使用示例：

```java
User user = new User();
user.setName("Alice");
userDao.insertUser(user);

List<User> userList = Arrays.asList(
    new User("Bob", "bob@example.com"),
    new User("Cathy", "cathy@example.com")
);
userDao.insertUsers(userList);
```

#### @Query

- **查询所有数据**：

```java
@Query("SELECT * FROM users")
List<User> getAllUsers();
```

- **条件查询**：

```java
@Query("SELECT * FROM users WHERE id = :userId")
User getUserById(int userId);
```

- **支持返回 LiveData 或 Flow**（适用于观察数据变化）：

```java
@Query("SELECT * FROM users")
LiveData<List<User>> getAllUsersLiveData();
```

使用示例：

```java
List<User> users = userDao.getAllUsers();
User user = userDao.getUserById(1);
```

#### @Update

> Room 使用主键将传递的实体实例与数据库中的行进行匹配。如果没有具有相同主键的行，Room 不会进行任何更改。

```java
@Update
void updateUser(User user);
```

- **批量更新**：

```java
@Update
void updateUsers(List<User> users);
```

使用示例：

```java
User user = userDao.getUserById(1);
user.setName("Updated Name");
userDao.updateUser(user);
```

#### @Delete

删除传入的对象，Room 根据主键匹配删除记录：

```java
@Delete
void deleteUser(User user);
```

- **批量删除**：

```java
@Delete
void deleteUsers(List<User> users);
```

使用示例：

```java
User user = userDao.getUserById(1);
userDao.deleteUser(user);
```

## 数据库

 `AppDatabase` 类用于保存数据库。 `AppDatabase` 定义数据库配置，并作为应用对持久性数据的主要访问点。数据库类必须满足以下条件：

- 该类必须带有 `@Database`注解，该注解包含列出所有与数据库关联的数据实体的 `entities`数组。
- 该类必须是一个抽象类，用于扩展 `RoomDatabase`
- 对于与数据库关联的每个DAO类，数据库类必须定义一个具有零参数的抽象方法，并返回DAO类的实例。

```java
// 标注此类为数据库类，包含的表为 User，版本号为 1
@Database(entities = {User.class}, version = 1)
public abstract class UserDataBase extends RoomDatabase {
    // 定义一个抽象方法，返回 DAO（数据访问对象）实例，用于操作 User 表数据
    public abstract UserDao userDao();

    // 用于存储单例实例的静态变量，确保全局只有一个数据库实例
    private static volatile UserDataBase INSTANCE;

    // 获取数据库实例的静态方法，采用单例模式。
    public static UserDataBase getINSTANCE(Context context) {
        if (INSTANCE == null) {
            synchronized (UserDataBase.class) {
                if (INSTANCE == null) {
                    // 使用 Room 的 databaseBuilder 构建数据库实例 
                    // 数据库类,数据库文件名
                    INSTANCE = Room.databaseBuilder(context.getApplicationContext(), UserDataBase.class, "users").build();                                       
                }
            }
        }
        return INSTANCE; 
    }
}
```

# Dao_Impl

![image-20241215163951647](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20241215163951647.png)

**`Dao_Impl` 类**：

`UserDao_Impl` 类是由 Room 编译器生成的实现类，继承了 `UserDao` 接口并实现了所有数据库操作的方法

**构造**

`UserDao_Impl` 构造方法接受一个 `RoomDatabase` 实例并初始化了用于执行插入、删除和更新操作的适配器。

- **插入适配器**：`__insertionAdapterOfUser`，用于执行用户数据插入的 SQL 语句。
- **删除适配器**：`__deletionAdapterOfUser`，用于执行用户数据删除的 SQL 语句。
- **更新适配器**：`__updateAdapterOfUser`，用于执行用户数据更新的 SQL 语句。

**事务管理**

所有数据库操作都通过事务管理 (`__db.beginTransaction()` 和 `__db.setTransactionSuccessful()`)，以确保数据操作的一致性和原子性。在操作结束后，事务被结束 (`__db.endTransaction()`)。

**数据库操作方法**

- **`insertUser(User user)`**：将传入的 `User` 实体插入数据库。通过 `EntityInsertionAdapter` 来执行插入操作。
- **`deleteUser(User user)`**：根据 `User` 实体的 `id` 删除对应的用户数据。通过 `EntityDeletionOrUpdateAdapter` 来执行删除操作。
- **`updateUser(User user)`**：根据 `User` 实体的 `id` 更新用户数据。通过 `EntityDeletionOrUpdateAdapter` 来执行更新操作。

**查询方法**

- **`findUserByEmail(String email)`**：查询 `users` 表中是否存在指定 `email` 的用户，返回该用户数量。
- **`validateAccount(String email, String password)`**：根据传入的 `email` 和 `password` 查询用户信息，验证账户的有效性。如果匹配成功，返回对应的 `User` 实体。

**SQL 语句绑定**

对于每个数据库操作（插入、删除、更新、查询），都会通过 `SupportSQLiteStatement` 绑定具体的参数，这些参数来自 `User` 实体的属性。例如，插入时将 `id`、`email`、`password` 和 `userName` 绑定到 SQL 语句中。



> 通过APT生成了`UserDao_Impl`实现了`UserDao`接口的所有关于数据库操作的方法。
>
> `UserDao_Impl`持有对RoomDatabase的引用。而`RoomDatabase`在持有数据库实例
>
> `UserDao_Impl`通过`UserDao`里面的方法通过注解生成数据库语句并调用数据库实例执行语句。

**UserDao_Impl**

```java
@Generated("androidx.room.RoomProcessor") //表示该类由Room编译器生成
public final class UserDao_Impl implements UserDao { // 实现了UserDao接口
    private final RoomDatabase __db; // Room数据库实例

    // 插入、删除、更新User实体的适配器
    private final EntityInsertionAdapter<User> __insertionAdapterOfUser;
    private final EntityDeletionOrUpdateAdapter<User> __deletionAdapterOfUser;
    private final EntityDeletionOrUpdateAdapter<User> __updateAdapterOfUser;

    // 构造方法，初始化数据库和适配器
    public UserDao_Impl(@NonNull final RoomDatabase __db) {
        this.__db = __db;
        
        // 初始化插入适配器
        this.__insertionAdapterOfUser = new EntityInsertionAdapter<User>(__db) {
            @Override
            @NonNull
            protected String createQuery() {
                // 插入用户数据的SQL语句
                return "INSERT OR ABORT INTO `users` (`id`,`email`,`password`,`userName`) VALUES (nullif(?, 0),?,?,?)";
            }

            @Override
            protected void bind(@NonNull final SupportSQLiteStatement statement, final User entity) {
                // 绑定数据到插入语句
                statement.bindLong(1, entity.getId());
                if (entity.getEmail() == null) {
                    statement.bindNull(2);
                } else {
                    statement.bindString(2, entity.getEmail());
                }
                if (entity.getPassword() == null) {
                    statement.bindNull(3);
                } else {
                    statement.bindString(3, entity.getPassword());
                }
                if (entity.getUserName() == null) {
                    statement.bindNull(4);
                } else {
                    statement.bindString(4, entity.getUserName());
                }
            }
        };

        // 初始化删除适配器
        this.__deletionAdapterOfUser = new EntityDeletionOrUpdateAdapter<User>(__db) {
            @Override
            @NonNull
            protected String createQuery() {
                // 删除用户数据的SQL语句
                return "DELETE FROM `users` WHERE `id` = ?";
            }

            @Override
            protected void bind(@NonNull final SupportSQLiteStatement statement, final User entity) {
                // 绑定数据到删除语句
                statement.bindLong(1, entity.getId());
            }
        };

        // 初始化更新适配器
        this.__updateAdapterOfUser = new EntityDeletionOrUpdateAdapter<User>(__db) {
            @Override
            @NonNull
            protected String createQuery() {
                // 更新用户数据的SQL语句
                return "UPDATE OR ABORT `users` SET `id` = ?,`email` = ?,`password` = ?,`userName` = ? WHERE `id` = ?";
            }

            @Override
            protected void bind(@NonNull final SupportSQLiteStatement statement, final User entity) {
                // 绑定数据到更新语句
                statement.bindLong(1, entity.getId());
                if (entity.getEmail() == null) {
                    statement.bindNull(2);
                } else {
                    statement.bindString(2, entity.getEmail());
                }
                if (entity.getPassword() == null) {
                    statement.bindNull(3);
                } else {
                    statement.bindString(3, entity.getPassword());
                }
                if (entity.getUserName() == null) {
                    statement.bindNull(4);
                } else {
                    statement.bindString(4, entity.getUserName());
                }
                statement.bindLong(5, entity.getId());
            }
        };
    }

    @Override
    public void insertUser(final User user) {
        __db.assertNotSuspendingTransaction(); // 确保数据库操作不是在挂起事务中进行
        __db.beginTransaction(); // 开始事务
        try {
            __insertionAdapterOfUser.insert(user); // 插入数据
            __db.setTransactionSuccessful(); // 设置事务成功
        } finally {
            __db.endTransaction(); // 结束事务
        }
    }

    @Override
    public void deleteUser(final User user) {
        __db.assertNotSuspendingTransaction(); // 确保数据库操作不是在挂起事务中进行
        __db.beginTransaction(); // 开始事务
        try {
            __deletionAdapterOfUser.handle(user); // 删除数据
            __db.setTransactionSuccessful(); // 设置事务成功
        } finally {
            __db.endTransaction(); // 结束事务
        }
    }

    @Override
    public void updateUser(final User user) {
        __db.assertNotSuspendingTransaction(); // 确保数据库操作不是在挂起事务中进行
        __db.beginTransaction(); // 开始事务
        try {
            __updateAdapterOfUser.handle(user); // 更新数据
            __db.setTransactionSuccessful(); // 设置事务成功
        } finally {
            __db.endTransaction(); // 结束事务
        }
    }

    // 查找用户通过邮件
    @Override
    public int findUserByEmail(final String email) {
        final String _sql = "SELECT COUNT(*) FROM users WHERE email = ?";
        final RoomSQLiteQuery _statement = RoomSQLiteQuery.acquire(_sql, 1);
        int _argIndex = 1;
        if (email == null) {
            _statement.bindNull(_argIndex);
        } else {
            _statement.bindString(_argIndex, email);
        }
        __db.assertNotSuspendingTransaction(); // 确保数据库操作不是在挂起事务中进行
        final Cursor _cursor = DBUtil.query(__db, _statement, false, null);
        try {
            final int _result;
            if (_cursor.moveToFirst()) {
                _result = _cursor.getInt(0); // 获取查询结果
            } else {
                _result = 0; // 如果没有查询到结果，返回0
            }
            return _result;
        } finally {
            _cursor.close();
            _statement.release();
        }
    }

    // 验证账户通过电子邮件和密码
    @Override
    public User validateAccount(final String email, final String password) {
        final String _sql = "SELECT * FROM users WHERE email = ? AND password = ?";
        final RoomSQLiteQuery _statement = RoomSQLiteQuery.acquire(_sql, 2);
        int _argIndex = 1;
        if (email == null) {
            _statement.bindNull(_argIndex);
        } else {
            _statement.bindString(_argIndex, email);
        }
        _argIndex = 2;
        if (password == null) {
            _statement.bindNull(_argIndex);
        } else {
            _statement.bindString(_argIndex, password);
        }
        __db.assertNotSuspendingTransaction(); // 确保数据库操作不是在挂起事务中进行
        final Cursor _cursor = DBUtil.query(__db, _statement, false, null);
        try {
            final int _cursorIndexOfId = CursorUtil.getColumnIndexOrThrow(_cursor, "id");
            final int _cursorIndexOfEmail = CursorUtil.getColumnIndexOrThrow(_cursor, "email");
            final int _cursorIndexOfPassword = CursorUtil.getColumnIndexOrThrow(_cursor, "password");
            final int _cursorIndexOfUserName = CursorUtil.getColumnIndexOrThrow(_cursor, "userName");
            final User _result;
            if (_cursor.moveToFirst()) {
                // 解析查询结果并返回User对象
                _result = new User();
                _result.setId(_cursor.getInt(_cursorIndexOfId));
                _result.setEmail(_cursor.isNull(_cursorIndexOfEmail) ? null : _cursor.getString(_cursorIndexOfEmail));
                _result.setPassword(_cursor.isNull(_cursorIndexOfPassword) ? null : _cursor.getString(_cursorIndexOfPassword));
                _result.setUserName(_cursor.isNull(_cursorIndexOfUserName) ? null : _cursor.getString(_cursorIndexOfUserName));
            } else {
                _result = null; // 如果没有找到符合条件的用户，返回null
            }
            return _result;
        } finally {
            _cursor.close();
            _statement.release();
        }
    }

    // 获取所需的转换器（这里为空，因为没有复杂的类型转换）
    @NonNull
    public static List<Class<?>> getRequiredConverters() {
        return Collections.emptyList();
    }
}
```



---



> 参考：
>
> 1. [Android Jetpack架构组件之Room入门及源码分析 - 简书](https://www.jianshu.com/p/71bd78117919)
> 2. [Room  | Jetpack  | Android Developers](https://developer.android.google.cn/jetpack/androidx/releases/room?hl=zh_cn)
