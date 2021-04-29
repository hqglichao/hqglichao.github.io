---
layout: post
title: Android 小Tip
date: 2018-12-11 17:16:00 +0800
categories: Android
tag: [View, TouchEvent, Tip]
---

* content
{:toc}


View 点击不穿透                       {#View}
====================================

如果有多层view叠加时，要使点击最上层view不穿透到下一层view。必需要让上层View消耗掉点击事件，也就是在`OnTouchEvent`中`return true`。  
因为这样所有`Touch`事件就都有点击的这个view处理了，不会上传到`parentVew`。所以点击就不会穿透到`parentView`了。  
`xml`代码如下：  
```bash
android:clickable="true"
```

ListView 在padding中绘制               {#ListView}
====================================

`ListView`和`ScrollView`滑动时可以在padding部分内绘制内容
```bash
android:clipToPadding="false"
```

Dialog Style
====================================

在`/res/value/styles.xml`中写入，直接通过`R.style.MyDialog`引用  
```bash
    <style name="MyDialog">
        <item name="android:windowFrame">@null</item> <!-- 边框 -->
        <item name="android:windowIsFloating">false</item> <!-- 是否浮现在activity之上 -->
        <item name="android:windowIsTranslucent">false</item> <!-- 半透明 -->
        <item name="android:windowNoTitle">true</item> <!-- 无标题 -->
        <item name="android:windowBackground">@color/transparent</item> <!-- 背景透明 -->
        <item name="android:windowContentOverlay">@null</item>
        <item name="android:theme">@android:style/Theme.Dialog</item>
    </style>
```

SpannableString
====================================

用`SpannableString`使`TextView`中的某部分字体改变颜色，或添加点击事件  
* 改变颜色  
  ```java
  SpannableString spannableString = new SpannableString(subTitle);
  spannableString.setSpan(new ForegroundColorSpan(color), index, lastInex + 1, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
  tvTextView.setText(spannableString);
  ```
* 部分文字添加点击事件  
  其他和改变颜色相似，只是`setSpan`传入的第一个参数不同：
  ```bash
  spannableString.setSpan(new ClickableSpan(){...}, index, lastInex + 1, Spannable.SPAN_EXCLUSIVE_EXCLUSIVE);
  ```

>使用`databinding`也可以使用`SpannableString`，只要在`ViewModel`里面返回的类型从`String`改成`SpannableString`就可以了。


隐式 Intent
====================================
`隐式Intent`指的是：不会指定特定的组件，而是声明要执行的常规操作，从而允许其他应用中的组件来处理它。  
如启动`activity`时：
```java
    Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.putExtra(Intent.EXTRA_TEXT, sendMsg);
    sendIntent.setType("text/plain");

    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(sendIntent);
    }
```
会弹出一个列表让你选择想要打开的应用，如短信等  
如果你想要成为一个以上的一个过滤器：
```java
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```
>参考：[https://developer.android.com/guide/components/intents-filters](https://developer.android.com/guide/components/intents-filters)

onTaskRemoved
===================================
当`service`关联的`Task`被移除的时候，会调用如下接口：
```bash
public void onTaskRemoved(Intent rootIntent) {
}
```
如果app运行过程中开始了一个`service`，当用户手动杀手app的时候，会回调这个接口，可以在里面处理一些事情。  
`Android O+`程序退后台之后，`service`一分钟后会被杀，会失效。  
>参考：[https://stackoverflow.com/questions/21040339/how-to-know-when-my-app-has-been-killed](https://stackoverflow.com/questions/21040339/how-to-know-when-my-app-has-been-killed)

implementation VS api
====================================
`gradle`中使用`implementation`和`api`的区别主要体现在编译速度，和子引用可不可以在主项目中使用上（`api`声明的主项目中可以用lib里面的引用库内容，而`implementation`声明的不可以）。
>参考文章：[https://medium.com/mindorks/implementation-vs-api-in-gradle-3-0-494c817a6fa](https://medium.com/mindorks/implementation-vs-api-in-gradle-3-0-494c817a6fa)


混淆
===================================
>[https://jebware.com/blog/?p=418](https://jebware.com/blog/?p=418)


Usb Device no permission
===================================
手机连接linux设备电脑，不能操作手机，或者`android studio`安装设备时显示`null`。  
解决方法：  
**打开**
```bash
sudo gedit /etc/udev/rules.d/51-android.rules
```
**添加**  
```bash
SUBSYSTEM=="usb", ATTR{idVendor}=="USB_ID", MODE="0666", GROUP="plugdev"
```
USB_ID：这个值填`lsusb`中显示的对应的设备的"："前的4位数。如`Huawei`是`12d1`。  
>参考：[https://developer.android.com/studio/run/device](https://developer.android.com/studio/run/device)


更改Project名
===================================
1、 先重命名目录，然后导入  
2、 将`*.iml`和其中内容中的老名字替换为新名字  
3、 包目录上右键`Refactor`，重新命名包名

没有存储权限时的缓存
===================================
`getExternalFilesDir`这个在`6.0`以上版本需要**动态请求权限**，而`getCacheDir`这个不需要。
这两个都能获得缓存路径，路径有所不同：
```bash  
getExternalFilesDir：/storage/emulated/** (用于长久储存)
getCacheDir: /data/user/**
```

宽/高自适应Fresco
===================================
```java
public static void loadImageWithAutoHeight(final SimpleDraweeView view, String path, final int imgWidth, final int radius) {
        LogUtil.d(Constants.TAG_V_525 +"path===" + path);
        if (view == null) {
            return;
        }

        ControllerListener<ImageInfo> controllerListener = 
        new ControllerListener<ImageInfo>() {
            @Override
            public void onSubmit(String s, Object o) {

            }

            @Override
            public void onFinalImageSet(String s, 
            @Nullable ImageInfo imageInfo, 
            @Nullable Animatable animatable) {
                if (imageInfo == null) {
                    LogUtil.e(Constants.TAG_V_525 + " error: imageInfo is null.");
                    return;
                }
                int height = imageInfo.getHeight();
                int width = imageInfo.getWidth();
                ViewGroup.LayoutParams layoutParams = view.getLayoutParams();
                layoutParams.width = imgWidth;
                layoutParams.height = 
                (int) ((float)(imgWidth * height) / (float) width);
                view.setLayoutParams(layoutParams);

                if (radius > 0) {
                    RoundingParams roundingParams = 
                    RoundingParams.fromCornersRadius(radius);
                    view.getHierarchy().setRoundingParams(roundingParams);
                }
            }

            @Override
            public void onIntermediateImageSet(String s, 
            @Nullable ImageInfo imageInfo) {

            }

            @Override
            public void onIntermediateImageFailed(String s,
             Throwable throwable) {

            }

            @Override
            public void onFailure(String s, Throwable throwable) {

            }

            @Override
            public void onRelease(String s) {

            }
        };
        Uri uri = Uri.parse(path);
        view.setController(Fresco
                .newDraweeControllerBuilder()
                .setControllerListener(controllerListener)
                .setUri(uri)
                .build());
    }
```

全机型16:9图片适应
===================================
```java
    ViewGroup.MarginLayoutParams layoutParams = (ViewGroup.MarginLayoutParams) itemView.getLayoutParams();
    ViewGroup parentView = (ViewGroup) itemView.getParent();
    int padding = ViewUtils.dip2px(10); // GridView 纵向 Padding, 同 ae_camera_video_story_play_page_view.xml 中的数值;
    int itemWidth = ViewUtils.getScreenWidth() - padding * 2;
    int itemHeight = parentView.getHeight() - padding;

    if (itemWidth * 16 / 9 > itemHeight) {
        itemWidth = itemHeight * 9 / 16;
    } else {
        itemHeight = itemWidth * 16 / 9;
    }
    layoutParams.width = itemWidth;
    layoutParams.height = itemHeight;
    padding += (parentView.getWidth() - itemWidth - padding * 2) / 2; // GridView两边的padding

    if (Log.isDevelopLevel()) {
        Log.d(TAG, QLog.DEV, "one itemHeight " + itemHeight + " itemWidth: " + itemWidth +
        " screenWidth: " + ViewUtils.getScreenWidth() + " viewWidth: " + parentView.getWidth() + " padding: " + padding);
    }

    itemView.setLayoutParams(layoutParams);
    parentView.setPadding(padding, 0, 0, 0);
```

View的animate()使用
===================================
```java
vAnimView.animate().setDuration(200).alpha(0f).start();  
```
可以通过设置时间+动画类型直接开始

AnimatorSet使用
===================================
```java
AnimatorSet animatorSet = new AnimatorSet();
ValueAnimator text1AppearAni = ValueAnimator.ofInt(10, 0);
text1AppearAni.setDuration(1000);
text1AppearAni.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
    }
});
ValueAnimator text2AppearAni = ValueAnimator.ofInt(-22, 0);
text2AppearAni.setDuration(1000);
text2AppearAni.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
    }
});
ValueAnimator dismissAni = ValueAnimator.ofFloat(0f, 1f);
dismissAni.setDuration(1000);
dismissAni.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
    }
});

//串联动画
animatorSet.play(text1AppearAni)
        .with(text2AppearAni)
        .with(resultDividerAppearAni)
        .before(dismissAni);
```


TextView去除上下空边距
===================================
android:includeFontPadding="false"

GridLayoutManager的坑
===================================
GridLayoutManager + RecyclerView在调用notifyItemChange的时候会刷新回顶部
解决方案：
gridLayoutManager.setAutoMeasureEnabled(false);

>参考：https://stackoverflow.com/questions/36724898/notifyitemchanged-make-the-recyclerview-scroll-and-jump-to-up  


替换纯色图标的颜色
===================================
```java
view.setColorFilter(Color.GRAY, PorterDuff.Mode.SRC_IN)
```
