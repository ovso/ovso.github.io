---
layout: post
title: "Failed to resolve: play-services-basement"
description: 
categories: [Android]
tags: [Opensource]
redirect_from:
  - /2018/10/10/
---

# 빌드 실패(에러)

```java
Failed to resolve: play-services-basement
```

# 원인

Gradle 도구에서 androidDependencies 를 실행하자. 아래와 같은 로그를 확인할 수 있다.

```
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:androidDependencies'.
> Could not resolve all artifacts for configuration ':app:debugCompileClasspath'.
   > Could not find play-services-basement.aar (com.google.android.gms:play-services-basement:15.0.1).
     Searched in the following locations:
         https://jcenter.bintray.com/com/google/android/gms/play-services-basement/15.0.1/play-services-basement-15.0.1.aar

...
```

play-services-basement 라이브러리를 찾을 수 없다. 이 라이브러리는 jcenter에서 찾을 수 있다. 라는 로그를 접하게 된다.

# 해결

이전 repositories

```groovy
allprojects {
  repositories {
    jcenter()
    google()
    maven { url "https://jitpack.io" }
  }
}
```

이후 repositories

```groovy
allprojects {
  repositories {
    google()
    jcenter()
    maven { url "https://jitpack.io" }
  }
}
```



[이곳](https://developer.android.com/studio/releases/gradle-plugin){:target="_blank"}을 참고하여 repositories의 순서를 바꿨다. 빌드에 성공했다.

불러오는 순서에 문제가 있었던 것이다.

# 참고

play-service-basement 에러는 두 가지 원인이 있는 것으로 보인다.

링크를 참고하자.

[https://gh0st.tistory.com/24](https://gh0st.tistory.com/24){:target="_blank"}