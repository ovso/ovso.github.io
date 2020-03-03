---
layout: post
title: "ADB로 OS 버전 가져오기(build.version)"
categories: [Android]
tags: [ADB]

---

개발중에 단말의 빌드버전을 알고 싶을때가 있다. 안드로이드 단말은 워낙 다양해서 바로 찾지 못할 경우가 있는데, ADB로 출력하면 아주 편하다. 

# SDK 버전 출력( = Api level)

```
adb -d shell getprop ro.build.version.sdk
// 26
```

# Release 버전 출력

```
adb -d shell getprop ro.build.version.release
// 8.0.0
```

