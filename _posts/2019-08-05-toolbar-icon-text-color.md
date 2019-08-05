---
layout: post
title: "툴바(Toolbar)의 햄버거 아이콘, 제목 색상 변경"
categories: [Android]
tags: [Android]
---

# Style

```xml
<style name="ToolbarColoredBackArrow" parent="AppTheme">
    <item name="android:textColorSecondary">INSERT_COLOR_HERE</item>
</style>
```

# Layout xml

```xml
<androidx.appcompat.widget.Toolbar
  android:id="@+id/toolbar"
  android:layout_width="match_parent"
  android:layout_height="?attr/actionBarSize"
  android:background="@android:color/white"
  app:popupTheme="@style/AppTheme.PopupOverlay"
  app:theme="@style/ToolbarColoredBackArrow" <!-- here!!! -->
/>
```



배경과 다르게 툴바의 햄버거 버튼이나 타이틀 색상을 변경하고 싶을때가 있다. 이럴 때 간단하게 스타일을 활용하자!

[https://stackoverflow.com/questions/28620883/how-to-change-toolbar-home-icon-color](https://stackoverflow.com/questions/28620883/how-to-change-toolbar-home-icon-color)