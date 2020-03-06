---
layout: post
title: "Terminal realpath(전체 경로, 실제 경로)"
categories: [Terminal]
tags: [Terminal]



---

# Realpath

OS X에서 터미널을 사용하다 보면 실제 경로를 보고 싶을 때가 있다. 또는 필요할 때가 있다. 그럴땐 `realpath <파일명>` 또는  `<폴더명>` 입력해보자. 그러면 전체 경로를 알 수 있다.

```
peter$ realpath java
/Library/Java/JavaVirtualMachines/jdk1.8.0_241.jdk/Contents/Home/bin/java
```

단, `realpath` 를 사용하려면 `coreutils` 를 설치해야 한다.

## Install

```
peter$ brew install coreutils

Updating Homebrew...
==> Auto-update Homebrew!
Updated 1 tab (homebrew/core).
==> Updated Formulae
cocoapods
....
...
..
.

peter$
```

설치가 완료 되면, `realpath <파일명>` 해보자!

물론, 실제 경로명을 알아내는 것은 `coreutils` 만의 기능은 아니다.

# Go2Shell

*Go2Shell*은 일종의 터미널이다. 개발자들에게 오랫동안 사랑받아왔다. 이를 설치하면 *Finder* 우측 상단에 이모지(**>_<**)가 나타난다. 전체 경로를 알고 싶은 폴더에 들어가서 이모지(**>_<**)를 누르면 *터미널*이 실행되면서 상단 첫 줄에 전체 경로를 나타낸다.  *Go2Shell* 앱스토어에서 내려받을 수 있다.

```
/Library/Java/JavaVirtualMachines/jdk1.8.0_241.jdk/Contents/Home/bin
peter$ 
```

# Finder

*Finder* 를 통한 방법은 가장 쉬운 방법일 수 있다. 별도의 설치도 필요없다. *Finder* 로 원하는 경로에 들어간 후 파일 또는 폴더를 *터미널 *에 끌어 넣으면 전체 경로가 나타난다.

```
peter$ /Library/Java/JavaVirtualMachines/jdk1.8.0_241.jdk/Contents/Home/bin/java
```

