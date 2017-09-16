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
adb devices
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
adb shell getprop ro.build.version.release
4.1.2

// 연결되어 있는 장치가 두 개 이상일 때, 지정하여 나타내기
adb -s emulator-5560 shell getprop ro.build.version.release
4.1.2

// USB에 연결되어 있는 실제 장치가 하나 일 때
adb -d shell getprop ro.build.version.release 
4.1.2

// 실행중인 에뮬레이터가 하나 일 때
adb -e shell getprop ro.build.version.release 
4.1.2
```

### API Level

```
//연결되어 있는 장치가 하나 일 때
adb shell getprop ro.build.version.sdk
16

//연결되어 있는 장치가 두 개 이상일 때, 지정하여 나타내기
adb -s emulator-5560 shell getprop ro.build.version.sdk
16

//USB에 연결되어 있는 실제 장치가 하나 일 때
adb -d shell getprop ro.build.version.sdk
16

// 실행중인 에뮬레이터가 하나 일 때
adb -e shell getprop ro.build.version.sdk 
16
```

### getprop로 볼 수 있는 단말의 모든 등록정보

```
adb shell (-s emulator-5560:여러개일 경우) getprop
```

### APK 설치

```
// 여러 장치가 있을 경우
adb -s emulator-5560 install -r(재설치) Sample.apk

// USB로 연결된 장치가 하나만 있을 경우
adb -d install -r(재설치) Sample.apk

// Emulator로 연결된 장치가 하나만 있을 경우
adb -e install -r(재설치) Sample.apk
```

### Logcat 명령

##### 원하는 수준 이상의 로그 출력

```
adb logcat *:V // Verbose
adb logcat *:D // Debug
adb logcat *:I // Info
adb logcat *:W // Warning
adb logcat *:E // Error
adb logcat *:F // Fatal
adb logcat *:S // Silent(가장 높은 순위, 이경우 아무것도 출력되지 않음)
```

##### 특정 디바이스에서 원하는 수준 이상의 로그 출력

```
adb -s emulator-5554 logcat *:F
```

##### 특정 디바이스에서 원하는 수준과 원하는 태그 로그 출력

```
// *:S로 인해서 ActivityManager 태그와 OJH 태그만 출력 되도록 보장
adb -s emulator-5554 logcat ActivityManager:I OJH:D *:S
```

##### 특정 디바이스에서 원하는 문자열을 포함하는 태그 로그 출력

```
// OJH를 포함하는 로그만 출력
adb -s emulator-5554 logcat|grep OJH
```

##### 로그 출력 형식 제어

```
[adb] logcat [-v <format>]
```

```
adb -s emulator-5554 logcat -v raw
```

로그 메시지에는 태그 및 우선순위 이외에도 여러 메타데이터 필드가 포함됩니다. 특정 메타데이터 필드만 표시하도록 메시지의 출력 형식을 수정할 수 있습니다. 이렇게 하려면 `-v` 옵션을 사용하고 아래 나열된 지원 출력 형식 중 하나를 지정합니다.

`brief` — 메시지를 발급하는 프로세스의 우선순위/태그 및 PID를 표시합니다(기본 형식).

`process` — PID만을 표시합니다.

`tag` — 우선순위/태그만을 표시합니다.

`raw` — 다른 메타데이터 필드는 없이 원시 로그 메시지를 표시합니다.

`time` — 메시지를 발급하는 프로세스의 날짜, 호출 시간, 우선순위/태그 및 PID를 표시합니다.

`threadtime` — 메시지를 발급하는 스레드의 날짜, 호출 시간, 우선순위/태그, PID 및 TID를 표시합니다.

`long` — 모든 메타데이터 필드와 별도의 메시지를 빈 줄과 함께 표시합니다.

### 참고

[https://developer.android.com/studio/command-line/adb.html?hl=ko](https://developer.android.com/studio/command-line/adb.html?hl=ko){:target="_blank"}