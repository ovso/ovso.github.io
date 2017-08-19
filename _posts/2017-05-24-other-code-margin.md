---
layout: post
title: "IntelliJ(Android Studio)에서 Code Margin 사용하기"
description: 
categories: [other]
tags: [other]
redirect_from:
  - /2017/08/18/
---

## Margin

Android Studio를 사용하다 보면 코딩화면 우측에 세로 줄을 볼 수 있다. 클린코드 작성에 도움을 주기 위해 제공하는 것으로 생각 된다. 이 세로 줄의 위치는 아래와 같이 환경설정에서 조정 가능하다.

![](https://ovso.github.io/images/2017-05-24-code-margin-01.png)

Right margin의 숫자를 바꾸고 Apply를 누르면 세로 줄의 위치가 이동하는 것을 확인할 수 있다. 기본값은 '100'이다. 우측에 보면 'Wrap on typing' 체크박스가 있는데 체크 할 경우 코드를 작성하는 중에 줄이 바뀐다. 필자의 경우 코딩하는데 혼란을 줄 수 있다는 생각에 체크해제(off) 하여 사용한다. 기본값은 체크해제(off)다.



## Code Reformat/Alignment

코드정렬 단축키는 'option+command+L'이다. 코드가 세로줄을 넘어가지 않게 하기 위해, 필자는 실시간 정렬(Wrap on typing)보다 타이핑한 후 정렬하는 방법을 선호한다. 단축키를 이용한 코드정렬은 Margin을 설정한 것만으로는 동작하지 않는다. 환경설정의 'Code Style > Java > Wrapping and Braces' 에서 Ensure Right margin is not exceeded 에 체크를 해줘야 한다.

![](https://ovso.github.io/images/2017-05-24-code-margin-02.png)

체크했다면 Apply 한 후, 테스트해보자.