---
layout: post
title: "InflateException(MaterialCardView)"
categories: [Android]
tags: [Android]
---

# 예외 발생

Dependencies에 material(com.google.android.material:material:x.x.x)을 추가하고 ViewHolder에서 MaterialCardView를 사용했으나 아래와 같은 예외(InflateException)가 발생했다.

```
android.view.InflateException: Binary XML file line #19: Binary XML file line #19: Error inflating class com.google.android.material.card.MaterialCardView

Caused by: android.view.InflateException: Binary XML file line #19: Error inflating class com.google.android.material.card.MaterialCardView

Caused by: java.lang.reflect.InvocationTargetException
...
..
.
```



# 예외 발생 원인 및 해결

MaterialCardView는 material(com.google.android.material:material:x.x.x)에서 제공한다. material을 사용하려면 Material을 지원하는 테마를 사용해야한다. 필자는 Material을 제공하는 테마를 사용하지 않았다. 따라서 AndroidManifest에서 사용하는 Style의 AppTheme를 수정했다.

### 변경 전

AndroidManifest.xml

```xml
  <!-- Base application theme. -->
  <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <!-- Customize your theme here. -->
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
  </style>
  ...
  ..
  .
```

### 변경 후

AndroidManifest.xml

```xml
  <!-- Base application theme. -->
  <style name="AppTheme" parent="Theme.MaterialComponents.Light.DarkActionBar">
    <!-- Customize your theme here. -->
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
  </style>
  ...
  ..
  .
```



AppTheme의 parent(부모) 테마를 변경하니 이제 잘 동작한다.