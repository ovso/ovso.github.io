---
layout: post
title: "안드로이드 스튜디오 단축키 Command+shift+A 안되는 문제"
categories: [Android Studio]
tags: [Android Studio]
---

# Command + Shift + A

Command + Shift + A 는 내가 자주 사용하는 단축키다. 스튜디오에서 툴 기능을 찾을때 사용하는데 '더블 쉬프트'보다 빠르다.

# 문제

안드로이드 스튜디오 업데이트, MAC OS 업데이트 이후, 안드로이드 스튜디오 단축키를 누르면 창이 뜨면서 사라진다. 그리고 노란색 화면의 터미널 새 창이 뜬다. 새 창으로 뜬 터미널의 이름은 'aprops open'이다.

![](https://i.stack.imgur.com/I2juW.png)

# 원인

시스템 단축키와 겹치면서 안드로이드 스튜디오에서 문제가 발생

# 해결

시스템 -> 키보드 -> 단축키 -> 서비스 -> 터미널에서 main 페이지 인덱스 검색(Command + Shift + A)을 체크해제

![](https://i.stack.imgur.com/lP3Pl.png)





링크 : [https://stackoverflow.com/questions/55443168/apropos-plugin-popup](https://stackoverflow.com/questions/55443168/apropos-plugin-popup)