---
layout: post
title: "Jeaho's 코딩 컨벤션"
description: 네비게이션뷰 커스텀
categories: [Android]
tags: [Android]
---

# 안드로이드 개발 및 코딩 컨벤션

## 코드 스타일(JAVA)

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



## Lombok

[롬복을 남용한다.](https://projectlombok.org/features/all)



## RxBus 사용

App 클래스에 생성한 RxBus(Singleton)를 참조하여 사용한다.

### RxBus 객체 가져오기

```java
...
private RxBus rxBus;
public void init() {
    rxBus = ((App)getContext().getApplicationContext()).getRxBus();
}
...
```

### RxBus 사용과 RxBusEvent 클래스

RxBus 이벤트를 송출한 곳(클래스)에 RxBusEvent 클래스를 선언한다.

```java
public class MainViewHolder extends ... {
    ...
    public MainViewHolder(View itemView) {
        super(itemView)
    }
    ...
    ...
    public void onClick() {
        rxbus.send(new RxBusEvent(getUrl()))
        //rxbus.send(new RxBusEvent(...))
    }
    ...
    ...
    @Getter @AllArgsConstructor public final static class RxBusEvent {
		private String url;
    }
}
```

### RxBus 관찰

```java
public class MainPresenterImpl implements MainPresenter {
    private RxBus rxBus;
    ...
    public MainPresenterImpl(MainArguments args) {
        ...
		rxBus = args.getRxBus();
        ...
    }
	public void onCreate() {
    	...
    	toRxBusObservable();
    }
    
    private void toRxBusObservable() {
        rxBus.toObservable.subscribe( o -> {
            if(o instanceof MainViewHolder.RxBusEvent) {
                // Do something
            } else if(o instanceof ...) {
                // Do something
            }
        });
    }
}
```



## 주석

* **주석 따위 생략한다.**

* 아키텍쳐(MVP), 변수명, 메서드명 완벽해지도록 노력하면 주석 따위 신경 안써도 된다.



## 리스트 UI

### Vertical(세로)

리싸이클러뷰를 사용한다. **뷰 홀더에 가로스크롤 UI를 구현해야 한다면 HorizontalScrollView를 사용하자.**

### Horizontal(가로)

니맘대로.



## 리스트 Divider

* 리싸이클러뷰의 Divider는 오픈소스를 사용한다.

* ScrollView, HorizontalScrollView, TabLayout는 Divider 옵션을 사용한다.
* 그림자가 있는 리스트라면 Margin으로 조절한다.



## 리소스명

아래의 규칙을 따른다.

모호 할 때가 있다. 예를 들어, 메인화면 > 뷰홀더 > 아이템... 일때 **동료와 상의하자.**

![](https://ovso.github.io/images/resource_naming.png)



## 패키지 구성

![](https://ovso.github.io/images/mvp_package.png)

공용 클래스를 만들었는데 어디에 놓아야 할지 모르겠다면, 동료와 상의하자.



## 데이터 모델링

모델링이 반드시 필요한 곳이 아니라면, 되도록 모델링을 하지 않고 원본 객체(Json 또는 연동객체)를 사용한다.



## 리팩토링

* 할 수 있는 시간이 없다. 당장하자.
* 생각나면 틈을 내서 하자.