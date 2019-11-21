---
layout: post
title: "Waiting for debugger"
categories: [Android]
tags: [Android]
---

# 문제

<img src="https://raw.githubusercontent.com/ovso/ovso.github.io/master/images/waiting-for-debugger.png" style="zoom:20%;" />

AVD에 앱을 빌드할때마다(Debug 'app' or Run 'app') Waiting For Debugger 팝업이 뜬다. 문제는 이 팝업이 계속 떠 있다는 것이다. 더 이상의 진행은 되지 않는다. 

# 원인

모음

# 해결

StackOverflow를 떠돌다 보면 Android Studio를 종료한 후 다시 시작하면 해결된다는 말이 있었지만 필자는 그렇지 않았다. AVD를 재시작 하니 정상 동작 했다. 혹시 개발폰에서 Waiting For Debugger 팝업이 뜬다면, Android Studio를 재시작할게 아니라 개발폰을 재시작 하면 될것이다.