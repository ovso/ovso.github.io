---
layout: post
title: "SpeechRecognizer(음성인식)"
description: 
categories: [Android]
tags: [Opensource]
redirect_from:
  - /2018/10/10/
---

# Intent 설정

```java
Intent intent = new Intent(RecognizerIntent.ACTION_RECOGNIZE_SPEECH);
```

# Language 설정

```java
intent.putExtra(RecognizerIntent.EXTRA_LANGUAGE, Locale.getDefault().toLanguageTag());
```

언어를 설정할 때 주의할 것은 있다. '언어 값'이다. 흔히 생각하는 en, ko, jp가 아니다. [ko-KR, ja-JP, it-IT 형태](https://developer.android.com/reference/android/speech/RecognizerIntent.html#EXTRA_LANGUAGE){:target="_blank"}이어야 한다. 이런 형태는 tag라고 하는데 [Locale](https://developer.android.com/reference/java/util/Locale){:target="_blank"} 객체의 [toLanguageTag](https://developer.android.com/reference/java/util/Locale.html#toLanguageTag()){:target="_blank"}를 통해서 가져올 수 있다. 이런 tag를 [IETF BCP 47 language tag](https://developer.android.com/reference/java/util/Locale.html#toLanguageTag()){:target="_blank"}라 한다.

# Language Tag 리스트 얻기

*리스트를 얻으려면 디바이스 환경설정에서 언어를 추가해야 한다.*

그런 다음 [toLanguageTags](https://developer.android.com/reference/androidx/core/os/LocaleListCompat#toLanguageTags){:target="_blank"} 를 호출하여 리스트를 얻을 수 있다. 리스트는 String 객체에 Append되어 있는 형태이다.

```java
LocaleListCompat.getDefault().toLanguageTags(); // ko-KR,ja-JP,it-IT
```