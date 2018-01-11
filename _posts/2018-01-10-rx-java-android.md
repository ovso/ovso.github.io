---
layout: post
title: "RxJava, RxAndroid"
description: 
categories: [RxJava]
tags: [RxJava]
redirect_from:
  - /2018/01/10/
---

# RxJava, RxAndroid

# Single에서 FlatMap(2.0)

## 사례

[Retrofit](http://square.github.io/retrofit/){:target="_blank"}으로 네트워킹 한 다음 응답값을 하나의 객체로 받았다. 객체 안의 리스트 데이터를 subscribe에서 받으려 한다. 그리고 리스트 데이터 중 전화번호가 없는 것은 걸러내야 한다. 먼저 FlatMap을 사용해야 한다. Single에서는 FlatMap을 바로 사용할 수 없다. 때문에 flattenAsObservable을 사용했고, flatMap이후 filter를 거쳐 toList로 뽑아내면 된다.

## 코드

```Java
network.getResult(this.query, page)
        .subscribeOn(Schedulers.io())
        .flattenAsObservable(dResult -> {
          return dResult.getDocuments();
        })
        .flatMap(documents -> Observable.just(documents))
        .filter(documents -> !TextUtils.isEmpty(documents.getPhone()))
        .toList()
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(items -> {
        }));
```

