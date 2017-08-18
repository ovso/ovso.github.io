---
layout: post
title: "Android .gitignore"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/08/18/
---

.gitignore를 설정에는 두 가지가 있다. SourceTree의 기본설정에서 설정하는 방법과, 프로젝트의 루트에 설정하는 방법이 그 두가지다. 안드로이드 프로젝트만 진행한다면, 글로벌 .gitignore를 해도 되겠지만, 여러 종류의 프로젝트를 Github로 관리 하고 있다면 두 번째의 방법이 낫다고 생각한다. 

#### .gitignore(깃 초기화시)

```
*.iml
.gradle
/local.properties
/.idea/workspace.xml
/.idea/libraries
.DS_Store
/build
/captures
.externalNativeBuild
```

#### .gitignore(사용자 정의)

```
gradle.properties
gradlew
gradlew.bat
settings.gradle
.gitignore
```

*SourceTree로 깃을 초기화 할 때 생성되는 .gitignore와 파일을 보며 직접 정의한 .gitignore를 합쳐 사용하면 되겠다.*

나도 현재 쓰고 있고 많은 사람들이 사용하고 있는 [.gitignore](https://github.com/github/gitignore/blob/master/Android.gitignore){:target="_blank"}도 참고하자