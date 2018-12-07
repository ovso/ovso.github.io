---
layout: post
title: "DialogFragment fullscreen"
description: Fullscreen on DialogFragment
categories: [Android]
tags: [Android]
---

# DialogFragment fullscreen

아래와 같이 스타일을 적용하면 된다.

```java
public class MyDialogFragment extends DialogFragment {
	@Override public void onCreate(@Nullable Bundle savedInstanceState) {
    	super.onCreate(savedInstanceState);
    	setStyle(DialogFragment.STYLE_NORMAL, android.R.style.Theme_Light_NoTitleBar);
	}
}
```

스타일의 종류를 여러가지다. 적절하게 구현하면 되겠다.

구현하는 곳은 [onCreate](https://developer.android.com/guide/components/fragments?hl=ko){:target="_blank"} 다른 곳에서 구현하면 반영되지 않는다.

