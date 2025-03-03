#  `ViewPager2` 

## 1. 添加依赖
确保项目的 `build.gradle` 文件中添加了 `ViewPager2` 的依赖：
```groovy
implementation 'androidx.viewpager2:viewpager2:1.0.0'
```

## 2. 布局文件中添加 ViewPager2
在你的布局文件（例如 `activity_main.xml`）中，添加 `ViewPager2` 控件：
```xml
<androidx.viewpager2.widget.ViewPager2
    android:id="@+id/viewPager"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

## 3. 创建适配器
为 `ViewPager2` 创建适配器，适配器可以是 `FragmentStateAdapter` 或 `RecyclerView.Adapter`。

### 使用 FragmentStateAdapter
```java
public class ViewPagerAdapter extends FragmentStateAdapter {

    public ViewPagerAdapter(@NonNull FragmentActivity fragmentActivity) {
        super(fragmentActivity);
    }

    @NonNull
    @Override
    public Fragment createFragment(int position) {
        // 返回不同位置的Fragment
        switch (position) {
            case 0:
                return new Fragment1();
            case 1:
                return new Fragment2();
            default:
                return new Fragment1();
        }
    }

    @Override
    public int getItemCount() {
        // 返回页面数量
        return 2;
    }
}
```

### 使用 RecyclerView.Adapter

如果不使用 Fragment，可以创建自定义的 `RecyclerView.Adapter`：
```java
public class MyVPAdapter extends RecyclerView.Adapter<MyVPAdapter.MyViewHolder> {
    private List<String> mCityList;
    public MyVPAdapter(List<String> cityList) {
        this.mCityList = cityList;
    }


    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_page, parent, false);
        MyViewHolder myViewHolder = new MyViewHolder(view);
        return myViewHolder;
    }

    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        holder.tv1.setText(mCityList.get(position).toString());
    }

    @Override
    public int getItemCount() {
        return mCityList.size();
    }

    class MyViewHolder extends RecyclerView.ViewHolder {
        private TextView tv1;

        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            tv1 = itemView.findViewById(R.id.tvTitle);
        }
    }
}
```

## 4. 设置适配器
在 `Activity` 或 `Fragment` 中，设置 `ViewPager2` 的适配器：
```java
ViewPager2 viewPager = findViewById(R.id.viewPager);
ViewPagerAdapter adapter = new ViewPagerAdapter(this);
// 竖向滑动
viewPager2.setOrientation(ViewPager2.ORIENTATION_VERTICAL);
viewPager.setAdapter(adapter);
```

## 5. 添加页面指示器
如果想要添加页面指示器，可以使用 `TabLayout`：
1. 在布局文件中添加 `TabLayout`：
   ```xml
   <com.google.android.material.tabs.TabLayout
       android:id="@+id/tabLayout"
       android:layout_width="match_parent"
       android:layout_height="wrap_content" />
   ```

2. 在代码中将 `TabLayout` 和 `ViewPager2` 进行绑定：
   ```java
   ViewPager2 viewPager2 = findViewById(R.id.viewPager2);
   TabLayout tabLayout = findViewById(R.id.tablayout);
   
   MyVPAdapter myVPAdapter = new MyVPAdapter(this);
   viewPager2.setAdapter(myVPAdapter);
   
   new TabLayoutMediator(tabLayout, viewPager2, new TabLayoutMediator.TabConfigurationStrategy() {
               @Override
               public void onConfigureTab(@NonNull TabLayout.Tab tab, int position) {
                   switch (position){
                       case 0:
                           tab.setText("主页");
                           break;
                       case 1:
                           tab.setText("我的");
                           break;
                   }
               }
           }).attach();
   ```

> 第一个参数 TabLayout；
>
> 第二个参数 ViewPager2；
>
> 第三个参数 TabConfigurationStrategy，这是一个接口，里面需要实现一个方法，onConfigureTab(@NonNull TabLayout.Tab tab, int position)，这个方法第一个参数是当前的tablayout，第二个是当前的位置。









> 参考：
>
> 1. [ViewPager2+Fragment+TabLayout - 熊猫Panda先生 - 博客园 (cnblogs.com)](https://www.cnblogs.com/lihuawei/p/16642947.html)
> 2. [ViewPager2系列--与TabLayout的结合ViewPager可以让用户通过滑动来浏览不同的页面，而TabLa - 掘金 (juejin.cn)](https://juejin.cn/post/7247733201925783610)
> 3. [【Android】ViewPager2和TabLayout协同使用，实现多Fragment页面切换类似于QQ音乐，bilibili效果_tablayout和viewpager2-CSDN博客](https://blog.csdn.net/m0_72983118/article/details/134407403)