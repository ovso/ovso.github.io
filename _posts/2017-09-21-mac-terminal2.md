---
layout: post
title: "터미널에서 비어있지 않은 디렉토리 삭제"
description: 
categories: [mac]
tags: [mac]
redirect_from:
  - /2017/09/21/
---

### 디렉토리를 포함한 모든 파일과 폴더를 삭제

```
rm -rf <Directory name>
```

### "Permission denied"일 경우

```
sudo rm -rf <Directory name>
```

### 디렉토리를 삭제할 때 -f를 사용하지 않는 것이 좋다

```
sudo rm -r <Directory name>
```

이것은 이미 터미널에서 삭제하려는 디렉토리와 동일한 레벨에 있다고 가정한다. 그러지 않다면, 아래와 같이 작성한다.

```
sudo rm -r /path/to/<Directory name>
```

### 옵션

-f = 존재 하지 않는 파일은무시하고, 프롬프트는 표시하지 않는다.

-r = 디렉토리와 그 내용을 재귀적으로 제거한다.

-v = 수행중인 작업을 설명한다.