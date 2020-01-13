---
layout: post
title: "라이브러리 종속성 확인(Dependency)"
categories: [Android]
tags: [Android]

---

# 라이브러리 종속성 확인

앱 모듈 수준의 `build.gradle` :

```
./gradlew :app:dependencies

+--- androidx.databinding:databinding-common:3.5.1
+--- androidx.databinding:databinding-runtime:3.5.1
|    +--- androidx.lifecycle:lifecycle-runtime:2.0.0 -> 2.2.0-rc02
|    |    +--- androidx.lifecycle:lifecycle-common:2.2.0-rc02
|    |    |    \--- androidx.annotation:annotation:1.1.0
|    |    +--- androidx.arch.core:core-common:2.1.0
|    |    |    \--- androidx.annotation:annotation:1.1.0
|    |    \--- androidx.annotation:annotation:1.1.0
|    +--- androidx.collection:collection:1.0.0 -> 1.1.0
|


....
```

프로젝틔 수준의 `build.gradle`:

```
./gradlew buildEnvironment

classpath
+--- com.android.tools.build:gradle:3.5.1
|    +--- com.android.tools.build:builder:3.5.1
|    |    +--- com.android.tools.build:builder-model:3.5.1
|    |    |    \--- com.android.tools:annotations:26.5.1
|    |    +--- com.android.tools.build:builder-test-api:3.5.1
|    |    |    \--- com.android.tools.ddms:ddmlib:26.5.1
|    |    |         +--- com.android.tools:common:26.5.1
|    |    |         |    +--- com.android.tools:annotations:26.5.1
|    |    |         |    +--- com.google.guava:guava:27.0.1-jre
|

....
```

