@[toc]

# ListView

> `ListView` 是 Android 中的一种视图组件，用于显示可滚动的垂直列表。每个列表项都是一个视图对象，`ListView` 会通过适配器（`Adapter`）将数据绑定到这些视图对象上。它通常用于显示一组相似的数据，比如联系人列表、消息列表等。

## 步骤

1. **准备数据**：这是数据源，通常是一个数组或一个`List`。
2. **构建适配器**：适配器用于将数据映射到ListView的每一项。你可以使用系统提供的适配器（如`ArrayAdapter`）或者自定义适配器。
3. **绑定适配器到ListView**：将适配器设置到ListView上。
4. **处理ListView项的点击事件**：添加点击事件监听器来处理每一项的点击事件。

## 适配器Adpter

作用：充当 `ListView` 和数据源之间的桥梁，将数据源转换为可以显示在 `ListView` 中的视图项。

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240723093657352.png" alt="image-20240723093657352" style="zoom:80%;" />

**常用适配器类型**

1. **ArrayAdapter**：适用于简单的数据源，如数组或列表。它将每个数据项转换为一个 `TextView` 或其他简单视图。
2. **SimpleAdapter**：用于将复杂的数据源（如 `List<Map<String, Object>>`）绑定到多个视图。
3. **Custom Adapter**：通过继承 `BaseAdapter` 或其他适配器类，可以创建自定义适配器以实现复杂的需求。

## ArrayAdapter

新建一个ArrayListActivity类及其布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ListView
        android:id="@+id/lv"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

添加入一个ListView控件

```java
public class ArrayListActivity extends AppCompatActivity {
    private ListView mListView;
    private List<String> mStringList;
    private ArrayAdapter<String> mArrayAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_array_list);
       
        // 初始化 ListView。
        mListView = findViewById(R.id.lv);
        
        // 初始化字符串列表并填充数据。
        mStringList = new ArrayList<>();
        for (int i = 0; i < 50; i++) {
            mStringList.add("这是条目" + i);
        }
        
        // 使用 ArrayAdapter 绑定数据到 ListView。
        mArrayAdapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, mStringList);
        mListView.setAdapter(mArrayAdapter);
        
        // 设置 ListView 的点击事件。
        mListView.setOnItemClickListener((parent, view, position, id) -> {
            Toast.makeText(ArrayListActivity.this, "你点击了" + position, Toast.LENGTH_SHORT).show();
        });
        // 设置 ListView 的长按事件。
        mListView.setOnItemLongClickListener((parent, view, position, id) -> {
            Toast.makeText(ArrayListActivity.this, "你长按了" + position, Toast.LENGTH_SHORT).show();
            return true;
        });
    }
}
```

* `mListView = findViewById(R.id.lv);`

  获得ListView组件

* `mStringList = new ArrayList<>();`

  创建一个list储存信息

* `mArrayAdapter = new ArrayAdapter<>(this, android.R.layout.simple_list_item_1, mStringList);`

  1. 创建了一个ArrayAdapter对象，命名为mArrayAdapter。
  2. 通过构造函数传入了三个参数：
     * `this`：表示当前的上下文对象，即这个ArrayAdapter将被用于当前的Activity或Fragment中。
     * `android.R.layout.simple_list_item_1`：表示列表项的布局文件，这里使用了Android系统提供的简单列表项布局，该布局只有一个文本视图。
     * `mStringList`：表示要显示的数据集合，即一个字符串列表。

  通过这个ArrayAdapter对象，可以将`mStringList`中的字符串显示在列表视图中。

* `mListView.setAdapter(mArrayAdapter);`

  将`mArrayAdapter`对象设置为`mListView`的适配器

实现效果：

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-23-10-28-26-233_com.example.la.jpg" alt="Screenshot_2024-07-23-10-28-26-233_com.example.la" style="zoom: 25%;" />

## SimpleAdapter

> SimpleAdapter是Android中用于将数据模型转换成ListView或其他视图组件的适配器。它简化了数据绑定过程，通过映射数据集中的字段到布局文件中的视图

同样新建一个ArrayListActivity类及其布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <ListView
        android:id="@+id/lv"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

xml和刚才一致

```java
public class SimpleListActivity extends AppCompatActivity {
    
    private ListView mListView;
    private SimpleAdapter msimpleAdapter;
    private List<Map<String, Object>> mList;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_simple_list);
        
        // 初始化ListView
        mListView = findViewById(R.id.lv);
        // 初始化并填充列表数据
        mList = new ArrayList<>();
        for (int i = 0; i < 5; i++) {
            Map<String, Object> map1 = new HashMap<>();
            map1.put("image", R.drawable.apple);
            map1.put("text", "苹果");
            mList.add(map1);
            Map<String, Object> map2 = new HashMap<>();
            map2.put("image", R.drawable.banana);
            map2.put("text", "香蕉");
            mList.add(map2);
            Map<String, Object> map3 = new HashMap<>();
            map3.put("image", R.drawable.litchi);
            map3.put("text", "荔枝");
            mList.add(map3);
        }
        
        // 使用SimpleAdapter绑定数据到ListView
        msimpleAdapter = new SimpleAdapter(this, mList, R.layout.list_item_layout,
                new String[]{"image", "text"}, new int[]{R.id.image, R.id.tv});
        mListView.setAdapter(msimpleAdapter);
    } 
}
```

* `msimpleAdapter = new SimpleAdapter(this, mList, R.layout.list_item_layout, `

  `new String[]{"image", "text"}, new int[]{R.id.image, R.id.tv});`

  1. 创建了一个SimpleAdapter对象，用于将数据绑定到ListView上。
  2. `this`：上下文对象，表示当前的Activity或Fragment。
  3. `mList`：数据源，通常是一个List集合，包含了要展示的数据。
  4. `R.layout.list_item_layout`：不同于刚才安卓自带的，这里我们使用自己创建的布局，下面的两个参数`String[]`是刚才map数组中的键，`int[]`是自定义布局中对应的视图id
  5. `new String[]{"image", "text"}`：数据源中要展示的字段名数组，这里表示要展示名为"image"和"text"的字段。
  6. `new int[]{R.id.image, R.id.tv}`：对应字段在布局文件中的控件ID数组，这里表示字段"image"对应ID为R.id.image的ImageView控件，字段"text"对应ID为R.id.tv的TextView控件

* `mListView.setAdapter(msimpleAdapter);`

`R.layout.list_item_layout`：自定义的布局

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">

    <ImageView
        android:id="@+id/image"
        android:layout_width="0dp"
        android:layout_height="100dp"
        android:layout_weight="1" />

    <TextView
        android:id="@+id/tv"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_weight="3" />

</LinearLayout>
```

实际效果：

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-23-11-29-07-637_com.example.la.jpg" alt="Screenshot_2024-07-23-11-29-07-637_com.example.la" style="zoom:25%;" />

## BaseAdpter

新建一个`ItemBean`类来存储数据

```java
public class ItemBean {
    private String name;
    private int imageId;

    public ItemBean(String name, int imageId) {
        this.name = name;
        this.imageId = imageId;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setImageId(int imageId) {
        this.imageId = imageId;
    }

    public String getName() {
        return name;
    }

    public int getImageId() {
        return imageId;
    }
}
```

**接下来为这个类构造适配器**：

继承`BaseAdapter`类，自动生成四个重写方法

```JAVA
/**
 * 自定义适配器，用于在ListView中显示物品列表。
 * 该适配器负责将数据集中的每个项目绑定到相应的视图上。
 */
public class MyAdapter extends BaseAdapter {
    // 存储物品数据的列表
    private List<ItemBean> mlist;
    // LayoutInflater用于从XML文件中加载布局
    private LayoutInflater mLayoutInflater;
    // 上下文对象，用于获取资源和进行其他上下文相关的操作
    private Context mcontext;

    /**
     * 构造函数初始化适配器。
     * 
     * @param mlist 物品数据的列表
     * @param mcontext 上下文对象，用于初始化LayoutInflater
     */
    public MyAdapter(List<ItemBean> mlist, Context mcontext) {
        this.mlist = mlist;
        this.mcontext = mcontext;
        this.mLayoutInflater = LayoutInflater.from(mcontext);
    }

    //返回数据集的大小
    @Override
    public int getCount() {
        return mlist.size();
    }

    //根据位置获取数据集中的物品。
    @Override
    public Object getItem(int position) {
        return mlist.get(position);
    }

    // 获取物品在ListView中的唯一标识符
    @Override
    public long getItemId(int position) {
        return position;
    }

    /**
     * 为ListView的每个项目创建并返回一个视图。
     * 
     * @param position 物品的位置
     * @param convertView 当前被重用的视图
     * @param parent 视图的父容器
     * @return 绑定到数据集中的视图
     */
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        // 使用LayoutInflater加载列表项的布局
        convertView = mLayoutInflater.inflate(R.layout.list_item_layout, parent, false);
        
        // 查找并初始化列表项中的ImageView和TextView
        ImageView img = convertView.findViewById(R.id.image);
        TextView tv = convertView.findViewById(R.id.tv);
        
        // 获取当前位置的物品数据,设置ImageView的图片资源
        ItemBean itemBean = mlist.get(position);
        img.setImageResource(itemBean.getImageId());
        tv.setText(itemBean.getName());
        
        return convertView;
    }
}
```

* ` convertView = mLayoutInflater.inflate(R.layout.list_item_layout, parent, false);`

  `mLayoutInflater`通过构造方法处的`LayoutInflater.from(mcontext)`赋值

  调用其`inflate`方法：

  1. `convertView`: 这个变量通常用来存储从布局文件转换而来的`View`对象。在`ListView`或`RecyclerView`的`getView()`方法中，它会被检查是否为空。如果非空，那么会直接使用这个`View`对象来展示数据，以避免频繁创建新的`View`，从而提高性能。
  2. `mLayoutInflater`: 这是一个`LayoutInflater`实例，它是Android框架提供的一个类，用于将XML布局文件转换为对应的View对象。
  3. `R.layout.list_item_layout`: 这是一个资源ID，指向了XML布局文件`list_item_layout.xml`。这个布局文件定义了一个`ListView`或`RecyclerView`中单个item的外观和结构，包括控件的类型、大小、位置以及样式等。
  4. `parent`: 这是一个`ViewGroup`类型的参数，代表了新创建的View最终将被添加到的父容器。在`ListView`或`RecyclerView`的情况下，`parent`就是`ListView`或`RecyclerView`本身。虽然在这个调用中，我们并没有立即把新创建的`View`添加到`parent`中（因为false参数），但这个参数还是需要的，因为它会影响`LayoutInflater`如何计算View的尺寸和位置。
  5. `false`: 这个布尔值参数告诉`LayoutInflater`不要将生成的`View`添加到`parent`中。这是因为`ListView`或`RecyclerView`有自己的逻辑来管理子`View`的添加和移除，它们会在适当的时候将`View`添加到自己内部的`ViewGroup`中。

  下面是`BaseAdpterActivity`类，和前两个步骤相似

  ```java
  public class BaseAdpterActivity extends AppCompatActivity {
      
      private ListView mListView;
      private BaseAdapter mBaseAdapter;
      private List<ItemBean> mlist;
  
      @Override
      protected void onCreate(Bundle savedInstanceState) {
          super.onCreate(savedInstanceState);
          setContentView(R.layout.activity_base_adpter);
  
          // 初始化数据列表，这里使用了循环来添加多个数据项。
          mlist = new ArrayList<>();
          for (int i = 0; i < 20; i++) {
              ItemBean itemBean1 = new ItemBean("apple", R.drawable.apple);
              ItemBean itemBean2 = new ItemBean("banana", R.drawable.banana);
              ItemBean itemBean3 = new ItemBean("litchi", R.drawable.litchi);
              mlist.add(itemBean1);
              mlist.add(itemBean2);
              mlist.add(itemBean3);
          }
  
          // 初始化ListView和其适配器
          mListView = findViewById(R.id.lv);
          mBaseAdapter = new MyAdapter(mlist, this);
          mListView.setAdapter(mBaseAdapter);
      }
  }
  ```

  效果：

  <img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-23-12-24-11-021_com.example.la.jpg" alt="Screenshot_2024-07-23-12-24-11-021_com.example.la" style="zoom:25%;" />

## 效率问题

列表中信息的出现需要频繁调用`getView`方法，效率很低。

```java
public class MyAdapter extends BaseAdapter {
    //省略这里代码.......
    @Override
    public View getView(int position, View convertView, ViewGroup parent) {
        
        ViewHolder viewHolder;
        
        if (convertView == null) {
            // 如果convertView为空，说明该视图是第一次被创建
            convertView = mLayoutInflater.inflate(R.layout.list_item_layout, parent, false);
            viewHolder = new ViewHolder();
            viewHolder.img = convertView.findViewById(R.id.image);
            viewHolder.tv = convertView.findViewById(R.id.tv);
            convertView.setTag(viewHolder);
        } else {
            // 如果convertView不为空，说明该视图可以被复用，直接从标签中获取ViewHolder对象
            viewHolder = (ViewHolder) convertView.getTag();
        }
        // 获取当前位置的物品数据
        ItemBean itemBean = mlist.get(position);
        // 绑定数据到视图
        viewHolder.img.setImageResource(itemBean.getImageId());
        viewHolder.tv.setText(itemBean.getName());

        return convertView;
    }

    /**
     * ViewHolder类用于缓存ListView项的视图引用，
     * 以减少在getView方法中每次都需要通过findViewById查找视图的开销。
     */
    class ViewHolder {
        ImageView img;
        TextView tv;
    }
}
```

1. `convertView`为空：创建一个新的视图并初始化它，然后使用 `setTag`() 方法将一个包含子控件引用的对象与 `convertView` 关联起来。
2.  `convertView`不为`null`，则调用 `getTag()` 方法来获取之前存储的 `ViewHolder` 对象，这样就可以直接访问子控件而不需要再次查找它们。
3. 使用`setTag()`和`getTag()`方法来缓存这些子控件的引用
4. `ViewHolder`：每次`getView()`方法被调用时，如果直接在视图中使用`findViewById()`方法来获取子视图，这会导致性能下降，因为`findViewById()`是一个耗时的操作。`ViewHolder`模式通过在视图首次创建时保存对所有子视图的引用，避免了后续滚动时的多次查找

# RecyclerView

> `RecyclerView` 是 Android 提供的一个更高级和灵活的列表视图控件，相对于 `ListView`，`RecyclerView` 提供了更多的功能和更好的性能。它引入了一些新的概念，如 `ViewHolder` 模式，更高效的滚动和动画支持，以及更灵活的布局管理器（`LayoutManager`）。

## 具体实现

可以实现和`ListView`一致的效果

<img src="https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenshot_2024-07-23-12-24-11-021_com.example.la.jpg" alt="Screenshot_2024-07-23-12-24-11-021_com.example.la" style="zoom:25%;" />

示例目录：

![image-20240723154019488](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240723154019488.png)

1. **主布局文件 `activity_main.xml`**

加入`RecyclerView`控件，宽高设置`match_parent`占满布局。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/rlv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:textAllCaps="false"/>

</LinearLayout>
```

2. 数据模型 `FruitBean.java`，和刚刚的一致

```java
public class FruitBean implements Serializable {
    private String name;
    private int imageId;

    public FruitBean() {
    }
    public FruitBean(String name, int imageId) {
        this.name = name;
        this.imageId = imageId;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setImageId(int imageId) {
        this.imageId = imageId;
    }

    public String getName() {
        return name;
    }

    public int getImageId() {
        return imageId;
    }
}
```

3. 自定义适配器`MyAdapter`

```java
// RecyclerView的适配器类，用于将数据集中的数据绑定到RecyclerView的各个Item上。
//继承RecyclerView.Adapter，这里的泛型是下面的内部类
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {

    
    // 数据源列表
    private List<FruitBean> mList;
    // 布局填充器，用于将XML布局文件转换为View对象
    private LayoutInflater mLayoutInflater;
    // 上下文对象
    private Context mContext;

    //构造函数，初始化适配器。
    public MyAdapter(List<FruitBean> mList, Context mContext) {
        this.mList = mList;
        this.mContext = mContext;
        this.mLayoutInflater = LayoutInflater.from(mContext);
    }

    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        // 将布局文件转换为View对象
        View view = mLayoutInflater.inflate(R.layout.list_item_layout, parent, false);
        // 创建并返回ViewHolder实例
        MyViewHolder myViewHolder = new MyViewHolder(view);
        return myViewHolder;
    }

     // 将数据绑定到ViewHolder，在RecyclerView滚动时会不断调用
    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
       // 获取当前项的数据
        FruitBean fruitBean = mList.get(position);

        // 将数据绑定到ViewHolder中的TextView和ImageView
        holder.tv.setText(fruitBean.getName());
        holder.img.setImageResource(fruitBean.getImageId());
    }

    @Override
    public int getItemCount() {
        return mList.size();
    }

     // ViewHolder类，用于持有每个列表项的视图
    class MyViewHolder extends RecyclerView.ViewHolder {
		// 列表项中的ImageView和TextView
        private ImageView img;
        private TextView tv;

        public MyViewHolder(View view) {
            super(view);

            // 通过itemView获取布局中的控件，并设置参数
            this.img = view.findViewById(R.id.image);
            this.tv = view.findViewById(R.id.tv);
        }
    }
}
```

4. `MainActivity`中

```java
public class MainActivity extends AppCompatActivity {
    // 声明RecyclerView、数据列表和适配器
    private RecyclerView mrecyclerView;
    private List<FruitBean> mlist;
    private MyAdapter myAdapter;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 初始化数据
        initData();

        // 绑定RecyclerView，设置适配器
        mrecyclerView = findViewById(R.id.rlv);
        myAdapter = new MyAdapter(mlist, this);
        mrecyclerView.setAdapter(myAdapter);
        // 设置布局管理器，这里是竖向滚动
        RecyclerView.LayoutManager layoutManager = new LinearLayoutManager(this);
        mrecyclerView.setLayoutManager(layoutManager);
    }

    // 初始化数据列表，添加多个水果项
    private void initData() {
        mlist = new ArrayList<>();
        for (int i = 0; i < 20; i++) {
            FruitBean itemBean1 = new FruitBean("apple", R.drawable.apple);
            FruitBean itemBean2 = new FruitBean("banana", R.drawable.banana);
            FruitBean itemBean3 = new FruitBean("litchi", R.drawable.litchi);
            mlist.add(itemBean1);
            mlist.add(itemBean2);
            mlist.add(itemBean3);
        }
    }
}
```





## 不同布局形式的设置

### 横向滚动

只用改一行代码

```java
 RecyclerView.LayoutManager layoutManager = new LinearLayoutManager(this,LinearLayoutManager.HORIZONTAL,false);
        mrecyclerView.setLayoutManager(layoutManager);
```

`new LinearLayoutManager(this, LinearLayoutManager.HORIZONTAL, false)`：

- 这里创建了一个`LinearLayoutManager`实例
- `this`：传入的是当前`Activity`的上下文（`Context`）。
- `LinearLayoutManager.HORIZONTAL`：表示RecyclerView的布局方向为水平（HORIZONTAL），即列表项将水平排列。
- `false`：表示不反转布局。`false`意味着列表项从起点到终点正常排列，如果设置为`true`，列表项将从终点到起点反向排列。

### 瀑布流

```java
RecyclerView.LayoutManager layoutManager = new StaggeredGridLayoutManager(2,StaggeredGridLayoutManager.VERTICAL);
```

`new StaggeredGridLayoutManager(2, StaggeredGridLayoutManager.HORIZONTAL)`：

- `2`：表示布局中有2个跨度（列或行，取决于方向）。在水平布局中，这表示有2行。
- `StaggeredGridLayoutManager.HORIZONTAL`：表示RecyclerView的布局方向为垂直，即列表项将垂直排列，并且每一列包含多个项目。

### 网格

```java
RecyclerView.LayoutManager layoutManager = new GridLayoutManager(this,2);
```

`new GridLayoutManager(this, 2)`：

- `this`：传入的是当前`Activity`的上下文（`Context`）。
- `2`：表示布局中有2个跨度（列）。这意味着RecyclerView的每一行将包含2个项目。

## 点击事件

在`ViewHolder`中添加`rlContainer`变量来保存最外层布局的实例，再在`onBindViewHolder`方法中为他注册点击事件

```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.MyViewHolder> {
//省略
    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        FruitBean fruitBean = mList.get(position);

        holder.tv.setText(fruitBean.getName());
        holder.img.setImageResource(fruitBean.getImageId());

        // 设置容器布局的点击事件
        holder.rlContainer.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Toast.makeText(mContext, "你点击了" + position, Toast.LENGTH_SHORT).show();
            }
        });
    }


    class MyViewHolder extends RecyclerView.ViewHolder {

        private ImageView img;
        private TextView tv;
        //添加最外层的布局属性
        RelativeLayout rlContainer;

        public MyViewHolder(View view) {
            super(view);

            this.img = view.findViewById(R.id.image);
            this.tv = view.findViewById(R.id.tv);
            this.rlContainer = view.findViewById(R.id.rl_item_container);
        }
    }
}
```

---

---

==***感谢您的阅读
如有错误烦请指正***==

---

>  参考：
>
>  1. [26-Android中的列表-RecyclerView_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1hy4y1W7mV/?p=43&spm_id_from=pageDriver)
>  2. [【Android】ListView与RecyclerView基础运用-CSDN博客](https://blog.csdn.net/2301_79344902/article/details/140489649?spm=1001.2014.3001.5502)
>  3. 《第一行代码》
