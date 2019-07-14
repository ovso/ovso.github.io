---
layout: post
title: "Android API Key 제한을 위한 SHA-1 출력"
categories: [Android]
tags: [Android]
---

# API 키를 특정 Android 애플리케이션으로 제한하려면 어떻게 해야 하나

디버그 인증서 디지털 지문 또는 출시 인증서 디지털 지문을 제공하여 API 키를 특정 Android 애플리케이션으로 제한할 수 있다.

구글 클라우드 플랫폼을 사용하려면 반드시 API KEY를 설정하게 되어 있다. Android앱을 개발하면서 API KEY는 공개될 수 있다. 문자열이기 때문이다. 공개되면 누구나 사용할 수 있다. 때문에 API KEY가 공개되었더라도 누구나 사용할 수 없게 제한을 할 필요가 있다. Android앱의 경우 SHA-1을(를) 등록하여 API KEY를 누구나 사용할 수 없게 제한할 수 있다.

### 디버그 인증서 디지털 지문

```
keytool -list -v -keystore ~/.android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

### 출시 인증서 디지털 지문

```
keytool -list -v -keystore your_keystore_name -alias your_alias_name
```

your_keystore_name은 .keystore 확장자를 포함하여 키 저장소의 정규화된 경로와 이름으로 바꾼다. your_alias_name은 인증서 생성 시 할당한 별칭으로 바꾼다.