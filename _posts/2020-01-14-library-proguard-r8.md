---
layout: post
title: "모듈(라이브러리)의 프로가드"
categories: [Android]
tags: [Android]

---

# What is proguard?

프로가드란, 앱을 축소, 난독화 및 최적화하는 도구다. 사용하지 않는 리소스나 클래스를 제거하여 앱을 축소하고, 클래스, 멤버명을 줄여 난독화 한다. 난독화 또는 앱 축소 역할을 한다.

구체적으로, 다른과 같은 네 가지 역할을 한다.

### 코드 축소

앱에서 사용하지 않는 클래스, 필드, 메서드, 속성 및 라이브러리 종속성을 감지하여 안전하게 삭제한다. 이는 [64k 참조 제한](https://developer.android.com/studio/build/multidex.html?hl=ko)을 해결하기 위한 유용한 도구로 사용되기도 한다.

### 리소스 축소

앱의 라이브러리 종속성에서 사용하지 않는 리소스를 포함하여 패키징된 앱에서 사용하지 않는 리소스를 삭제합니다.

### 난독화

클래스와 멤버 이름을 줄여 DEX 파일 크기를 줄입니다. 자세히 알아보려면 [코드 난독화](https://developer.android.com/studio/build/shrink-code?hl=ko#obfuscate) 방법에 관한 섹션을 참조하세요.

### 최적화

코드를 검사하고 다시 작성하여 앱 DEX 파일의 크기를 더 줄입니다. 예를 들어, if/else 구문의 `else {}` 브랜치가 전혀 사용되지 않음을 R8에서 감지한 경우 R8이 `else {}` 브랜치 코드를 삭제합니다

# R8

[Android Gradle 플러그인 3.4.0](https://developer.android.com/studio/releases/gradle-plugin?hl=ko#3-4-0) 이상을 사용하여 프로젝트를 빌드하는 경우 플러그인은 더 이상 ProGuard를 사용하여 컴파일 타임 코드 최적화 작업을 하지 않습니다. 대신 플러그인은 *R8 컴파일러*를 이용하여 다음의 컴파일 타임 작업을 처리합니다.

쉽게 말해 R8 내부에서 프로가드를 사용한다고 보면 된다.

물론, `minifyEnable` 속성은 `true` 여야 한다.

`minifyEnabled` 속성을 `true`로 설정하면 기본적으로 R8에서 코드 축소가 사용 설정됩니다

컴파일 타임에 R8은 프로젝트의 결합된 keep 규칙을 기반으로 그래프를 작성하여 도달하지 않는 코드를 결정합니다. : 

![](https://developer.android.com/studio/images/build/r8/tree-shaking.png?hl=ko)

# R8 사용

[Android Gradle 플러그인 3.4.0](https://developer.android.com/studio/releases/gradle-plugin?hl=ko#3-4-0) 이상부터 R8을 사용할 수 있지만, 기본값은 `비활성` 이다. 때문에 R8을 활성화 해줘야 한다.

`gradle.properties`:

```
android.enableR8=false ---> #android.enableR8=false
```



# (외부)모듈에서 프로가드 설정하기

앱 모듈 수준의 `build.gradle` 에서 아래와 같이 규칙을 설정하면, 모듈을 사용하는 앱에서 별도의 프로가드 규칙을 추가하지 않아도 된다.

`build.gradle`:

```groovy
android {
    defaultConfig {
      ...
      consumerProguardFiles 'consumer-rules.pro'
    }
}
```

`consumer-rules.pro`:

```
# okhttp3
-dontwarn okhttp3.**
-dontwarn okio.**
-dontwarn javax.annotation.**
-keepnames class okhttp3.internal.publicsuffix.PublicSuffixDatabase

# reactivestreams
-dontwarn org.reactivestreams.FlowAdapters
-dontwarn org.reactivestreams.**
-dontwarn java.util.concurrent.flow.**
-dontwarn java.util.concurrent.**

# kotlinx
-dontwarn kotlinx.coroutines.flow.**inlined**

# keep
-keep class android.support.annotation.Keep
-keep @android.support.annotation.Keep class * {*;}
-keepclasseswithmembers class * {
	@android.support.annotation.Keep <methods>;
}
-keepclasseswithmembers class * {
	@android.support.annotation.Keep <fields>;
}
-keepclasseswithmembers class * {
	@android.support.annotation.Keep <init>(...);
}

...
..
.
```



# 참고

** 중요 

https://engineering.linecorp.com/ko/blog/line-android-app-build-with-r8-compiler/