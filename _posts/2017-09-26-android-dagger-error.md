---
layout: post
title: "Conflict with dependency 'com.google.code.findbugs:jsr305'"
description: 
categories: [Android]
tags: [Dagger]
redirect_from:
  - /2017/09/26/
---

Dagger를 사용하기 위해 아래와 같이 최신 버전을 추가했다. 그러나 그래들 동기화 중에 에러가 발생했다.

```groovy
dependencies {
  	...
	compile 'com.google.dagger:dagger:2.11'
  	annotationProcessor 'com.google.dagger:dagger-compiler:2.11'
  	compile 'com.google.dagger:dagger-android-support:2.11'
  	...
}
```

# 에러

```
Error:Conflict with dependency 'com.google.code.findbugs:jsr305' in project ':app'. Resolved versions for app (3.0.1) and test app (2.0.1) differ. See http://g.co/androidstudio/app-test-app-conflict for details.
```

# 원인

그래들 에러가 나타나는 원인은, 어디에선가 버전이 다른(3.0.1, 2.0.1) JSR305 라이브러리를 동시에 사용하려고 했기 때문이다.

# 처리과정

어느 부분에서 오래된 버전(2.0.1)을 사용하고 있는지, Terminal을 통해 알아보자.

```
./gradlew :app:dependencies

\--- com.android.support.test.espresso:espresso-core:2.2.2
     +--- com.squareup:javawriter:2.1.1
     +--- com.android.support.test:rules:0.5
     |    \--- com.android.support.test:runner:0.5
     |         +--- junit:junit:4.12
     |         |    \--- org.hamcrest:hamcrest-core:1.3
     |         \--- com.android.support.test:exposed-instrumentation-api-publish:0.5
     +--- com.android.support.test:runner:0.5 (*)
     +--- javax.inject:javax.inject:1
     +--- org.hamcrest:hamcrest-library:1.3
     |    \--- org.hamcrest:hamcrest-core:1.3
     +--- com.android.support.test.espresso:espresso-idling-resource:2.2.2
     +--- org.hamcrest:hamcrest-integration:1.3
     |    \--- org.hamcrest:hamcrest-library:1.3 (*)
     +--- com.google.code.findbugs:jsr305:2.0.1
     \--- javax.annotation:javax.annotation-api:1.2
```

위 내용을 살펴보면 맨 아래에서 위로 두 번째 줄에 **com.google.code.findbugs:jsr305:2.0.1** 버전이 존재한다. 이것은 espresso-core:2.2.2 내부에 존재하는 버전이다. 다른 부분을(./gradlew :app:dependencies) 살펴보면 Dagger에서 3.0.1 버전을 이미 갖고 있다. 해결하려면, espresso-core:2.2.2에서 2.0.1버전을 사용하지 못하게 막거나(exclude), Dagger에서 3.0.1버전을 사용하지 못하게 막아야 한다. 물론, 최신버전을 사용하는 것을 누구든 권장할 것이다.

# 해결

```groovy
dependencies {
  compile fileTree(dir: 'libs', include: ['*.jar'])
  androidTestCompile('com.android.support.test.espresso:espresso-core:2.2.2', {
    exclude group: 'com.android.support', module: 'support-annotations'
    exclude group: 'com.google.code.findbugs'// 사용하지 못하게 막기
  })
}
```



즐겁게 개발자자 :)