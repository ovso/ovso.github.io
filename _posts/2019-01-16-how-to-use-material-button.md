---
layout: post
title: "MaterialButton, MaterialCardView 사용법"
categories: [Android]
tags: [Android]
---

# MaterialButton 사용법

[https://github.com/material-components/material-components-android/blob/master/docs/components/MaterialButton.md](https://github.com/material-components/material-components-android/blob/master/docs/components/MaterialButton.md)



xml에서 MaterialButton을 선언하면 상, 하단 여백이 이미 지정되어 있다. padding이 아니다. margin은 더더욱 아니다. 바로, android:inset.. 이다. 이걸 몰라서 CardView로 감싼 TextView를 사용했었다. 부끄럽다.

```xml
<com.google.android.material.button.MaterialButton
	android:layout_width="wrap_content"
	android:layout_height="wrap_content"
	android:gravity="center"
	android:text="정보보기"
	android:textColor="@android:color/white"
	android:textSize="15sp"
	android:insetTop="0dp"
	android:insetBottom="0dp"
/>
```



# MaterialCardView 사용법

[https://github.com/material-components/material-components-android/blob/master/docs/components/MaterialCardView.md](https://github.com/material-components/material-components-android/blob/master/docs/components/MaterialCardView.md)





