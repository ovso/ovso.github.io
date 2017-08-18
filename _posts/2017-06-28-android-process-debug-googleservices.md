---
layout: post
title: "No matching client found for package name"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/08/18/
---

## 문제

```
Error:Execution failed for task ':app:processDebugGoogleServices'.
> No matching client found for package name 'io.github.ovso.xxx'
```

쎄미 프로젝트 완성에 가까워졌을 때, 패키지명(그리고 applicationId)이 바꾸고 싶어졌다. 설계가 없었기 때문에 하나의 패키지에 모든 클래스가 모두 포함되어 있었다. 클래스 수가 적었기 때문에 수동으로 모두 바꿨는데 위와 같은 빌드 문제가 발생했다.

## 원인 및 해결

###### "app:processDebugGoogleServices"

이와 같은 내용으로 보아, 필자가 사용한 파이어베이스와 관련됨을 짐작했다.

###### google-services.json

위의 파일을 열어 패키지 명을 찾아 변경해주니 해결되었다.