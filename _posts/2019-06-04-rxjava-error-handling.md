---
layout: post
title: "Rxjava2 error handling"
categories: [RxJava2]
tags: [RxJava2]
---

# 에러

```
io.reactivex.exceptions.UndeliverableException: The exception could not be delivered to the consumer because it has already canceled/disposed the flow or the exception has nowhere to go to begin with. Further reading: https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0#error-handling | java.lang.RuntimeException: java.io.InterruptedIOException: thread interrupted
```

# 원인

RxJava2를 사용해서, 병렬 쓰레드를 사용했다. onComplete 호출되기 전에 화면을 종료하니(compositeDisposable.clear() ) 에러가 발생했다.

```kotlin
addDisposable(
  Flowable.fromIterable(getObservables(countryCodes))
  .parallel()
  .runOn(Schedulers.computation())
  .map {
    elements.add(it.blockingFirst().getElementsByTag("item").first())
  }
  .sequential()
  .subscribeBy(
    onError = {
      Timber.e(it)
      isLoading.set(false)
    },
    onComplete = {
      isLoading.set(false)
      elementsLiveData.postValue(elements) // postValue -> mainThread
    }
  )
)
```



# 해결

에러 로그를 자세히 들여다보면 에러를 처리할 수 있는 가이드를 제공해주고 있다.

```
https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0#error-handling
```

필자는 kotlin를 통해 개발하고 있고, Application 클래스에 아래와 같이 선언하면 크래시가 발생하지 않는걸 알 수 있다.

```kotlin
RxJavaPlugins.setErrorHandler(Functions.emptyConsumer());
```



[https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0#error-handling](https://github.com/ReactiveX/RxJava/wiki/What's-different-in-2.0#error-handling)

CodeIris란 심플하게 UML(Unified Modeling Language)를 그려주는 도구다. Intellij에 설치할 수 있다. 다른 개발환경에서도 설치 가능하다. 정말 심플하기 때문에 전체적인 흐름 정도만 보면 된다.

# How to install?

안드로이드 스튜디오의 플러그인 마켓을 통한 설치법이 가장 쉽고 편하다.

Preferences(⌘ + ,) 창을 연다. Plugins 탭 안의 MarketPlace탭에서 Code Iris를 검색해서 설치하면 되겠다.

# How to use?

