---
layout: post
title: "Adb를 통한 CPU 사용량 보기"
categories: [ADB]
tags: [ADB]
---

# 실시간 CPU 사용량 순위 보기

아래의 명령어는 CPU 사용량 순위를 나타낸다. 숫자 10은 순위를 나타낸다.

```
adb shell top -m 10
```

```
...
  PID USER         PR  NI VIRT  RES  SHR S[%CPU] %MEM     TIME+ ARGS            
 1862 system       18  -2 1.6G 141M  73M S 18.3   9.4 147:56.42 system_server
 1609 audioserver  20   0  28M 3.3M 1.8M S 10.3   0.2  62:47.26 android.hardwar+
 1623 system       20   0  11M 1.4M 868K S  9.6   0.0 111:34.34 android.hardwar+
 1628 audioserver  20   0  79M 5.3M 412K S  6.6   0.3  42:41.68 audioserver
 2570 u0_a38       10 -10 1.6G 120M  58M S  5.6   8.0  49:58.86 com.google.andr+
 2417 shell        20   0  35M 1.2M 780K S  4.0   0.0  30:53.90 adbd --root_sec+
 1890 system       -3   0 111M 3.6M 2.2M S  1.6   0.2  76:46.70 android.hardwar+
 1719 system       -2  -8 175M 9.0M 4.3M S  1.0   0.6 502:39.02 surfaceflinger
19309 shell        20   0 7.9M 3.8M 3.1M R  0.6   0.2   0:00.17 top -m 10
19223 shell        20   0 7.9M 3.8M 3.0M R  0.6   0.2   0:01.71 top -m 10

```

