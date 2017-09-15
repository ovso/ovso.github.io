---
layout: post
title: "Android ADB(Android Debug Bridge)"
description: 
categories: [Android]
tags: [ADB]
redirect_from:
  - /2017/09/16/
---

# Android ADB

adb를 제대로 사용하려면 먼저, [adb의 기본](https://developer.android.com/studio/command-line/adb.html?hl=ko){:target="_blank"}을 습득할 필요가 있다.

자주 쓰거나 중요하다고 생각하는 ADB 명령어들을 나열해본다.

### 연결되어 있는 장치 목록

```
jaeho$ adb devices
List of devices attached
080e3f0d45966	device // 단말
emulator-5560	device
emulator-5558	device
emulator-5554	device
```

### OS 버전

##### 연결되어 있는 장치가 하나 일 때

```
// 연결되어 있는 장치가 하나 일 때
jaeho$ adb shell getprop ro.build.version.release
4.1.2

// 연결되어 있는 장치가 두 개 이상일 때, 지정하여 나타내기
jaeho$ adb -s emulator-5560 shell getprop ro.build.version.release
4.1.2

// USB에 연결되어 있는 실제 장치가 하나 일 때
jaeho$ adb -d shell getprop ro.build.version.release 
4.1.2

// 실행중인 에뮬레이터가 하나 일 때
jaeho$ adb -e shell getprop ro.build.version.release 
4.1.2
```

### API Level

```
//연결되어 있는 장치가 하나 일 때
jaeho$ adb shell getprop ro.build.version.sdk
16

//연결되어 있는 장치가 두 개 이상일 때, 지정하여 나타내기
jaeho$ adb -s emulator-5560 shell getprop ro.build.version.sdk
16

//USB에 연결되어 있는 실제 장치가 하나 일 때
jaeho$ adb -d shell getprop ro.build.version.sdk
16

// 실행중인 에뮬레이터가 하나 일 때
jaeho$ adb -e shell getprop ro.build.version.sdk 
16
```

### getprop로 볼 수 있는 단말의 모든 등록정보

```
adb shell (-s emulator-5560:여러개일 경우) getprop
```

### 응용

```
// APK 설치
adb -s emulator-5560 install -r(재설치) Sample.apk // 여러 장치가 있을 경우
adb -d install -r(재설치) Sample.apk // USB연결된 장치가 하나만 있을 경우
```

### 참고

[https://developer.android.com/studio/command-line/adb.html?hl=ko](https://developer.android.com/studio/command-line/adb.html?hl=ko){:target="_blank"}