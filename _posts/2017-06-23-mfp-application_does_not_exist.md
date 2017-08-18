---
layout: post
title: "APPLICATION_DOES_NOT_EXIST"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---


# 현상

로그인시 에러코드가 APPLICATION_DOES_NOT_EXIST, 에러미시지가 Application doesn't exist라고 나왔다.

로그인 할 때 Adapter와 연동한다.

# 원인

원인은, build.gradle(Module) > versionName을 바꿔서 빌드했기 때문이다. 

앱을 콘솔서버에 배치할때 버전네임이 배치된 앱을 구분하는 역할을 한다. 버전을 바꾸면 당연히 배치된 앱을 찾을 수가 없다.

또 다른 원인,

콘솔 서버에 앱이 등록되어 있지 않을때도 이런 현상이 나타난다.

# 해결

build.gradle(Module)의 versionName을 콘솔서버에 배치된 앱과 동일하게 맞추던지, 앱을 다시 배치하면 된다. 

앱을 재배치 하게 되면, 등록된 앱이 버전별로 나뉜다.

[*stackoverflow*](https://stackoverflow.com/questions/40116118/application-does-not-exist-for-mobilefirst-8-0-security-authentication){:target="_blank"}

