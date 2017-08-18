---
layout: post
title: "Android Hugo"
description: 
categories: [Android]
tags:[Android]
redirect_from:
  - /2017/08/18/
---

종종 테스트 프로젝트 작성하거나 분석할 때 Log.d(TAG, "") 하는게 귀찮을 때가 있다. 그럴 땐 [Hugo](https://github.com/JakeWharton/hugo)를 사용해보자. gradle 설정 방법은 메인페이지 보다 [hugo-example](https://github.com/JakeWharton/hugo/blob/master/hugo-example/build.gradle)를 참고 하는게 그나마(?) 낫다. 메인페이지를 따라 하면 하면 오류를 발생하기 때문이다. 

#### build.gradle(Module)

```groovy
apply plugin: 'hugo'
```

#### build.gradle(Project)

```groovy
buildscript {
  ...
  dependencies {
    ...
	classpath 'com.jakewharton.hugo:hugo-plugin:1.2.1'
  }
}
```