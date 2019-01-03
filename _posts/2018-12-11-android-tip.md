---
layout: post
title: Android 小Tip
date: 2018-12-11 17:16:00 +0800
categories: Android
tag: [View, TouchEvent]
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
  ```bash
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
```bash
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
```bash
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```
>参考：[https://developer.android.com/guide/components/intents-filters](https://developer.android.com/guide/components/intents-filters)