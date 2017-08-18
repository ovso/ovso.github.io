---
layout: post
title: "Android Firebase Proguard Error"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/08/18/
---

쎄미프로젝트에서 파이어베이스 데이터베이스를 사용했다. 프로가드를 적용하고 설치 후 실행하니.. 문제가 발생했다.

## 재현경로

(프로가드 적용 후) 파이어베이스 연동 후 데이터를 MainItem 클래스에 매핑하는 순간

## 문제발생

```
E/AndroidRuntime: FATAL EXCEPTION: main
                                                   Process: io.github.ovso.xxx, PID: 29778
                                                   com.google.firebase.database.c: No properties to serialize found on class io.github.ovso.xx.MainItem
                                                       at com.google.android.gms.c.gp$a.<init>(Unknown Source)
                                                       at com.google.android.gms.c.gp.a(Unknown Source)
                                                       at com.google.android.gms.c.gp.e(Unknown Source)
                                                       at com.google.android.gms.c.gp.b(Unknown Source)
                                                       at com.google.android.gms.c.gp.a(Unknown Source)
                                                       at com.google.firebase.database.a.a(Unknown Source)
                                                       at io.github.ovso.xx.MainActivity$2.a(Unknown Source)
                                                       at com.google.android.gms.c.dh.a(Unknown Source)
                                                       at com.google.android.gms.c.eh.b(Unknown Source)
                                                       at com.google.android.gms.c.ek$1.run(Unknown Source)
                                                       at android.os.Handler.handleCallback(Handler.java:739)
                                                       at android.os.Handler.dispatchMessage(Handler.java:95)
                                                       at android.os.Looper.loop(Looper.java:148)
                                                       at android.app.ActivityThread.main(ActivityThread.java:5417)
                                                       at java.lang.reflect.Method.invoke(Native Method)
                                                       at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:726)
                                                       at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:616)
```

## 원인

MainItem에서 직렬화 할 속성이 없다는 이유...

해결책은 두 가지다.

## Proguard로 해결

```
-keep class io.github.ovso.xx.MainItem { *; }
```

또는

```java
@Keep public class MainItem {
  private String url;
  private String subject;
  private int position = -1;
}
```

## 직렬화(serialize) 할 수 있도록 해결

```Java
public class MainItem {
  public String url;
  public String subject;
  public int position = -1;
}
```

