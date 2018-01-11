---
layout: post
title: "Git과 Github"
description: 
categories: [other]
tags: [git]
redirect_from:
  - /2018/01/10/
---

# Git과 Github

평소, 깃을 사용하고 있다. 그러나 깃에 대해서 아는 것보다 모르는 것이 많다. 부딪히면서 차근차근 알아가도록 하자!

# Reset과 Rebase

[개발바보들 1화 – git “Back to the Future”](http://devpools.kr/2017/01/31/%EA%B0%9C%EB%B0%9C%EB%B0%94%EB%B3%B4%EB%93%A4-1%ED%99%94-git-back-to-the-future/){:target="_blank"}

[[초보용] Git 되돌리기( Reset, Revert )](http://devpools.kr/2017/02/05/%EC%B4%88%EB%B3%B4%EC%9A%A9-git-%EB%90%98%EB%8F%8C%EB%A6%AC%EA%B8%B0-reset-revert/){:target="_blank"}

# .gitignore가 동작하지 않을 때

.gitignore가 동작하지 않을 때가 있다. 캐시를 날리고 다시 추가하면 된다.

[중요한 것은 아래와 같이 수행하기 전에 변경한 모든 것은 커밋되어 있어야 한다.](https://stackoverflow.com/questions/25436312/gitignore-not-working){:target="_blank"}

```
git rm -rf --cached . // 모든 파일이 git에서 제거된다.
git add . 			  // 모든 파일이 git에 추가(commit)된다.
```