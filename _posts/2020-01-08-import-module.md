---
layout: post
title: "외부 모듈을(Module) 참조로(Link) 가져오기"
categories: [Android]
tags: [Android]
---

# Module을 참조해야 하는 이유

외부 저장소에 모듈을 만든 후, 메인 프로젝트에서 그 모듈을 사용해야 하는 상황에서는 모듈을 참조하는 방법을 권장한다. 메인 프로젝트에서  `import module` 하게 되면 모듈을 복사해온다. 이렇게 해도 문제는 없다. 다만 모듈이 복사된다. 외부에 구현해 놓은 모듈을 버리게(?) 되는 셈이다. 또는 항상 둘다 업데이트 해야 하는 문제가 발생한다.

때문에, Module을 참조하여 사용해야 한다.

# Module 참조 방법

메인 프로젝트 루트에 있는 `setting.gradle` 을 아래와 같이 수정한다.

```groovy
include ':App' // 메인 프로젝트
include ':myModule'
project(':myModule').projectDir = new File('../MyModuleProject/myModule)
```

위와 같이 설정하면, 외부 모듈을 참조할 수 있다. 그리고 앱 수준의 `build.gradle`에 종속성을 추가해줘야 한다.

앱 수준의 `build.gradle` :

```groovy
android {
  ...
    dependencies {
      ...
      implementation project(':myModule')
    }
}
```

위와 같이 추가해주어야만, 모듈의 API를 사용할 수 있다.