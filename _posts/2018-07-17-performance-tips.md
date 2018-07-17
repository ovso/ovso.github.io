---
layout: post
title: "성능 팁"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/07/19/
---

# 성능 팁 몇 가지

## 정적 변수 선언

인수한 프로젝트 빌드를 실패했다. 처음보는 에러다.

```java
static int intVal = 42;
static String strVal = "Hello, world!"
```

위 코드를, 아래와 같이 사용한다.

```java
static final int intVal = 42;
static final String strVal = "Hello, world!"
```

## 손보다 빠른 System API

```java
System.arraycopy(Object src,  int  srcPos, Object dest, int destPos, int length);
```



[참고](https://developer.android.com/training/articles/perf-tips ){:target="_blank"}