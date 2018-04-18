---
layout: post
title: "Gradle dependencies exclude(Gradle 의존성 제거)"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/04/16/
---

# Gradle 의존성 제거

## 의존성 문제

모듈 수준의 build.gradle에서 의존성을 제거해야 할 때가 있다. 보통 아래와 같은 메시지와 마주 할 때다.

```
All com.android.support libraries must use the exact same version specification (mixing versions can lead to runtime crashes). Found versions 27.1.1, 26.1.0. Examples include com.android.support:animated-vector-drawable:27.1.1 and com.android.support:customtabs:26.1.0 less... (⌘F1) 
```

서로 다른 모듈에서, 서로 다른 버전의 동일한 모듈을 사용하고 있다는 말이다. 둘 중 하나를 사용하지 못하게 막는 방법으로 해결할 수 있다.

## 의존성 제외

방법은 세 가지 정도가 존재한다. 더 존재할 수도 있다.

### 구성 전반적으로 제외 하는 방법

```groovy
  configurations.all {
    exclude group: 'com.android.support', module: 'customtabs'
  }
```

### 단지, 컴파일에서 제외하는 방법

```groovy
  configurations {
    compile.exclude group: 'com.android.support', module: 'customtabs'
  }
```

### Dependencies에서 제외하는 방법

```groovy
dependencies {
    implementation ('com.android.support:appcompat-v7:27.1.1'){
        exclude group: 'com.android.support', module: 'customtabs'
    }
}
```



## 참고

[https://stackoverflow.com/questions/43382383/gradle-dependencies-exclude](https://stackoverflow.com/questions/43382383/gradle-dependencies-exclude){:target="_blank"}