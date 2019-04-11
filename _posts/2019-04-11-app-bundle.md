---
layout: post
title: "Bundletool"
categories: [Android]
tags: [Android]
---

# Bundletool 이란?

Bundletool은 Gradle, Android Studio 및 Google Play에서 Android App Bundle을 만들거나 <u>앱 번들을 기기에 배포되는 다양한 APK로 변환하는 데 사용되는 기본 도구</u>입니다. Bundletool은 명령 줄 도구로도 사용할 수 있으므로 앱의 APK에 대한 Google Play의 서버 측 빌드를 다시 만들고 검사하고 확인할 수 있습니다.

# Bundletool 다운로드

깃허브 저장소 : [Bundletool](<https://github.com/google/bundletool>)

# app.aab로부터 apk 세트 생성

```
bundletool build-apks --bundle = <.aab에 대한 경로> --output = <out.apks>
```

두 개의 인수가 필요하다. app.aab 파일의 경로와 apk세트 파일이 필요하다.

### app.abb부터 apk 세트를 생성하는 다른 방법

bundletool은 [bundletool-all-x.x.x.jar](<https://github.com/google/bundletool/releases>)의 별명이다. 이 별명을 사용하려면 OS에 별칭을 등록해야 한다. 등록하지 않고 사용하려면 아래와 같이 명령한다.

```
java -jar <bundletool.jar 경로> build-apks --bundle = <.aab에 대한 경로> --output = <out.apks>
```

# out.apks 파일로부터 apk 추출

apk 세트파일(out.apks)을 압축해제하여 다양한 apk 파일들을 얻는다.

```
unzip out.apks -d apks
```

압축해제하면, apks 하위에 splits 디렉토리가 생기는데 그 안에 모든 apk 파일들이 존재한다.

```
ls -lh apks/splits | awk '{print $9, $5}'
base-ko.apk 15K
base-en.apk 14K
base-ar.apk 16K
base-as.apk 11K
...
..
.
base-xhdpi.apk 106K
base-xxhdpi.apk 130K
base-xxxhdpi.apk 137K
```

apk 파일들 중 base-master.apk가 있다. 이 파일이 동료들에게 배포할 파일이다. 즉, 기존처럼 빌드하여 나온 app.apk파일과 동일한 파일이라는 것이다.

# 터미널에서 bundletool로 사용하기

먼저, bundletool.jar를 OS에 alias로 등록해야 한다.

### .bash_aliases 파일 생성하

```
touch .bash_aliases
```

### .bash_aliases 내용 작성

```
alias bundletool=’java -jar /Users/jaeho/bundletool-all-0.9.0.jar’
```

### 명령어 반영(로드)

```
source .bash_aliases
```

이제 터미널에서, java -jar bundletool.jar build-apks…. 명령어 대신 bundletool build-apks ….을 사용할 수 있다!