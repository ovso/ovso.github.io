---
layout: post
title: "Gradle 캐시 삭제"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/07/09/
---

# Gradle 캐시 삭제

인수한 프로젝트 빌드를 실패했다. 처음보는 에러다.

```
failed to resolve: support-v4
failed to resolve: runtime
...
```



원인은 캐시문제, 캐시를 제거해주니 제대로 빌드됐다.

```
rm -rf $HOME/.gradle/caches/
```



[참고](rm -rf $HOME/.gradle/caches/ ){:target="_blank"}