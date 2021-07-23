---
layout: post
title: "앱 재시작(App restart)"
categories: [Android]
tags: [Android]

---

# 앱을 재시작 하자

<script src="https://gist.github.com/ovso/0f4e9a244f272d569294b8ee1949d6b3.js"></script>

[스택오버플로어](https://stackoverflow.com/questions/6609414/how-do-i-programmatically-restart-an-android-app)에서 좀 더 많은 정보를 얻을 수 있다. 위의 재시작 로직보다 좀 더 개선된 버전의 로직을 볼 수 있다. 위의 `restart` 로직은, [Process Phoenix](https://github.com/JakeWharton/ProcessPhoenix) 에서 사용하는 방법이라고 한다.

[Process Phoenix](https://github.com/JakeWharton/ProcessPhoenix)는 [JakeWharton](https://github.com/JakeWharton)이 것으로, 앱을 쉽게 재시작할 수 있게 하는 오픈소스다.

## Process Phoenix

[Process Phoenix](https://github.com/JakeWharton/ProcessPhoenix) 의 사용법 두가지:

```java
ProcessPhoenix.triggerRebirth(MainActivity.this);
```

```java
Intent nextIntent = new Intent(MainActivity.this, MainActivity.class);
nextIntent.putExtra(EXTRA_TEXT, "Hello!");
ProcessPhoenix.triggerRebirth(MainActivity.this, nextIntent);
```

