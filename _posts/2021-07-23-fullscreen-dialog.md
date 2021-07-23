---
layout: post
title: "전체화면(Full screen) 다이얼로그 만들기"
categories: [Android]
tags: [Android]

---

# 전체화면(Full screen) 다이얼로그 만들기

가끔 전체화면(Full screen) 팝업을 만들어야 할 때가 있다. 만드는 방법은 여러가지가 있다.

```tex
- Dialog로 만들기
- AlertDialog로 만들기
- Activity로 만들기
- DialogFragment로 만들기
```

결과가 기획의도에 맞다면, 어떻게 만들던지 상관없다. 

필자의 경우 [DialogFragment](https://developer.android.com/reference/androidx/fragment/app/DialogFragment?hl=en) 에 실망한 경우가 있다. 동작하는 속도가 액티비티 또는 다이얼로그보다 늦는 것 같은 느낌(?) 그리고 `깜빡거림`이 있었기 때문이다. Activity로 만들면 생명주기에 영향을 받는다. 생명주기에 영향을 받기 싫어서 Activity로도 만들지 않았다. 남은건 Dialog와 AlertDialog다. 필자는 AlertDialog를 커스텀하기로 했다. Dialog를 상속받았기 때문에 사용성이 좋을 것이라는 추측과, 단순히 익숙해서다.

아래와 같이 구현했다. dismiss() 는 onRetryClickListener를 구현하는 쪽에서 해도 무방하다. 사용하는 사람에게 좀 더 dismiss()에 대한 자유도를 줄 수 있다. ()

<script src="https://gist.github.com/ovso/301c4117467302183222541ab80e664b.js"></script>

위와 같이 구현했다. dismiss() 는 onRetryClickListener를 구현하는 쪽에서 해도 무방하다. 그러면 사용하는 코드에서 dismiss()에 대한 자유도를 줄 수 있다. `(() -> Unit)? = null ` --> `((DialogInterface) -> Unit)? = null` 로 사용하면 되겠다.

스타일은 아래와 같다. 입맛에 따라 적절히 편집하면 되겠다.

<script src="https://gist.github.com/ovso/007713d5dc89a63727d6c1ed999b25d0.js"></script>

