---
layout: post
title: "프로가드 버전 올리기(Proguard)"
categories: [Android]
tags: [Android]

---

# 프로가드 버전 올리기

## 현재 버전 확인

```
./graldew buildEnvironment

....

|    +--- net.sf.proguard:proguard-gradle:6.0.3
|    |    \--- net.sf.proguard:proguard-base:6.0.3
|    +--- com.android.tools.build:bundletool:0.9.0
|

....
```

## 버전 올리기

프로젝트 수준의 `build.gradle`:

```
buildscript {
		...
    dependencies {
        ...
        classpath 'net.sf.proguard:proguard-gradle:6.2.2'
    }
}
```

## 업데이트된 버전 확인

```
./graldew buildEnvironment

...

|    +--- net.sf.jopt-simple:jopt-simple:4.9
|    +--- net.sf.proguard:proguard-gradle:6.0.3 -> 6.2.2
|    |    \--- net.sf.proguard:proguard-base:6.2.2
|

...
```



# 결론

프로가드를 포함하고 있는 R8이 나왔다. R8 사용을 권장한다. R8은 프로가드보다 훨씬 빠르다. 프로가드에 문제가 있다면, 프로가드 버전을 올리기보다는 R8을 사용하는 것이 낫다.