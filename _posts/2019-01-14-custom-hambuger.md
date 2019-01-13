---
layout: post
title: "햄버거 아이콘 애니메이션 막기(DrawerArrowStyle)"
description: 햄버거 아이콘 커스텀
categories: [Android]
tags: [Android]
---

DrawerLayout을 이용해 NavigationView를 구현하면 햄버거 버튼이 자동으로 생성된다. 기본적으로, NavigationView가 열리거나 닫힐때 햄버거 버튼의 애니메이션이 동작한다. NavigationView가 상단 전체를 덮으면 햄버거 버튼의 애니메이션이 부자연스럽다. 애니메이션을 없애는게 더 좋겠다.

# DrawerArrowStyle

햄버거 버튼을 커스텀할 때 사용하는 것이 DrawerArrowStyle이다.

아래는 styles.xml에 정의한 액티비티의 테마다. 가장 아래 drawerArrowStyle에 주목하자.

```xml
...
  <style name="AppTheme.NoActionBar">
    <item name="windowActionBar">false</item>
    <item name="windowNoTitle">true</item>
    <item name="android:statusBarColor">@android:color/transparent</item>
    <item name="drawerArrowStyle">@style/DrawerArrowStyle</item>
  </style>
...
```

같은 styles.xml이다. spinBars의 값을 false로 주니 애니메이션이 멈추었다. 햄버거의 컬러도 바꾸었다.

```xml
...
  <style name="DrawerArrowStyle" parent="@style/Widget.AppCompat.DrawerArrowToggle">
    <item name="spinBars">false</item>
    <item name="color">@android:color/darker_gray</item>
  </style>
...
```

# 참고

[ActionBarDrawerToggle](https://developer.android.com/reference/android/support/v7/app/ActionBarDrawerToggle)