---
layout: post
title: "TextView Drawable Left"
description: 네비게이션뷰 커스텀
categories: [Android]
tags: [Android]
---

# 텍스트뷰 좌측에 아이콘 넣기

```xml
<TextView 
	...
    android:text="아이콘 넣기"
    android:drawableLeft="@drawable/icon_circle"
    ...
/>
```

단순하다. 부모뷰를 만들고 사용하는 것보다 훨씬 효율적이다. 그러나 단순히 선언만 한다면 drawable 크리를 조절을 할 수 없다.

# 아이콘 크기 조절

res/drawable/all_drawabledot_7dp.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">

  <item
      android:drawable="@drawable/icon_circle"
      android:height="7dp"
      android:width="7dp"
      />
</layer-list>
```

## 크기 조절한 drawable을 텍스트뷰에 적용

```xml
<TextView 
	...
    android:text="아이콘 넣기"
    android:drawableLeft="@drawable/all_drawabledot_7dp"
    android:drawablePadding="10dp"
    ...
/>
```

이렇듯 layer-list를 사용한 drawable를 사용하면 원하는대로 아이콘 크기 조절이 가능하다. 글자와 아이콘 사이의 간격이 있어야 한다면, drawablePadding 값을 넣어주면 된다.