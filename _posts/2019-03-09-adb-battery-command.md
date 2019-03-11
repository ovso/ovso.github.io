---
layout: post
title: "Adb를 통한 배터라 상태 변경(Battery status changed)"
categories: [ADB]
tags: [ADB]
---

# 실시간 배터리 상태 변경

# 배터리 상태 보기

```
adb shell dumpsys battery
```

```
Current Battery Service state:
  (UPDATES STOPPED -- use 'reset' to restart)
  AC powered: true
  USB powered: false
  Wireless powered: false
  Max charging current: 5000000
  Max charging voltage: 5000000
  Charge counter: 10000
  status: 2
  health: 2
  present: true
  level: 20
  scale: 100
  voltage: 5000
  temperature: 250
  technology: Li-ion
```

# 명령어 도움말

```
adb shell dumpsys battery -h
```

```
Battery service (battery) commands:
  help
    Print this help text.
  set [-f] [ac|usb|wireless|status|level|temp|present|invalid] <value>
    Force a battery property value, freezing battery state.
    -f: force a battery change broadcast be sent, prints new sequence.
  unplug [-f]
    Force battery unplugged, freezing battery state.
    -f: force a battery change broadcast be sent, prints new sequence.
  reset [-f]
    Unfreeze battery state, returning to current hardware values.
    -f: force a battery change broadcast be sent, prints new sequence.
```



# Level

```
adb shell dumpsys battery set level 30
```

# Power source

```
adb shell dumpsys battery set usb 0 // or 1
adb shell dumpsys battery set ac 0 // or 1
adb shell dumpsys battery set wireless 0 // or 1
adb shell dumpsys battery set unplug // 배터리 충전(개발자가 정의해야 한다.)
```

