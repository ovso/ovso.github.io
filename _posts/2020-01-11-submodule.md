---
layout: post
title: "Git submodule(깃 서브모듈)"
categories: [Android]
tags: [git]

---

# What is Submodule?

[서브모듈]([https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88](https://git-scm.com/book/ko/v2/Git-도구-서브모듈))은 외부 모듈을 말한다. 즉, 서브모듈은 여러 앱을 만들때 공통적으로 사용하는 기능을 모아 놓은 하나의 외부 모듈이다. 

외부 모듈을 사용하지 않고 내부 모듈을 사용하면 비효율적이다. 앱 하나에 새로운 하나의 내부 모듈이 생기게 되니 관리 자체가 어려워지기 때문이다.

# Git submodule

안드로이드 스튜디오에서 서브모듈을 사용하는 것은 어렵지 않다. [외부 모듈을 참조](https://ovso.github.io/blog/2020/01/08/import-module/) 하기만 하면 된다. 하지만 커밋과 푸시가 자유롭지 못하다. 안드로이드 스튜디오를 두 개를 실행해 놓은 상태에서 각각 커밋, 푸시를 해야 하는 불편함이 있다. 

때문에 Git submodule 설정이 필요하다.

# Git submodule 설정

```
git submodule add <repository path>
```

현재 잘 안돼고 있다. 설정했음에도 커밋과 푸시가 자유롭지 않다. 풀.. 의 경우 잘 된다. 뭘까?

# Git submodule 제거

<script src="https://gist.github.com/myusuf3/7f645819ded92bda6677.js"></script>

