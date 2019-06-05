---
layout: post
title: "Cannot find the setter for attribute..(BindingAdapter)"
categories: [Android]
tags: [Android]
---

# 에러

데이터바인딩을 통해 뷰홀더를 구현하는데 아래와 같은 에러가 떨어졌다. 데이터바인딩으로 TextView에 text를 넣는 것은 문제가 없다. 그러나 ImageView가 문제였다.

```
Found data binding errors.
****/ data binding error ****msg:Cannot find the setter for attribute 'app:image_url' with parameter type java.lang.String on android.widget.ImageView. file:/Users/jaeho/Documents/workspace_personal/MuTest/app/src/main/res/layout/item_all.xml loc:24:25 - 24:39 ****\ data binding error ****
```

# 원인

필자는 BindingAdapter를 사용했다. BindingAdapter는 Annotation을 사용한다. kotlin에서 Annotation을 사용하려면 Annotation Processor 플러그인을 반드시 추가해줘야 한다.

```kotlin
object ItemBinding {
  @JvmStatic
  @BindingAdapter("image_url")
  fun loadImage(imageView: ImageView, url: String) {
    Glide.with(imageView.context).load(url).into(imageView)
  }
}
```

# 결론

모듈 수준의 build.gradle 최상단에 플러그인을 선언해주는 것으로 문제를 해결했다.

```groovy
apply plugin: 'kotlin-kapt'
```

[다른 방법도 존재한다.](https://kotlinlang.org/docs/reference/kapt.html)

