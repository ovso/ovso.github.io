---
layout: post
title: "Jeaho's 코딩 컨벤션"
description: 네비게이션뷰 커스텀
categories: [Android]
tags: [Android]
---

## 코드 스타일(Java)

안드로이드 스튜디오의 [코드 스타일은 Square사의 SquareAndroid](https://github.com/square/java-code-styles)로 한다.

## 생성자 매개변수

클래스 생성자에 넘겨야 할 매개변수가 3~4개 이상이면 클래스화 한다.

```java
public class Presenter {
    public Presenter(PresenterArgs args) {
        ...
    }
}
```

매개변수 클래스를 만들때는 빌더패턴을 사용한다.

```java
PresenterArgs args = new PresenterArgs.Builder()
    .setView(..)
    .setVoiceMain(...)
    .setResourceProvider(...)
    .build();
Presenter presenter =  new Presenter(args);
```

## RxBus 사용

App 클래스에 생성한 RxBus(Singleton)를 참조하여 사용한다.

#### RxBus 객체 가져오기

```java
...
private RxBus rxBus;
public void onCreate() {
    rxBus = ((App)getContext().getApplicationContext()).getRxBus();
}
...
```

#### RxBus 사용하기

```java
...
public void onClick() {
    rxbus.send(new RxBusEvent(...))
}
...
```

#### RxBusEvent 클래스 선언

