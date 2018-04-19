---
layout: post
title: "CompilationFailedException"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/04/16/
---

# CompilationFailedException

```
org.gradle.api.internal.tasks.compile.CompilationFailedException: Compilation failed; see the compiler error output for details.
```

## 발견

[Dagger](https://google.github.io/dagger/){:target="_blank"} 를 사용하다 발견했다.

## 원인

Gradle이 버전업 하면서 뭔가 좀더 엄격해진것 같다. [Dagger](https://google.github.io/dagger/){:target="_blank"} 때문에 발생한 것은 아닌것 같다. [@Singleton](https://docs.oracle.com/javaee/7/api/javax/inject/Singleton.html){:target="_bank"}의 선언 위치를 틀렸더니 발생했다. 반드시 [Dagger](https://google.github.io/dagger/){:target="_blank"}가 아니더라도 어디서든 발생할 여지는 충분하다. [구글링 해보면](https://www.google.co.kr/search?newwindow=1&ei=QZHYWtmvD8eK8wW5oqDwDQ&q=android+CompilationFailedException&oq=android+CompilationFailedException&gs_l=psy-ab.3..35i39k1j0.1604.3869.0.4226.14.12.1.0.0.0.170.1085.0j9.9.0....0...1c.1j4.64.psy-ab..4.4.519...0i7i30k1j0i13k1j0i13i30k1j0i7i10i30k1j0i13i5i30k1.0.kKX5cBfGVK0){:target="_blank"} 발생하는 부분들은 다양하다.

## 해결

API 잘 사용하자.... 헉;;