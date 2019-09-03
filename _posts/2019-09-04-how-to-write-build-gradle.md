---
layout: post
title: "build.gradle 작성법"
categories: [Android]
tags: [Android]
---

# 서론

안드로이드 프로젝트에서 build.gradle파일은 두 개다. 하나는, 프로젝트 수준의 build.gradle. 다른 하나는, 앱모듈 수준의 build.gradle이다. 개발자들이 자주 편집하는 곳은 앱모듈 수준의 build.gradle이다. 왜냐하면, 유용한 오픈소스를 추가, 제거, 업데이트해야 하기 때문이다. 

추가한 오픈소스가 많아지는 것만으로도 build.gradle이 복잡할 수 있다. 복잡하게 보이는 build.gradle을 정리하는 것만으로도 가독성이 향상될 수 있고 관리하기도 용이하니 시도해볼만하다.

# 본론

작성법은 거창할게 없다. 스스로 보기에 좋아보이는, 다른 개발자의 스타일을 차용(베껴?)오면 된다. 필자가 알고 있는 작성법은 세 가지로 나뉜다.

### 첫째,

```groovy
  implementation 'com.jakewharton:butterknife:10.1.0'
  annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0'
```

### 둘째,

```

```



### 셋째,



# 결론