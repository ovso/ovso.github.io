---
layout: post
title: "Android 앱의 알림 설정 화면으로 이동"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/09/16/
---

# 앱의 알림 설정 화면으로 이동

이번에 개인앱을 개발하면서 Firebase의 Notifications를(을) 사용하게 됐다. 앱의 알림 ON/OFF를 단말의 설정에서 하도록 유도했는데, API Level 별로 달라서 좀 헤맸다. 처리는 아래와 같다.

### API Level 26 이상

```java
Intent intent = new Intent();
intent.setAction("android.settings.APP_NOTIFICATION_SETTINGS");
intent.putExtra("android.provider.extra.APP_PACKAGE", getActivity().getPackageName());
startActivity(intent);
```

### API Level 21 ~ 25

```java
Intent intent = new Intent();
intent.setAction("android.settings.APP_NOTIFICATION_SETTINGS");
intent.putExtra("app_package", getActivity().getPackageName());
intent.putExtra("app_uid", getActivity().getApplicationInfo().uid);
startActivity(intent);
```

### API Level 21 미만

```java
Intent intent = new Intent();
intent.setAction(Settings.ACTION_APPLICATION_DETAILS_SETTINGS);
intent.addCategory(Intent.CATEGORY_DEFAULT);
intent.setData(Uri.parse("package:" + getActivity().getPackageName()));
startActivity(intent);
```

즐겁게 개발하자^__^