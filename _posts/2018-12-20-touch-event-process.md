---
layout: post
title: "터치 이벤트 전달 과정"
description: 터치 이벤트 전달 과정
categories: [Android]
tags: [Android]
---

# 레이아웃1

```xml
<LinearLayout ...>
    <FrameLayout ...>
        <Button ...>
        </Button>
    </FrameLayout>
</LinearLayout>
```

## 터치 이벤트 전달 과정

최상위 뷰(LinearLayout)를 시작으로 최하위 뷰(Button)로 이벤트가 전달된다. 유저가 최하위 뷰(Button)를 눌렀다고 가정하자. 먼저 LinearLayout의 [dispatchTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#dispatchTouchEvent(android.view.MotionEvent)), [onInterceptTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#onInterceptTouchEvent(android.view.MotionEvent)) 순으로 호출된다. 다음으로 FrameLayout의 [dispatchTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#dispatchTouchEvent(android.view.MotionEvent)), [onInterceptTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#onInterceptTouchEvent(android.view.MotionEvent)) 가 호출된다. 마지막으로 Button의 [dispatchTouchEvent](https://developer.android.com/reference/android/view/ViewGroup.html#dispatchTouchEvent(android.view.MotionEvent)), [onTouchEvent](https://developer.android.com/reference/android/view/View.html#onTouchEvent(android.view.MotionEvent))가 호출된다.

위의 과정은 MotionEvent가 [ACTION_DOWN](https://developer.android.com/reference/android/view/MotionEvent.html#ACTION_DOWN) 일때 발생한다.

MotionEvent가 [ACTION_UP](https://developer.android.com/reference/android/view/MotionEvent.html#ACTION_UP)이 되면 일련의 과정이 반복해서 발생한다. 즉 같은 메서드를 LinearLayout, FrameLayout, Button 순으로 발생시킨다.

# 레이아웃2

```xml
<LinearLayout ...>
    <FrameLayout ...>
        <LinearLayout ...>
        </LinearLayout>
    </FrameLayout>
</LinearLayout>
```

## 터치 이벤트 전달 과정

레이아웃1과 마찬가지로 최상위 뷰(LinearLayout)를 시작으로 최하위 뷰(LinearLayout)로 이벤트가 전달된다. LinearLayout, FrameLayout, LinearLayout 순으로 dispatchTouchEvent, onInterceptTouchEvent가 불린다. 터치한 뷰가 최하위 LinearLayout이기 때문에 다음으로, 최하위 뷰의 onTouchEvent가 호출된다.

이후부터가 레이아웃1과 다르다.

다음으로, 중간의 FrameLayout의 onTouchEvent가 발생하고, 최상위 뷰인 LinearLayout의 onTouchEvent가 발생한다.

단, 최하위 뷰에 클릭이나 터치 리스너를 등록하지 않았기 때문에 ACTION_DOWN일때만 호출된다.

최하위 뷰(LinearLayout)에 클릭리스너를 등록하면 ACTION_DOWN, ACTION_UP이 모두 호출된다. 때문에 최하위 뷰에 Button을 넣었을때와 동일한 이벤트 전달과정을 확인할 수 있다.

그리고 레이아웃1, 2가 동일하게 실제 터치된 최하위 뷰는 onInteceptTouchEvent가 발생하지 않는다.



# 이벤트 가로채기

이벤트 전달과정에서 호출된 메서드는 모두 boolean 형태의 반환타입을 갖는다. 임의로 true를 반환하면 그 즉시 이벤트를 소비하게 된다. 이벤트를 소비하게 되면, 하위 뷰로 이벤트가 전달되지 않는다. 

예를들어 레이아웃1,2에서 중간 뷰의 dispatchTouchEvent에서 true를 반환했다면, 최하위 뷰의 이벤트 메서드는 전혀 호출되지 않는다. 최하위 뷰가 없는것 같이 이벤트가 전달된다.

