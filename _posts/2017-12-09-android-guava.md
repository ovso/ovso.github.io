---
layout: post
title: "Guava"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/12/09/
---

# Guava

[구아바](https://github.com/google/guava){:target="_blank"}는 구글에서 만든 자바용 핵심 라이브러리 이다.

# Guava proguard

```
-injars path/to/myapplication.jar
-injars lib/guava-r07.jar
-libraryjars lib/jsr305.jar
-outjars myapplication-dist.jar

-dontoptimize
-dontobfuscate
-dontwarn sun.misc.Unsafe
-dontwarn com.google.common.collect.MinMaxPriorityQueue

-keepclasseswithmembers public class * {
    public static void main(java.lang.String[]);
}

```

위의 내용은 구아바의 프로가드 사용 가이드다. 안드로이드에서 그대로 사용할 수 없다. 빌드에러가 발생하기 때문이다.

# 빌드에러

##### 안드로이드 프로가드를 사용한다면 구아바를 사용할 때 빌드에러를 내뱉는다.

```
Warning:Exception while processing task java.io.IOException: Can't read [.../app/path/to/myapplication.jar] (No such file or directory)
Error:Execution failed for task ':app:transformClassesAndResourcesWithProguardForRelease'.
> Job failed, see logs for details
```

jar는 당연히 포함되어 있지 않기 때문에, 에러가 발생한다. 때문에 모두 주석처리 해준다.

-injars, -libraryjars, -outjars

[구아바 위키](https://github.com/google/guava/wiki/UsingProGuardWithGuava){:target="_blank"}를 보면  jsr305를 사용할 필요가 있다고 하니, 이는 종속성에서 간단하게 추가하면 된다.

##### 또 빌드에러가 발생한다. Warning이 주르르륵...

```
Warning:there were 105 unresolved references to classes or interfaces.
```

이럴땐, Warning을 없애주는 규칙을 추가한다. 잘 살펴보면 아래의 세 가지에 포함되는 Warning일 것이다.

```
-dontwarn afu.org.checkerframework.**
-dontwarn org.checkerframework.**
-dontwarn com.google.common.util.concurrent.**
```

이제 정상적으로 동작할 것이다.

아래는 제대로 된 프로가드 규칙이다.

```
#-injars path/to/myapplication.jar
#-injars lib/guava-r07.jar
#-libraryjars lib/jsr305.jar
#-outjars myapplication-dist.jar

-dontoptimize
-dontobfuscate
-dontwarn sun.misc.Unsafe
-dontwarn com.google.common.collect.MinMaxPriorityQueue

-keepclasseswithmembers public class * {
    public static void main(java.lang.String[]);
}

-dontwarn afu.org.checkerframework.**
-dontwarn org.checkerframework.**
-dontwarn com.google.common.util.concurrent.**
```



즐겁게 개발하자^^