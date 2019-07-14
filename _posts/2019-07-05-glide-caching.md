---
layout: post
title: "Glide Caching(캐시, 사전캐시-preload)"
categories: [Android]
tags: [Android]
---

# How to use Glide?

### 일반적인 방법

```kotlin
Glide.with(context).load(imageUrl).into(imageView)
```

### 옵션을 설정하는 방법

```kotlin
...
..
.
private var requestOptions = RequestOptions()
        .skipMemoryCache(true) // 메모리 캐시를 사용하지 않음
        .diskCacheStrategy(DiskCacheStrategy.DATA) // 디스크캐시 함
        .format(DecodeFormat.PREFER_RGB_565)
        .timeout(1000 * 10)

private var requestManager:RequestManager?
...
..
.
private fun setupGlide() { // 초기화 코드
  requestManager = Glide.with(context).apply {
    setDefaultRequestOptions(requestOptions)
    applyDefaultRequestOptions(requestOptions)
  }
}
...
..
.
```

# 참고

[https://bumptech.github.io/glide/doc/caching.html](https://bumptech.github.io/glide/doc/caching.html)