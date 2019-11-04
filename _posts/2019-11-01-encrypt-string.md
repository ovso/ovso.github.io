---
layout: post
title: "문자열 암호화 하기(Encrypt string)"
categories: [Android]
tags: [Android]
---

# 문자열 암호화(Encrypt string)

문자열 암호화란? 중요한 문자열(키값 등..)을 알아볼 수 없는 암호문으로 변환하는 것을 말한다. 소프트웨어를 개발하면서 중요한 문자열(키값)을 암호문으로 변환한다는 것은 해킹을 어렵게 만든다. 그러나 해킹을 불가능하게 할 순 없다.

# 문자열 암호화 방법

안드로이드에서 문자열을 암호하는 대표적인 방법은 두가지로 나눌 수 있다. 필자의 주관으로 두가지다. 그것은 덱스가드와, NDK다. 덱스가드는 유료이면서 비용이 높다. 그러나 해킹을 하기에는 정말 어려울 것이다. 그해 비해 NDK를 사용하는 방법은 비용이 들지 않는다. 개발자가 직접 구현하면 된다. 해킹하는 것이 쉽지는 않지만, 덱스가드에 비해서 어렵진 않을것 같다.

프로가드는 문자열을 난독화, 암호화 해주지 않는다. 때문에 안드로이드 개발자에겐 NDK가 중요한 문자열을 암호화 하기에 효과적인 방법일 수 있다.

# NDK 구현방법

## Android SDK 다운로드

안드로이드 스튜디오에서 SDK tools로 이동하여 NDK, CMake를 설치한다. (Preference(⌘ + ,) > Android SDK > SDK tools). C언어를 작성하고 안드로이드 앱에서 작성한 클래스를 사용하기 위해서다.

## Package structure

* app
  * src
    * main
      * cpp
        * `CMakeLists.txt`
        * `native-lib.cpp`
      * Java
* `build.gradle`

## CMakeLists.txt

```c++
# Sets the minimum version of CMake required to build your native library.
# This ensures that a certain set of CMake features is available to
# your build.
# 기본 라이브러리를 빌드하는 데 필요한 최소 CMake 버전을 설정합니다.
# 이렇게하면 빌드에서 특정 CMake 기능을 사용할 수 있습니다.

cmake_minimum_required(VERSION 3.4.1)

# Specifies a library name, specifies whether the library is STATIC or
# SHARED, and provides relative paths to the source code. You can
# define multiple libraries by adding multiple add.library() commands,
# and CMake builds them for you. When you build your app, Gradle
# automatically packages shared libraries with your APK.

# 라이브러리 이름을 지정하고 라이브러리가 STATIC인지 SHARED인지를 지정하고 소스 코드에 대한 상대 경로를 제공합니다.
# 여러 add.library () 명령을 추가하여 여러 라이브러리를 정의 할 수 있으며 CMake는이를 위해 빌드합니다. 
# 앱을 빌 드하면 Gradle은 APK와 함께 공유 라이브러리를 자동으로 패키지합니다.
  
add_library( # Specifies the name of the library.
             native-lib

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )
```

## native-lib.cpp

```c
#include <cstring>
#include <jni.h>

extern "C" JNIEXPORT jstring JNICALL
Java_io_github_ovso_brolayout_StringSecurityActivity_stringFromJNI(JNIEnv *env, jobject thiz) {
    return env->NewStringUTF("V293ISBob3cgY3VyaW91cyBlaD8");
}
```

`NewStringUTF()` 함수의 매개변수로 암호화가 필요한 문자열을 넣는다.

```
Java_io_github_ovso_brolayout_StringSecurityActivity_invokeNativeFunction
```

`Java_패지키명_안드로이드에서 native코드를 사용한클래스_안드로이드에서 문자열을 가져오는 함수`

Java : Java(안드로이드)와 연동하기위한 규칙

`_패키지명` : 안드로이드 applicationId(패키지명)에서 .(닷)을 _(언더바)로 대체한다.

`_호출클래스명` : 안드로이드에서 NDK코드를 사용할 클래스명

`_함수` : 실제 문자열을 갖고 있는 함수명

## build.gradle

externalNativeBuild는 `CMakeLists.txt`의 경로를 갖고 있다.

```groovy
android {
...
..
.
    externalNativeBuild {
        cmake {
            version '3.10.2'
            path 'src/main/cpp/CMakeLists.txt'
        }
    }
}
```



# 사용

```kotlin
class StringSecurityActivity : AppCompatActivity() {

    companion object {
        init {
            System.loadLibrary("native-lib")
        }
    }

    private external fun invokeNativeFunction(): String

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_string_security)
      	println(invokeNativeFunction()) // 출력
    }
}
```



# 리벌스엔지니어링

```kotlin
println(invokeNativeFunction())
```

출력하니 결과가 잘 나온다. `V293ISBob3cgY3VyaW91cyBlaD8=`

그럼 과연 리벌스엔지니어링(디컴파일) 했을때는 어떨까?

