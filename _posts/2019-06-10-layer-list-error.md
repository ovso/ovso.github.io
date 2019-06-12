---
layout: post
title: "layer-list error(SplashActivity)"
categories: [Android]
tags: [Android]
---

# 에러

SplashActivity 화면을 만드는데 아래와 같은 에러가 발생했다. 이 에러로 수 시간을 낭비했다.

```
java.lang.RuntimeException: Unable to start activity ComponentInfo{io.github.ovso.globaltrend/io.github.ovso.globaltrend.view.ui.splash.SplashActivity}: android.content.res.Resources$NotFoundException: Drawable io.github.ovso.globaltrend:drawable/splash_background with resource ID #0x7f0800b3
....
...
..
.
Caused by: android.content.res.Resources$NotFoundException: Drawable io.github.ovso.globaltrend:drawable/splash_background with resource ID #0x7f0800b3
Caused by: android.content.res.Resources$NotFoundException: File res/drawable/splash_background.xml from drawable resource ID #0x7f0800b3
....
...
..
.
Caused by: org.xmlpull.v1.XmlPullParserException: Binary XML file line #5: <bitmap> requires a valid 'src' attribute
```

# 원인

Splash 화면을 구성하고 있는 아래의 xml이다. 문제가 없어 보인다. 그러나 __@drawable/trending_up__또는 __bitmap__태그가 문제다. __trending_up__은 __vector__이미지다. 때문에 __bitmap__태그에 들어가면 안된다. __vector__를 사용하려면 __item__태그를 사용해야 한다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@color/colorLauncherIconBg"/>
  <item>
    <bitmap
        android:gravity="center"
        android:src="@drawable/trending_up"/>
  </item>
</layer-list>
```



# 해결 방법 두 가지

### 1

__bitmap__ 태그를 사용하지 않고 __item__태크에 직접 __drawable과 gravity__를 설정하면 에러가 발생하지 않는다. 그렇지만 하위 버전, 예를 들어 롤리팝에서는 아이콘 이미지가 깨진다. 화면에 꽉찰 정도로 커진다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  // 배경 레이어
  <item android:drawable="@color/colorLauncherIconBg"/>
  // 아이콘 레이어
  <item
      android:drawable="@drawable/trending_up"
      android:gravity="center"/>
</layer-list>
```

### 2

__bitmap__태그를 사용하자. 대신 이미지는 반드시 __bitmap(xx.png)__를 사용하자.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@color/colorLauncherIconBg"/>
  <item>
    <bitmap
        android:gravity="center"
        android:mipMap="true"
        android:src="@mipmap/ic_launcher_round"/>
  </item>
</layer-list>
```

# 주의

__mipmap-anydpi-v26__ 리소스 폴더가 존재한다면 주의하자. __bitmap__태그를 썼다고 하더라도 __mipmap-anydpi-v26__ 리소스 폴더의 이미지는 __vector__ 이기 때문에 오류가 발생한다.

# 결론

### 두 번째 방법이 좋아보인다.