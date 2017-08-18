---
layout: post
title: "Mac ADB 환경변수 설정하기”
description: 
categories: [other]
tags: [other]
redirect_from:
  - /2017/08/18/
---

회사 맥북에 환경변수를 설정하기 위해서 두 시간 가량 헤맸다. 고작 [ADB](https://developer.android.com/studio/command-line/adb.html?hl=ko)를 환경변수에 등록하고자 하는데도 말이다. 대표적으로 ADB는 정상동작하는데 ls나 clear가 안되는 문제, 그 반대 문제, 경로 맨 앞에 '/'하나 빠뜨려서 ADB가 안되는 문제가 있었다.

* 먼저 Finder 또는 Terminal에서 홈으로 이동한다.
* 숨겨져 있는 .bash_profile 파일을 열고 아래와 같이 작성하고 저장한다.

```
export PATH=$PATH:/Users/jaeho/Library/Android/sdk/platform-tools
```

* 위의 $PATH에는 아래와 같이 5개의 경로가 설정되어 있다. 콜론(:)이 구분자다. $PATH를 빠뜨리면 ls나 clear같은 명령어가 동작하지 않는다.

```
MacBook-Pro:~ 사용자$ echo $PATH (Enter)
/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin
```

* 터미널 환경에 적용한다.

```
MacBook-Pro:~ 사용자$ source .bash_profile 
```


ADB가 잘 동작하는지 터미널에서 실행해보자!