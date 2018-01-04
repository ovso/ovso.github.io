---
layout: post
title: "DialogFragment에서 AlertDialog사용"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2018/01/04/
---

# 전제조건

DialogFragment의 onCreateDialog 메서드에서 AlertDialog.Builder를 구현했다.

DialogFragment는 하단 버튼을 제공하지 않는다. 개발자가 만들어 줘야 한다. 때문에 DialogFragment와 AlertDialog를 조합하면 UI를 구현하기에 편리하다.

# AlertDialog의 버튼 참조

## onStart()

DialogFragment에서 생성된 AlertDialog의 버튼을 참조하려면 onStart()에서 접근해야 한다. 가령, onCreateActivity()에서는 참조가 안된다. NPE를 볼수도 있다. 그러나 onStart()에서는 참조가 가능하다. 이유는 모른다. onStart()에서 멤버 변수에 AlertDialog를 할당해 놓았더라도 onStart()이후의 생명주기에서 onStart()에서 할당된 AlertDialog를 사용하려 하면 NEP를 구경하게 된다. 그러니 **반드시 onStart()에서 할당하고 사용까지 해야 한다.**

```java
AlertDialog ad = (AlertDialog)getDialog();
Button button = ad.getButton(DialogInterface.BUTTON_POSITIVE);
button.setText("Great!!")
```

