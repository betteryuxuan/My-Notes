> `ViewPager` 是 Android 中一个用于在同一屏幕上滑动不同页面（通常是左右滑动）的组件。它通常用于实现多页面滑动效果，比如应用的引导页、图片轮播、以及支持标签导航的界面。

`ViewPager` 与 `PagerAdapter` 结合使用。`PagerAdapter` 是一个适配器，它负责为 `ViewPager` 提供页面内容。每个页面通常是一个 `Fragment`，也可以是一个普通的 `View`。

> 特点：
> 1. **滑动效果**：`ViewPager` 允许用户通过滑动手势在不同页面之间切换。
> 2. **缓存页面**：默认情况下，`ViewPager` 会缓存当前页面的前一页和后一页，以提高 滑动性能。
> 3. **与TabLayout结合**：`ViewPager` 常常与 `TabLayout` 配合使用，实现顶部标签栏导航。

# 一、添加ViewPager控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <androidx.viewpager.widget.ViewPager
        android:id="@+id/viewPager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

# 二、构建适配器类

![image-20240904200221023](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240904200221023.png)

继承`PagerAdapter`并实现4个方法：

`getCount`，`destroyItem`，`instantiateItem`，`isViewFromObject`

```java
public class MyViewPagerAdapter extends PagerAdapter {

    @Override
    public int getCount() {
        return 0;
    }

    @Override
    public void destroyItem(@NonNull View container, int position, @NonNull Object object) {
        super.destroyItem(container, position, object);
    }

    @NonNull
    @Override
    public Object instantiateItem(@NonNull View container, int position) {
        return super.instantiateItem(container, position);
    }

    @Override
    public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
        return false;
    }
}
```

![image-20240904201150661](https://raw.githubusercontent.com/betteryuxuan/Image/main/image-20240904201150661.png)

# 三、在 MainActivity 中设置适配器

## 示例一：图片切换

### 适配器

```java
public class MyViewPagerAdapter extends PagerAdapter {
    List<ImageView> mImageViewList;

    public MyViewPagerAdapter(List<ImageView> mImageViewList) {
        this.mImageViewList = mImageViewList;
    }

    @Override
    public int getCount() {
        return mImageViewList == null ? 0 : mImageViewList.size();
    }


    @Override
    public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
        return view == object;
    }

    @NonNull
    @Override
    public Object instantiateItem(@NonNull ViewGroup container, int position) {
        container.addView(mImageViewList.get(position));
        return mImageViewList.get(position);
    }

    // 该方法需删除super方法，否则会报错闪退
    @Override
    public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
        container.removeView((View) object);
    }
}
```

### MainActivity

```java
public class MainActivity extends AppCompatActivity {
    private MyViewPagerAdapter myViewPagerAdapter;
    private ViewPager viewPager;
    List<ImageView> imageViewsList;

    @Override
    protected void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        viewPager = findViewById(R.id.viewPager);

        initData();
        myViewPagerAdapter = new MyViewPagerAdapter(imageViewsList);
        viewPager.setAdapter(myViewPagerAdapter);
        viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {

            }

            @Override
            public void onPageSelected(int position) {
                Toast.makeText(MainActivity.this, "当前页面为：" + (position + 1), Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onPageScrollStateChanged(int state) {

            }
        });
    }

    private void initData() {
        ImageView imageView1 = new ImageView(this);
        imageView1.setImageResource(R.drawable.p1);
        ImageView imageView2 = new ImageView(this);
        imageView2.setImageResource(R.drawable.p2);
        ImageView imageView3 = new ImageView(this);
        imageView3.setImageResource(R.drawable.p3);

        imageViewsList = new ArrayList<>();
        imageViewsList.add(imageView1);
        imageViewsList.add(imageView2);
        imageViewsList.add(imageView3);
    }
}
```

![Screenrecorder-2024-_-original-original](https://raw.githubusercontent.com/betteryuxuan/Image/main/Screenrecorder-2024-_-original-original.gif)

## 示例二：Fragment切换

### 适配器

与图片类似，`List`存储的为`Fragment`，并且自定义适配器类继承`FragmentPagerAdapter`

只用实现两个方法`getItem`，`getCount`

```java
public class MyFragmentVPAdapter extends FragmentPagerAdapter {
    List<Fragment> myFragmentList;

    public MyFragmentVPAdapter(@NonNull FragmentManager fm, List<Fragment> fragmentList) {
        super(fm);
        this.myFragmentList = fragmentList;
    }

    @NonNull
    @Override
    public Fragment getItem(int position) {
        return myFragmentList == null ? null : myFragmentList.get(position);
    }

    @Override
    public int getCount() {
        return myFragmentList == null ? 0 : myFragmentList.size();
    }
}
```

### Fragment

这里省略部分代码和`fragment`的`layout`，只放了一个`TextView`

这里通过`param1`参数传递信息来设置`fragment`样式

```java
public class VPFragment extends Fragment {

    private static final String ARG_PARAM1 = "param1";
    private static final String ARG_PARAM2 = "param2";

    private String mParam1;
    private String mParam2;

    private TextView textView;
    
    public static VPFragment newInstance(String param1, String param2) {
        VPFragment fragment = new VPFragment();
        Bundle args = new Bundle();
        args.putString(ARG_PARAM1, param1);
        args.putString(ARG_PARAM2, param2);
        fragment.setArguments(args);
        return fragment;
    }

    @Override
    public void onViewCreated(@NonNull View view, @Nullable Bundle savedInstanceState) {
        super.onViewCreated(view, savedInstanceState);
        
        textView = view.findViewById(R.id.tv);
        textView.setText(mParam1);
    }
}
```

### MainActivity

```java
public class MainActivity extends AppCompatActivity {
    // 声明 ViewPager 和自定义 Fragment 适配器
    private ViewPager viewPager;
    private MyFragmentVPAdapter mFragmentVPAdapter;
    List<Fragment> mFragmentList; // 存储 Fragment 的列表

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        // 绑定布局文件中的 ViewPager 控件
        viewPager = findViewById(R.id.viewPager);

        // 初始化 Fragment 数据
        initData();

        // 创建适配器实例并将其设置给 ViewPager
        mFragmentVPAdapter = new MyFragmentVPAdapter(getSupportFragmentManager(), mFragmentList);
        viewPager.setAdapter(mFragmentVPAdapter);

        // 为 ViewPager 添加页面变化监听器
        viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
            @Override
            public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
                // 当页面正在滑动时调用，可以获取滑动的进度
            }

            @Override
            public void onPageSelected(int position) {
                // 当新页面被选中时调用，显示当前页面索引的 Toast
                Toast.makeText(MainActivity.this, "这是碎片" + (position + 1), Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onPageScrollStateChanged(int state) {
                // 当页面滑动状态改变时调用，比如静止、拖动、自动滑动状态
            }
        });
    }

    // 初始化 Fragment 列表的方法
    private void initData() {
        mFragmentList = new ArrayList<>(); // 创建一个存储 Fragment 的列表

        // 创建并初始化多个 VPFragment 实例，每个 Fragment 对应一个页面
        VPFragment vpFragment1 = VPFragment.newInstance("这是碎片1", "");
        VPFragment vpFragment2 = VPFragment.newInstance("这是碎片2", "");
        VPFragment vpFragment3 = VPFragment.newInstance("这是碎片3", "");

        // 将 Fragment 添加到列表中
        mFragmentList.add(vpFragment1);
        mFragmentList.add(vpFragment2);
        mFragmentList.add(vpFragment3);
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
>  1. [36.3-ViewPager结合Fragment_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV13S4y1e7Hq/?p=70&spm_id_from=pageDriver)

```java
2.1
线性表
public class ReverseList {
    public static void reverse(int[] array) {
        int left = 0;
        int right = array.length - 1;
        while (left < right) {
            int temp = array[left];
            array[left] = array[right];
            array[right] = temp;
            left++;
            right--;
        }
    }
}
单链表
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null;
        ListNode cur = head;
        while (cur != null) {
            ListNode temp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
}
2.3
public class Main {
    public static void main(String[] args) {
        int[] array = {1, 2, 3, 4, 3, 5};
        int x = 3;
        int k = 0;
        for (int i = 0; i < array.length; i++) {
            if (array[i] != x) {
                array[k++] = array[i];
            }
        }
    }
}
```

