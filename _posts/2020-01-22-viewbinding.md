---
layout: post
title: "ViewBinding(뷰바인딩)"
categories: [Android]
tags: [Android]

---

# ViewBinding이란?

*뷰 바인딩* 기능을 사용하면 뷰와 상호작용하는 코드를 쉽게 작성할 수 있습니다. 모듈에서 사용 설정된 뷰 바인딩은 모듈에 있는 각 XML 레이아웃 파일의 *바인딩 클래스*를 생성합니다. 바인딩 클래스의 인스턴스에는 상응하는 레이아웃에 ID가 있는 모든 뷰의 직접 참조가 포함됩니다.

대부분의 경우 뷰 바인딩이 `findViewById`를 대체합니다.

[https://developer.android.com/topic/libraries/view-binding#fragments](https://developer.android.com/topic/libraries/view-binding#fragments)

# 설정

#### 모듈 수준의 `build.gradle`:

```groovy
android {
  ...
    viewBinding {
      enabled = true
    }
}
```

단, `Android Studio 3.6` 부터 사용 가능하다.

##### ViewBinding 무시

```xml
<LinearLayout
  tools:viewBindingIgnore="true" 
  ...>
  	...
</LinearLayout>

```

# 사용

`result_profile.xml` :

```xml
<LinearLayout ... >
  <TextView android:id="@+id/name" />
  <ImageView android:cropToPadding="true" />
  <Button android:id="@+id/button"
          android:background="@drawable/rounded_button" />
</LinearLayout>
```

데이터바인딩을 사용 할 때 처럼, `<layout></layout>` 을 사용하지 않아도 된다.

`MainActivity` :

```kotlin
private lateinit var binding: ResultProfileBinding

@Override
fun onCreate(savedInstanceState: Bundle) {
  super.onCreate(savedInstanceState)
  binding = ResultProfileBinding.inflate(layoutInflater)
  setContentView(binding.root)
}
```

`ResultProfileBinding` 클래스가 자동으로 생성되는데, 레이아웃의 이름이 곧 바인딩 클래스 이름이 된다. 규칙은, 카멜표기법을 따른다. 레이아웃 이름이 `result_profile` 이므로, 뷰바인딩 클래스 이름이 `ResultProfileBinding`이다.

# findViewById 대체

ButterKnife, Kotlin Android Extensions ViewBinding, DataBinding 등을 대체한다.

# 데이터 바인딩 라이브러리와의 차이점

뷰 바인딩과 [데이터 바인딩 라이브러리](https://developer.android.com/topic/libraries/data-binding)는 둘 다 모두 뷰를 직접 참조하는 데 사용할 수 있는 바인딩 클래스를 생성합니다. 하지만 다음과 같이 중요한 차이점이 있습니다.

- 데이터 바인딩 라이브러리는 `<layout></layout>` 태그를 사용하여 생성된 데이터 바인딩 레이아웃만 처리합니다.
- 뷰 바인딩은 레이아웃 변수 또는 레이아웃 식을 지원하지 않으므로 레이아웃을 XML의 데이터와 바인딩하는 데 사용할 수 없습니다.

[https://developer.android.com/topic/libraries/view-binding#findviewbyid](https://developer.android.com/topic/libraries/view-binding#findviewbyid)

# 결론

뷰바인딩을 반드시 써야 할 이유가 없다. 뷰바인딩은, 데이터바인딩에서 데이터와 뷰를 바인딩하는 기능을 뺀 나머지이기 때문이다. 데이터바인딩 기능의 범주에 들어간다. `데이터 바인딩 + 코틀린 익스텐션`이 있는 상황에서, 뷰바인딩을 굳이 사용할 필요가 없는 것이다.

좀 더 좋은 기능이 추가되거나, 빌드 속도에 큰 영향을 미친다면 고려해볼 필요는 있다.