---
layout: post
title: "부분 라운드 UI(Round)"
description: 
categories: [Android]
tags: [Android]
---

# 중첩 View를 통한 부분(상단) 라운드 효과

```xml
<androidx.constraintlayout.widget.ConstraintLayout
	...
    >
  <FrameLayout
      android:layout_width="0dp"
      android:layout_height="0dp"
      android:layout_marginEnd="15dp"
      android:layout_marginStart="15dp"
      android:layout_marginTop="20dp"
      app:layout_constraintDimensionRatio="2:1"
      app:layout_constraintEnd_toEndOf="parent"
      app:layout_constraintStart_toStartOf="parent"
      app:layout_constraintTop_toTopOf="parent"
      >
    <androidx.cardview.widget.CardView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_gravity="center"
        android:layout_marginBottom="-10dp" // here!!
        app:cardBackgroundColor="@color/color_FF333333"
        app:cardCornerRadius="10dp"
        >
      <ImageView
          android:layout_width="match_parent"
          android:layout_height="match_parent"
          android:foreground="?android:attr/selectableItemBackground"
          android:scaleType="fitXY"
          android:src="@drawable/ic_warning"
          />
    </androidx.cardview.widget.CardView>
  </FrameLayout>
    ...
</androidx.constraintlayout.widget.ConstraintLayout>
```

상단의 예제처럼 ConstraintLayout을 반드시 사용할 필요는 없다.

- FrameLayout 안에 CardView, 다시 그 안에 ImageView를 넣는다.
- CardView의 marginBottom에 마이너스 값을 준다.(here!!)
- marginBottom의 마이너스 dp는 radius 값에 따라 다르게 직접 입력한다.
- FrameLayout은 wrap_content으로 해야 한다.



*각각의 레이아웃 width, height는 적당하게 설정한다.*