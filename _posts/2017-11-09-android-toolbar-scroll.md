---
layout: post
title: "Toolbar scroll(hide)"
description: 
categories: [android]
tags: [android]
redirect_from:
  - /2017/11/09/
---

# RecyclerView를 사용할때 Toolbar 스크롤

RecyclerViwe를 사용할때 툴바 부분을 함께 스크롤 하고 싶을 때가 있다.

아래와 같이 적용해보자. 

```xml
  <android.support.v7.widget.Toolbar
      ....
      app:layout_scrollFlags="scroll|enterAlways"
      />
```

```Xml
  <android.support.v7.widget.RecyclerView
      ....
      app:layout_behavior="@string/appbar_scrolling_view_behavior"
      />
```

구글링하면 나오는 내용이지만 검색 시간을 줄이기 위해서 기록해 놓는 것이 좋다.



개발을 즐기자~!