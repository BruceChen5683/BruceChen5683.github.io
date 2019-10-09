##### 1. 简介

```
使用简答一行搞定Picasso.get().load("http://i.imgur.com/DvpvklR.png").into(imageView);
可在adapter中回收、取消下载
复杂图片只需少量内存
自动内存、硬盘缓存
```

##### 2. 功能

```
下载展示：
adapte中 Picasso.get().load(url).into(view);

减少内存占用：
Picasso.get().load(url).resize(50,50),centerCrop().into(imageView);

复杂转化：
public class CropSquareTransformation implements Transformation {
  @Override public Bitmap transform(Bitmap source) {
    int size = Math.min(source.getWidth(), source.getHeight());
    int x = (source.getWidth() - size) / 2;
    int y = (source.getHeight() - size) / 2;
    Bitmap result = Bitmap.createBitmap(source, x, y, size, size);
    if (result != source) {
      source.recycle();
    }
    return result;
  }

  @Override public String key() { return "square()"; }
}
Pass an instance of this class to the transform method.

不同加载方式：
icasso.get().load(R.drawable.landing_screen).into(imageView1);
Picasso.get().load("file:///android_asset/DvpvklR.png").into(imageView2);
Picasso.get().load(new File(...)).into(imageView3);
```

##### 3. 调试指示
```
Call setIndicatorsEnabled(true) on the Picasso instance.
网络红色，硬盘蓝色，内存绿色
```