---
layout: post
title: "ADB로 동영상 녹화하기"
categories: [Android]
tags: [ADB]
---

# 녹화 시작하기

```
adb shell screenrecord /sdcard/video.mp4
```

저장되는 곳은 단말기 또는 AVD 안의 sdcard폴더다.

# 녹화 종료하기

```
^ + Z
```

# 동영상 가져오기

```
adb pull /sdcard/video.mp4
```

퍼센티지가(0% ... 100%) 올라가면서 영상을 가져온다. 영상이 저장되는 폴더는 현재 터미널이 있는 위치다. 물론, 저장되는 위치도 지정이 가능할거다....(?)

# 참고

친절하게도 한글

[https://developer.android.com/studio/command-line/adb?hl=ko](https://developer.android.com/studio/command-line/adb?hl=ko)