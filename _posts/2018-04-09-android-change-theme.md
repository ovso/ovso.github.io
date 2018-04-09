---
layout: post
title: "앱 시작할때 테마 바꾸기"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/04/09/
---

# SplashScreen

토이프로젝트를 진행할때, Splashscreen은 심플하게 만들기 마련이다. 테마를 사용하는 것이 가장 심플해 보인다. 필자는, AndroidManifest.xml의 첫 액티비티의 테마를 SplashTheme로 변경한 다음, 화면이 보여지면 다시 원래대로 바꾸는 방법을 선호한다.

가끔이지만 실패할 경우가 있다. 그때는 onCreate에서 테마를 바꾸는 시도를 했는지 확인해보도록 하자. onCreate에서 테마를 바꾸면 반영되지 않는다. **필자는 보통 MVP를 적용하고 Dagger2를 사용하는데, Presenter를 @Inject 하면서 Presenter 생성자에서 테마를 변경**한다.

아래와 같이 작성하면 아주 잘된다. 

```java
// Activity
@Inject MainPresenter presenter;
```

```Java
// Presenter
public MainPresenterImpl(MainPresenter.View view) {
    view.changeTheme();
}
```

#### Dagger2를 사용하지 않는다면

시도해보지는 않았지만, onStart, onResume에서 시도하면 되지 않을까...