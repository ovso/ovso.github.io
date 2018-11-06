---
layout: post
title: "TabLayout에 구분선(Divider) 넣기"
description: 
categories: [Android]
tags: [Opensource]
redirect_from:
  - /2018/10/10/
---

# TabLayout에 구분선 넣기

의외로 쉽다. 아래와 같이 코딩해주면 TabLayout의 Tab 사이에 구분선이 들어간다. 사이즈, 패딩이 조절되니 참 편한다.

```java
LinearLayout linearLayout = (LinearLayout)tabLayout.getChildAt(0);
linearLayout.setShowDividers(LinearLayout.SHOW_DIVIDER_MIDDLE);
GradientDrawable drawable = new GradientDrawable();
drawable.setColor(ContextCompat.getColor(this, R.color.color_9C979797));
drawable.setSize(1, 1);
linearLayout.setDividerPadding(40);
linearLayout.setDividerDrawable(drawable);
```

tabLayout.getChildAt(0)을 LinearLayout으로 캐스팅하고 있는 것을 알 수 있다. [TabLayout은 HorizontalScrollView의 자식](https://developer.android.com/reference/com/google/android/material/tabs/TabLayout){:target="_blank"}이다. HorizontalScrollView를 썻다는 것은 자식뷰가 LinearLayout으로 되어 있다는 것을 짐작 할 수 있다.

실제로, TabLayout을 뒤져보면 LinerLayout의 자식인, SlidingTabIndicator라는 비공개 클래스를 찾을 수 있고, 이 클래스에 addView하는 것을 알 수 있다.

```java
private class SlidingTabIndicator extends LinearLayout {
    ...
}

private void addTabView(TabLayout.Tab tab) {
    TabLayout.TabView tabView = tab.view;
    this.slidingTabIndicator.addView(tabView, tab.getPosition, params)
}

```



## 그럼, LinearLayout에 아무거나 넣고 구분선을 넣을 수 있을까?

그렇다!! 아래와 같이 XML을 작성하고 Divider 로직을 넣어주면 되겠다.

```xml
<LinearLayout
	...
	android:orientation="vertical"> // or "horizontal"
    <TextView ... />
    <TextView ... />
    <TextView ... />
</LinearLayout>
```

