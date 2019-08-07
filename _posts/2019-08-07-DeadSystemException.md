---
layout: post
title: "DeadSystemException"
categories: [Android]
tags: [Android]
---

# DeadSystemException

Firebase Crashlytics를 보다가 아래와 같은 예외를 발견했다.

```
Fatal Exception: java.lang.RuntimeException
android.os.DeadSystemException
android.app.ActivityThread.handleSleeping (ActivityThread.java:4522)
android.app.ActivityThread.access$2400 (ActivityThread.java:237)
android.app.ActivityThread$H.handleMessage (ActivityThread.java:1884)
android.os.Handler.dispatchMessage (Handler.java:106)
android.os.Looper.loop (Looper.java:214)
android.app.ActivityThread.main (ActivityThread.java:7045)
java.lang.reflect.Method.invoke (Method.java)
com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run (RuntimeInit.java:493)
com.android.internal.os.ZygoteInit.main (ZygoteInit.java:965)
Caused by android.os.DeadSystemException 
```



정확한 원인은 알 수 없으나, 안드로이드 시스팀에 예외를 발생시켰고, 모든 앱이 즉지 종료된다고 한다.

```java
The core Android system has died and is going through a runtime restart. All running apps will be promptly killed.
//핵심 Android 시스템이 종료되었으며 런타임 재시작이 진행 중입니다. 실행중인 모든 앱이 즉시 종료됩니다.
```



7.0 이상의 버전에서 발생한다고 한다.

[https://codeday.me/jp/qa/20190312/379432.html](https://codeday.me/jp/qa/20190312/379432.html)



그리고 우리들의 코드와는 무관해 보인다.



# 그러나,

아래와 같이 재현이 가능하다는 글도 보인다.

#### Oreo 알림 기능 : Android 전화를 다시 시작할 수있는 중요한 문제입니다.

[https://proandroiddev.com/oreo-notification-feature-a-critical-issue-that-could-restart-your-android-phone-ca122fa4d9cb](https://proandroiddev.com/oreo-notification-feature-a-critical-issue-that-could-restart-your-android-phone-ca122fa4d9cb) 

위 링크 글을 읽어보면, [DeadSystemException](https://developer.android.com/reference/android/os/DeadSystemException)은 특정 폰에서 Notification 기능을 사용할때 발생한다고 말하고 있다. 소수의 특정 폰들이다. Nexus5X에서 발생했는데 createNotification()을 호출할때 발생했다고 한다.



DeadSystemException이 발생하기 전에 다른 예외가 발생한다는 현상과 함께, setVibrationPattern이 문제의 근원이라고 지적하고 있다.

### 문제를 방지하는 방법으로..

setVibrationPattern을 사용하지 않고 null을 보내면 된다고...

### 버그는..

LG Nexus5X

모토로라 모토 Z2

LG G7(LM-G710)

LG V30(H932)

LG G6(H870)

Android 8.0.0을 사용하는 모든 기기..