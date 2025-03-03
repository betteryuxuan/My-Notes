# Banner

## 导入依赖

```groovy
implementation 'io.github.youth5201314:banner:2.2.3'
```

## 布局文件

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <com.youth.banner.Banner
        android:id="@+id/banner"
        android:layout_width="match_parent"
        android:layout_height="300dp" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

## 构建适配器

```java
public class MainActivity extends AppCompatActivity {
    private List<BannerDataInfo> mBannerDataInfoList;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Banner banner = findViewById(R.id.banner);
        mBannerDataInfoList = new ArrayList<>();
        mBannerDataInfoList.add(new BannerDataInfo(R.drawable.a, "云1"));
        mBannerDataInfoList.add(new BannerDataInfo(R.drawable.b, "云2"));
        mBannerDataInfoList.add(new BannerDataInfo(R.drawable.c, "云3"));
        mBannerDataInfoList.add(new BannerDataInfo(R.drawable.d, "云4"));
        mBannerDataInfoList.add(new BannerDataInfo(R.drawable.e, "云5"));

        banner.setAdapter(new BannerImageAdapter<BannerDataInfo>(mBannerDataInfoList) {
                    @Override
                    public void onBindView(BannerImageHolder holder, BannerDataInfo data, int position, int size) {
                        holder.imageView.setImageResource(mBannerDataInfoList.get(position).getImg());
                    }
                })
            // 添加生命周期观察者
                .addBannerLifecycleObserver(this)
            // 圆点指示器
                .setIndicator(new CircleIndicator(this))
            // 轮播间隔时间
                .setLoopTime(4000)
            // 画廊效果
                .setBannerGalleryEffect(20,20);
        
        banner.setOnBannerListener(new OnBannerListener() {
            @Override
            public void OnBannerClick(Object data, int position) {
                Toast.makeText(MainActivity.this, "这是第 " + position + " 张", Toast.LENGTH_SHORT).show();
            }
        });
    }
}
```

> [youth5201314/banner: 🔥🔥🔥Banner 2.0 来了！Android广告图片轮播控件，内部基于ViewPager2实现，Indicator和UI都可以自定义。 (github.com)](https://github.com/youth5201314/banner)

