---
layout: post
title: "Android 라이브러리 충돌 문제"
description: 
categories: [Android]
tags: [Gradle]
redirect_from:
  - /2017/08/18/
---

## 현상

![](https://ovso.github.io/images/2017-05-24-library-conflict-01.png)

깃 코드를 내려받아서(Clone) 분석할 때 build.gradle(Module)의 dependencies에서 에서 라이브러리 충돌이 발생하는 경우가 있다. 충돌이 나더라도 빌드나 배포에는 지장이 없으나 나중에 문제가 생길 가능성 있다. 컴파일시에 나타나는 에러 표시를 보고 있자면 없애고 싶어진다. 개발자의 본능이 아닐까.

## 원인

마우스를 컴파일 에러에 가져다 대면 아래와 같은 힌트를 볼 수 있다.

```
All com.android.support libraries must use the exact same version specification (mixing versions can lead to runtime crashes). Found versions 23.1.1, 23.0.1. Examples include com.android.support:appcompat-v7:23.1.1 and com.android.support:cardview-v7:23.0.1 more... (⌘F1)
```

대략 이런 말이다. "모든 서포트라이브러리는 동일한 버전을 사용해야 한다.(혼합버전은 문제를 일으킬 가능성이 있다) 발견된 버전은 23.1.1, 23.0.1…" 

다행히 발견된 다른 버전이 하나밖에 없다. 그냥 보기에는 정말 그런지 알 수 없다. 실제로 그런지 알아볼 수 있는 방법이 있다. 터미널에서 아래와 같이 작성하고 실행하자.

```
./gradlew :app:dependencies
```

그럼, 아래와 같은 화면을 터미널에서 볼 수 있게 된다. 소스를 컴파일 하기 위한 클래스패스를 나타낸다. 잘 보면 두 버전이 섞여 있는 것을 알 수 있다. 화살표(—>)는, 자동으로 좌측 버전을 우측 버전으로 업데이트 했다는 얘기다. 별(*)은 무엇인지 모르겠다.

![](https://ovso.github.io/images/2017-05-24-library-conflict-02.png)

## 해결

![](https://ovso.github.io/images/2017-05-24-library-conflict-03.png)

서포트라이브러리 버전을 23.0.1에서 23.1.1로 변경하니 컴파일 에러가 사라졌다. 다행히 간단한 문제였다. 이보다 좀 더 복잡한 경우가 있을 수 있다. 당황하지 말고 차근차근 살펴보도록 하자.
