---
layout: post
title: "Error:Conflict with dependency 'com.google.code.findbugs:jsr305.."
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/10/15/
---

# 빌드에러 발생

라이브러리를 사용하면서 아래와 같은 문제와 마주했다. 3.0.1 버전과 2.0.1 버전이 충돌한다는 얘기다.

```
Error:Conflict with dependency 'com.google.code.findbugs:jsr305' in project ':app'. Resolved versions for app (3.0.1) and test app (2.0.1) differ. See http://g.co/androidstudio/app-test-app-conflict for details.
```

# 해결

아래의 방법은, Dependency에 명시한(또는 라이브러리 내에 명시되어 있는) 버전과 관계없이 모든 종속성에 대해, 명시한 버전 번호만 컴파일하도록 강요한다.

```groovy
android {
  ...
  configurations.all {
    resolutionStrategy.force 'com.google.code.findbugs:jsr305:3.0.2'
  }
  ...
}
```



화이팅!!