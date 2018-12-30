---
layout: post
title: "네비게이션뷰(NavigationView) 커스텀하기"
description: 네비게이션뷰 커스텀
categories: [Android]
tags: [Android]
---

# 네비게이션뷰란?

<img src="./../images/navigation_view.svg" width="250" />

안드로이드에서 [네비게이션뷰](https://developer.android.com/reference/android/support/design/widget/NavigationView){:trget="_blank"}란, 일반적으로 햄버거 버튼을 누르면 좌측에서 나오는 슬라이드 메뉴를 가리킨다. 안드로이드 스튜디오에서 프로젝트 생성시에 개발자 선택에 따라 자동으로 만들어 주기도 한다.

# 커스텀하는 일반적인 방법

["안드로이드 네비게이션뷰 커스텀" 이라고 구글에서 검색](https://www.google.com/search?newwindow=1&ei=4GEoXJ7RGdjN-Qb0rIGwDg&q=%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C+%EB%84%A4%EB%B9%84%EA%B2%8C%EC%9D%B4%EC%85%98%EB%B7%B0+%EC%BB%A4%EC%8A%A4%ED%85%80&oq=%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C+%EB%84%A4%EB%B9%84%EA%B2%8C%EC%9D%B4%EC%85%98%EB%B7%B0+%EC%BB%A4%EC%8A%A4%ED%85%80&gs_l=psy-ab.3...0.0..2016109...0.0..0.0.0.......0....2..gws-wiz.jkBnCfp-dXo){:target="_blank"}하면 각종 팁들이 나온다. 대부분이 각종 뷰를 NavigationView로 감싸는 방법을 소개하고 있다. 

```xml
<NavigationView 
	...>
    <LinearLayout
		...>
    </LinearLayout>
</NavigationView>
```

# 액션레이아웃을 통한 방법

필자는 이번 여행 관련 앱을 개발하면서 액션레이아웃 사용해봤다. ActionLayout은 [Menu resource의 Item에서 사용하는 속성](https://developer.android.com/guide/topics/resources/menu-resource){:target="_blank"}이다.

res/menu/activity_main_drawer.xml

```xml
<menu xmlns:android="http://schemas.android.com/apk/res/android"
      ...>
    <item
		android:id="@+id/menu_mainnav_bucket"
        app:actionLayout="@layout/item_mainnav_bucket">
    </item>
</menu>
```

res/layout/Item_mainnav_bucket.xml

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	...
	android:orientation="horizontal">
  <TextView .../>
  <ImageView .../>
</LinearLayout>
```

위와 같이 구성했다면 네비게이션뷰에서 제공해주는 [클릭이벤트](https://developer.android.com/reference/android/support/design/widget/NavigationView.OnNavigationItemSelectedListener){:target="_blank"}를 쉽게 구성할 수 있다.

```java
public interface OnNavigationItemSelectedListener {
	boolean onNavigationItemSelected(@NonNull MenuItem item);
}
```

# 일반적인 다른 방법

물론, ActionLayout로 구현하는 방법이 최선을 아니다. 프로젝트의 특성에 따라 달라질 수 있다. 예를 들어, 네비게이션뷰 목록이 수시로 바뀌고 갯수 또한 많아진다면 RecyclerView, ListView를 이용해 구현하는 것이 나을 수도 있다.

# 결론

네비게이션뷰이 목록 수가 정적이라면, 아이템의 레이아웃 구조가 제각각이라면, ActionLayout, ActionViewClass를 사용해보길 권한다.