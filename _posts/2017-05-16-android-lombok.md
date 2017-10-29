---
layout: post
title: "Android Lombok"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/10/29/
---

최근, 기억력 향상 관련 앱 1차 버전을 만들면서 [Lombok](https://projectlombok.org/) 라이브러리를 사용했다. [Lombok](https://projectlombok.org/) 은 Java를 사용하면서 지루하고 불편했던 Setter/Getter를 어노테이션으로 대체한다. Setter/Getter를 사용하지 않는 것은 일부 기능이며, [Lombok](https://projectlombok.org/) 은 [다른 유용한 기능들을 포함](https://projectlombok.org/features/index.html)한다.

Lombok을 사용하려면 Module수준의 build.gradle에 라이브러리를 추가하고, 안드로이드 스튜디오에 플러그인을 설치해야 한다.

### build.gradle(Module)

```groovy
dependencies {
    compile "org.projectlombok:lombok:1.16.18"
}
```

### 안드로이드 스튜디오 플러그인 설치(Preferences)

![](https://ovso.github.io/images/2017-05-16-01-lombok.png)



### Annotation Processing 확인(Default Preferences)

![](https://ovso.github.io/images/2017-05-16-02-lombok.png)



### 사용 예

```java
@Data
public class KeyPoint {
  	private String subject;
  	private String content;
}
```

```java
KeyPoint keyPoint = new KeyPoint();
String subject = keyPoint.getSubject();
String content = kyePoint.getContent();
```



## 문제 해결

### 빌드에러 1

```
cannot find symbol class Generated 또는 package javax.annotation does not exist
```

### 빌드에러 1 해결(모듈 수준 build.gradle)

```groovy
dependencies {
 provided 'javax.annotation:jsr250-api:1.0'
}
```

### 빌드에러 2

```
cannot find symbol class ConstructorProperties
```

### 빌드에러 2 해결

lombok.config 파일을 프로젝트 루트(Root)에 생성 후, 아래와 같이 설정

```
lombok.anyConstructor.suppressConstructorProperties = true
```

##### 

### 기타

*[Configuration system 참고](https://projectlombok.org/features/configuration.html){:target="_blank"}*

