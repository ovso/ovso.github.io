---
layout: post
title: "Material design"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/09/07/
---

# BottomAppBar

![](https://storage.googleapis.com/spec-host-backup/mio-design%2Fassets%2F1xkb-3HQEAsb0IA292GLCziMMh6QE1YIX%2Fanatomy-fab-cut.png)



```groovy
dependencies {
	implementation "com.android.support:design:28.0.0-rc01"
}
```



```xml
<android.support.design.widget.CoordinatorLayout
...
>
  <android.support.design.widget.FloatingActionButton
      android:id="@+id/fab"
      android:layout_width="wrap_content"
      android:layout_height="wrap_content"
      android:layout_gravity="center"
      android:layout_margin="@dimen/fab_margin"
      app:fabSize="normal"
      app:layout_anchor="@id/bottom_app_bar"
      app:srcCompat="@android:drawable/ic_dialog_email"
      />
  <android.support.design.bottomappbar.BottomAppBar
      android:id="@+id/bottom_app_bar"
      android:layout_width="match_parent"
      android:layout_height="?android:actionBarSize"
      android:layout_gravity="bottom"
      app:backgroundTint="@color/colorPrimary"
      app:fabAlignmentMode="center"
      app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
      app:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
      />

</android.support.design.widget.CoordinatorLayout>                                               
```

1. FAB의 layout_anchor에 BottomAppBar의 id를 넣고
2. BAB의 fabAlignmentMode로 FAB의 위치를 설정한다.
3. 반드시 CoordinatorLayout 안에 존재해야 한다.

## 참고

[app-bars-bottom](https://material.io/design/components/app-bars-bottom.html){:target="_blank"}

