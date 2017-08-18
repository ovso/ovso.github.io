---
layout: post
title: "MFPDemo 앱 시나리오 코드"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---

## Editor 

```java
앱 실행;
**분석(Analytics)진행**;

if(!**인증&보안**) {
  에러 팝업;
  앱 종료;
  return;
}

if(**앱 버전 확인**) {
  if(업데이트 유도 팝업){
    **앱 센터**실행;
    return;
  }
}

메인 화면;
if(!**로그인**) {
  경고 팝업;
  return;
}
if(**푸시를 위한 모바일 장치 등록**) {
  홈화면;  
} else {
  모바일 장치 등록 재시도;
  홈화면;
}
```

## Preview

[https://drive.google.com/open?id=0B5dhyAAjqlgCT0k4ZG1fNEFsZHM](https://drive.google.com/open?id=0B5dhyAAjqlgCT0k4ZG1fNEFsZHM){:target="_blank"}