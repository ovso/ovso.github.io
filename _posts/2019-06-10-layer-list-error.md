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

Splash 화면을 구성하고 있는 아래의 xml은 다른 프로젝트에도 사용하고 있다.

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

문제가 없는 xml이다. 그러나 [기종에 따라 문제](https://soulduse.tistory.com/70)가 되고 있다고 한다. 에러의 원인은 item 태그로 감싼 bitmap이다.

때문에 위의 xml을 아래와 같이 수정해야 한다. 



# 해결

bitmap 태그를 사용하지 않고 item태크에 직접 drawable과 gravity를 설정했다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
  <item android:drawable="@color/colorLauncherIconBg"/>
  <item
      android:drawable="@drawable/trending_up"
      android:gravity="center"/>
</layer-list>
```



# 결론

아무리 토이프로젝트더라도 여러 종류의 단말에서 테스트해볼 필요성이 있다.

참고 : [https://soulduse.tistory.com/70](https://soulduse.tistory.com/70)