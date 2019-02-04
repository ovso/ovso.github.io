---
layout: post
title: "Unresolved references"
categories: [Android]
tags: [Android]
---

# Unresolved references

연휴로 인해 며칠만에 안스를 열었더니 상당수의 라이브러리에서 컴파일 에러가 발생했다. 빌드는 잘 된다. 코딩은 할 수 없다.

![](https://ovso.github.io/images/unresolved-references.png)

```
UnResolved references
```

# 원인

설정이 꼬여서(?)

# 해결책

루트의 .idea와 build를 삭제하면 된다. 두 디렉토리를 삭제하자마자 안스는 종료된다. 안스를 다시 시작하면 각종 설정이 초기화 되면서 Unresolved references가 사라진 것을 확인할 수 있다.

[https://stackoverflow.com/questions/49545037/android-studio-3-1-erroneous-unresolved-references-in-editor](https://stackoverflow.com/questions/49545037/android-studio-3-1-erroneous-unresolved-references-in-editor)