---
layout: post
title: "Resources remover(리소스 제거)"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/10/01/
---

# 사용법

터미널에 위와 같이 입력하고 실행하면 자동으로 지워준다. 참 편하다.

```reStructuredText
./gradlew removeUnusedResources
```

![](https://github.com/konifar/gradle-unused-resources-remover-plugin/raw/master/art/shell.png)

# 설치

단지, 프로젝트 수준의 build.gradle에 plugins를 추가하면 된다.

```groovy
buildScript {
    ...
}

plugins {
  id "com.github.konifar.gradle.unused-resources-remover" version "0.3.3"
}

allprojects {
    repositories {
        ...
    }
}
```

**주의** : plugins의  <u>반드시</u> buildScript와 allprojects 사이에 추가해줘야 한다. 그렇지 않으면 오류가 발생해 사용할 수 없다.



# 참조

[Unused Resources Remover for Android](https://github.com/konifar/gradle-unused-resources-remover-plugin){:target="_blank"}

[https://plugins.gradle.org/plugin/com.github.konifar.gradle.unused-resources-remover](https://plugins.gradle.org/plugin/com.github.konifar.gradle.unused-resources-remover){:target="_blank"}