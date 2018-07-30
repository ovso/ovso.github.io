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

## SystemUtils.java: uses or overrides a deprecated API

**더이상 사용되지 않는 API를 사용하거나 대체합니다.**

이 Warning과 더불어 나온 Warning은 아래와 같다.

```groovy
SystemUtils.java:
uses or overrides a deprecated API.
Recompile with -Xlint:deprecation for details.
Some input files use unchecked or unsafe operations.
Recompile with -Xlint:unchecked for details.
```

막상 SystemUtils.java를 열어보면 어떤 API가 Deprecated 인지 알 수 없다. Deprecated 기호가 나타나지 않기 때문이다.

그래서 아래와 같이 모듈 수준의 build.gradle에 작성하고 빌드를 해야 정확히 알 수 있다.

```groovy
  gradle.projectsEvaluated {
    tasks.withType(JavaCompile) {
      options.compilerArgs << "-Xlint:unchecked" << "-Xlint:deprecation"
    }
  }
```

빌드를 마치면 이제 좀 더 정확한 Warning이 나타난다. Warning 수는 당연히 기존보다 늘었다.!!

그러나, 정확히 어떤 Java파일이 문제이고, 어떤 라인, 어떤 코드가 문제인지 정확하게 짚어준다. 고맙다!!

```groovy
...
    
[unchecked] unchecked conversion required: BaseAdapterDataModel<SelectableItem<Theme>>
found:    BaseAdapterDataModel
[unchecked] unchecked cast
required: ArrayList<Object>
found:    Object
[unchecked] unchecked cast
required: ArrayList<Object>
found:    Object

...
```

## Generate APK, Proguard 등에 영향이 미치지 않는다면

```java
@SuppressWarnings("unchecked") // 이런 식으로.. 처리하자
```

이제, 즐겁게 개발하자 :))