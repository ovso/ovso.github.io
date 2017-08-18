---
layout: post
title: "Android Realm 파일 외부로 꺼내기”
description: 
categories: [Android]
tags:[Realm]
redirect_from:
  - /2017/08/18/
---

*AVD에서만 가능하다.*

*[Realm](https://realm.io/docs/java/latest/api/io/realm/RealmConfiguration.html#getRealmDirectory--) 객체로부터 실제 경로를 얻을 수 있다.*

```java
Log.d(TAG, Realm.getConfiguration().getRealmDirectory())
```

```java
Log.d(TAG, Realm.getConfiguration().getRealmFileName())
```

*경로를 얻었다면 ADB를 사용해 실제로 파일을 꺼내보자.*

```
adb pull 디렉토리/파일명(기본 : default.realm)
```

*성공했다면 Realm Browser에서 열어보자.*

*! ADB 사용 중 문제(?)가 생겼다면, 루트 권한을 부여한 후 실행해보자.*

```
adb root
```

