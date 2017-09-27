---
layout: post
title: "Invalid drawable added to LayerDrawable! Drawable already belongs to another owner but does not expose a constant state"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/09/27/
---

FloatingActionButton의 배경을 바꾸기 위해 **android:backgroundTint**를 사용했다. 배경색은 잘 나왔으나 아래와 같은 경고 에러가 발생했다. 그러나 에러만 발생한다. <u>동작하는데는 전혀 문제가 없다.</u>

# 경고 에러(충돌)

```
Invalid drawable added to LayerDrawable! Drawable already belongs to another owner but does not expose a constant state.
// 잘못된 drawable이 LayerDrawable에 추가되었다. Drawable은 이미 다른 소유자에 속하지만 일정한 상태를 노출하지 않는다.
```

# 원인

*잘 모름*  ㅡ,,ㅡ^

backgroundTint는 Android API 21에서 추가되었는데, 개발중인 앱은 21이상을 지원한다. 레이아웃 xml상에서 app:backgroundTint를 사용하던, android:backgroundTint를 사용하던 상관 없어야 하지 않을까? 

# 해결

```
android:backgroundTint ----> app:backgroundTint
```

즐겁게 개발자자 :)