---
layout: post
title: "Android 애플리케이션 ID 변경(패키지명 바꿔서 빌드하기)"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/08/18/
---

때때로, 출시된 앱과 출시 예정인 앱을 동시에 봐야할 때가 있다. 이럴 땐, build.gradle(Module)에서 애플리케이션 ID(패키지명)를 바꿔서 빌드하면 된다. 아래는 애플리케이션 ID에 접미사를 붙여 빌드한다. 방법은 두 가지가 있다. 필자는 아래와 같은 방법으로 해봤다. 버전네임도 변경 가능하다.

```groovy
android {
    ...
    buildTypes {
        debug {
            applicationIdSuffix ".debug"
          	versionNameSuffix "-debug"
        }
    }
}
```

또 다른 방법도 있으니 참고하자.

[애플리케이션 ID 설정](https://developer.android.com/studio/build/application-id.html){:target="_blank"}

