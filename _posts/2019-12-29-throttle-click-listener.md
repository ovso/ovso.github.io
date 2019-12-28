---
layout: post
title: "버튼 연타(중복) 클릭 막기"
categories: [Android]
tags: [Android]
---

안드로이드 액티비티(또는 다이얼로그)를 전환 할 때 버튼이 여러 번 눌릴때가 있다. 문제(Issue)로 볼 수도 있고 그렇지 않을 수도 있다. 대부분이 문제로 인식할 것이다. 화면이 두 번 뜬다는 것은 모든 코드가 두 번 실행되는 것이니 문제가 맞다. 문제로 인식 하지 않는 경우라면(~~우기자면...~~), 안드로이드 `설정` 에서도 버튼이 두 번 눌려 화면이 두 번 뜰 경우가 있을 때다.

의식이 있는 사람(기획자, 디자이너, QA 담당자, 개발자 등..) 이라면 문제로 인식하고 있을 것이다.

원인은 정확히 모르겠다. 다만, 버튼을 눌렀을 때 전환되는 화면이 좀 늦게 뜨는 찰나에 동일한 이벤트가 들어오는 것이 문제라고 생각되는 정도다. 막아보자!!

# 연타를 막는 여러가지 방법

방법은, 검색하면 정말 많이 나올 것이다.

## 시간 차이를 이용하는 방법​ :slightly_smiling_face:

[https://youngest-programming.tistory.com/83](https://youngest-programming.tistory.com/83)

## RxAndroid를 사용하는 방법​ :smile:

[https://www.androidhuman.com/tip/rx/2016/04/08/rx_cheatsheet_throttle_first/](https://www.androidhuman.com/tip/rx/2016/04/08/rx_cheatsheet_throttle_first/)

## KTX와 데이터바인딩을 사용하는 방법 :star2:

[https://medium.com/@masaaki.iwaguchi/android-how-to-prevent-multiple-view-clicks-using-databinding-and-kotlin-extensions-5d5859071b4b](https://medium.com/@masaaki.iwaguchi/android-how-to-prevent-multiple-view-clicks-using-databinding-and-kotlin-extensions-5d5859071b4b)



# 권하는 방법

## **KTX와 데이터바인딩을 사용하는 방법**:star2:

### 하나의 클래스와 하나의 BindingAdapter가 필요

먼저, 하나의 클래스라는 건 OnClickListener를 커스텀한 클래스다. 취향(?)에 맞게 클래스 명을 지어주자. 필자는 OnThrottleClickListener라고 했다. 클래스는 View.OnClickListener와 delayMillis를 매개변수로 받고 있다.

오버라이드된 `onClick` 메서드에서 지연 시간을 구현한다. `onClick`의 시작은 `canClick.getAndSet(false)`라는 코드다. 초기 값(true)을 사용하는 동시에, 값을 변경하고 있다. 설정한 지연 시간이 다시 초기값(true)로 변경된다.

```kotlin
class OnThrottleClickListener(
  private val clickListener: View.OnClickListener,
  private val delayMillis: Long = 1000L
) : View.OnClickListener {
  private var canClick = AtomicBoolean(true)

  override fun onClick(v: View?) {
    if (canClick.getAndSet(false)) {
      v?.run {
        postDelayed({
          canClick.set(true)
        }, delayMillis)
        clickListener.onClick(v)
      }
    }
  }
}
```

다른 하나는 바인딩어뎁터다. 이름은 `onThrottleClick`이다. 이는 XML에서 사용하기 위한 이름이다. XML에서 사용할 때는 `app:onThrottleClick` 또는 `onThrottleClick`으로 사용 할 수 있다.

```kotlin
@BindingAdapter("onThrottleClick")
fun View.setOnThrottleClickListener(
  clickListener: View.OnClickListener?
) {
  clickListener?.also {
    setOnClickListener(OnThrottleClickListener(it))
  } ?: setOnClickListener(null)
}	
```

### XML에서 사용

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
                name="viewModel"
                type="xx.xx.xx.ViewModel"/>
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        <Button
                android:id="@+id/button"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ThrottleButton"
                app:layout_constraintLeft_toLeftOf="parent"
                app:layout_constraintRight_toRightOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/hw"
                onThrottleClick="@{() -> viewModel.onClick()}"/>
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

또는, `onThrottleClick="@{viewModel::onClick}"`으로 구현해도 된다. `android:onClick`과 동일하다.

Fragment 또는 Activity에서 이벤트를 받고 싶다면, 아래와 같이 사용하면 된다.

```xml
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto"
        xmlns:tools="http://schemas.android.com/tools">
    <data>
        <variable
                name="viewModel"
                type="xx.xx.xx.ViewModel"/>
          <variable 
              name="clickListener"
              type="android.view.View.OnClickListener"
              />
    </data>

    <androidx.constraintlayout.widget.ConstraintLayout
            android:layout_width="match_parent"
            android:layout_height="match_parent">
        <Button
                android:id="@+id/button"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="ThrottleButton"
                app:layout_constraintLeft_toLeftOf="parent"
                app:layout_constraintRight_toRightOf="parent"
                app:layout_constraintTop_toBottomOf="@+id/hw"
                onThrottleClick="@{clickListener"/>
    </androidx.constraintlayout.widget.ConstraintLayout>
</layout>
```

### 프로그래밍 방식으로 사용

바로 사용할 수 있다.

```kotlin
button.setOnThrottleClickListener(View.OnClickListener {
  // Do something
})
```

지연 시간을 조절할 수도 있다.

```kotlin
button.setOnClickListener(OnSingleClickListener(View.OnClickListener {
	// Do something
}, delayMillis = 3000L))
```

# 결론

효율을 생각하고 개발하자.