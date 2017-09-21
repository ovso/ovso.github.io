---
layout: post
title: "Mac OS에 Consolas 폰트 설치"
description: 
categories: [mac]
tags: [Mac]
redirect_from:
  - /2017/08/18/
---

평소, Mac OS에서 개발하고 Consolas 폰트를 주로 사용한다. Consolas가 [Microsoft의 작품](https://en.wikipedia.org/wiki/Consolas)이라 그런지 Mac OS에는 기본으로 설치되어 있지 않다.

설치해보자!

- *Consolas를 설치하기 위해서는 [HomeBrew](https://brew.sh/index_ko.html)가 필요하다. [HomeBrew](https://brew.sh/index_ko.html)가 설치되어 있지 않다면 아래의 명령어를 Termianl에서 실행해보자.*

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

- *[Consolas 설치 하기](http://ikato.com/blog/how-to-install-consolas-font-on-mac-os-x.html)*

```
1. brew install cabextract
2. cd ~/Downloads
3. mkdir consolas
4. curl -O http://download.microsoft.com/download/f/5/a/f5a3df76-d856-4a61-a6bd-722f52a5be26/PowerPointViewer.exe
5. cabextract PowerPointViewer.exe
6. cabextract ppviewer.cab
7. open CONSOLA*.TTF
```

![](https://ovso.github.io/images/2017-05-22-01-consolas.png)

* *보기*

![](https://ovso.github.io/images/2017-05-22-02-consolas.png)