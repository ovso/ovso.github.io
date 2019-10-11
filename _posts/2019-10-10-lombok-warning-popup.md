---
layout: post
title: "Lombok Requires Annotation Processing(롬복)"
categories: [Android]
tags: [Android]
---

# 롬복 경고 팝업

새로운 팀에 합류했다. 기존 코드가 Java여서 롬복을 함께 쓰고 있다. 안스3.5에서 프로젝트를 열면 아래와 같은 창이 항상 노출된다. 3.x 초반 버전에서는 Annotation Processing enable에 체크인 하여 해결했지만, 이 APE 체크박스가 보이지 않아서 다른 방법으로 해결했다.

![](https://raw.githubusercontent.com/ovso/ovso.github.io/master/images/lombok_warning_01.png)



# 해결

![](https://raw.githubusercontent.com/ovso/ovso.github.io/master/images/lombok_warning_02.png)

이전 안스 버전과는 다르게 Other Settings > Lombok plugin 에서 우측 체크를 모두 없앤 후 프로젝트를 열면 롬복 경고팝업이 뜨지 않는다.