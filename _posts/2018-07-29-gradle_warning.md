---
layout: post
title: "Gradle warning"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/07/29/
---

언제부터인지, Proguard가 안된다. 원인을 찾지 못하고 있다. 빌드하면, 8가지 Warning이 뜬다. 문득, Warning이 문제가 아닐까 하는 생각이 든다. Warning부터 하나 하나 처리해본 후 프로가드를 다시 적용해볼 셈이다.

## Firebase Warning

```groovy
Warning: The app gradle file must have a dependency on com.google.firebase:firebase-core for Firebase services to work as intended.

/* 
경고 : app grad 파일에는 com.google.firebase : firebase-core for Firebase 서비스가 의도 한대로 작동하도록해야합니다.
*/
```

위의 경고를 보니 의아하다. Warning이 발생하고 있는 프로젝트는 Firebase의 ads, 와 database만을 사용하기 때문이다. 

혹시 모르니, 아래와 같이 추가했다. 그랬더니, Warning이 사라졌다.... Firebase core는 대체 무엇이란 말인가...

```groovy
implementation "com.google.firebase:firebase-core:$rootProject.firebase_core_version"
```

[파이어베이스 릴리즈 노트](https://firebase.google.com/support/release-notes/android?hl=ko){:target="_blank"}