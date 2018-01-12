---
layout: post
title: "ViewPage와 Tablayout사용"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/01/12/
---

# addTab이 안되는 경우

## 이슈

아래와 같이 tabLayout에 FragmentPagerAdapter가 바인딩 되어 있는 ViewPager를 바인딩 했다면 탭추가(addTab) 수행되지 않는다. 이는 XML에서도 동일한다.

## 원인

이미 Fragment의 갯수대로 TabItem이 생성되어 있기 때문이리라. (개인적 의견)

```java
tabLayout.setupWithViewPager(viewPager);
tablayout.addTab(...) // x
tablayout.addTab(...) // x
```

## 해결,

Fragment 갯수대로 생성된 TabItem에 접근하여 속성을 변경할 수 있다.

```Java
tabLayout.setupWithViewPager(viewPager);
tablayout.getTabAt(0).setText("0")
tablayout.getTabAt(1).setText("1")
```



재밌게 ^___^