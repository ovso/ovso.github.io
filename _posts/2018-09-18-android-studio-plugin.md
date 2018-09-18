---
layout: post
title: "Android studio plugin"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/09/18/
---

# Resources remover

사용하지 않는 리소스를 제거해야 할때 [Gradle-unused-resources-remover-plugin](https://github.com/konifar/gradle-unused-resources-remover-plugin){:target="_blank"}를 사용하자.

## 설치 및 실행

1. 프로젝트 수준의 build.gradle에 플러그인을 설치한다.

```groovy
buildScript{
   ...
}
plugins {
  id "com.github.konifar.gradle.unused-resources-remover" version "0.3.3"
}
...
```

2. 실행

프로젝트 폴더에서 실행한다.

```
$ ./gradlew removeUnusedResources

[layout] ======== Start layout checking ========
[layout] app
[layout]   Remove i_hot_talk.xml
[layout]   Remove list_item_feed9.xml
[layout]   Remove list_item_feed8.xml
[layout]   Remove common_loading.xml
......
```



마지막으로, 잘~ 지워진 리소스를 확인한다. :)



# Android Resource Usage Count?

리소스 전체라기보다 strings.xml, colors.xml... 등에서 사용하는 갯수를 표시해준다.

![](http://ovso.github.io/images/2017-09-01-01.png)

## 설치

- Android Studio의 Preferences > Plugins 이동
- Resource Usage Count를 검색하여 설치
- Restart!!

