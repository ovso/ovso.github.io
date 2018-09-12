---
layout: post
title: "View Animate"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/09/12/
---

# View change animate

애니메이션을 적용하고 싶은 뷰(View)를 감싸고 있는 레이아웃에 아래와 같은 설정을 적용하면 View가 View.GONE, View.VISIBLE, 기타 움직임이 있을때 기본 애니메이션이 동작한다.

```xml
<ViewGroup
...
>
android:animateLayoutChanges="true"
</ViewGroup>
```

대체로, LinearLayout, FrameLayout 등에 위와 같은 설정을 적용한다.