---
layout: post
title: "Dagger 정보"
description: 
categories: [android]
tags: [Dagger]
redirect_from:
  - /2017/11/09/
---

# Dagger

Dagger가 무엇인지는 구글링하면 다 나온다. 많은 개발자들이 쓰고 있고, 구글샘플에도 필수적으로 사용되고 있다. 나도 얼마 전부터 이를 사용하고 있다. 사용하면 작성 클래스는 늘지만 좀 있어(?) 보인다. 그리고 뭔가 편하다는 것을 조금 느낀다. 각종 문서를 훑어봐도 원리를 제대로 이해하기는 힘들다. 여유를 갖자.(유연성을 갖자)



## @Inject

구글링 하면 @Inject가 뭔지 알 수 있다. class상에서 @Inject의 순서는 중요하다. 초기화(객체 생성)는 선언한 순서대로다.

```java
@Inject Adapter adapter;
@Inject Presenter presenter;
```