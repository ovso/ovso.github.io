---
layout: post
title: "IntelliJ Java Code Style 적용하기”
description: 
categories: [other]
tags:[other]
redirect_from:
  - /2017/08/18/
---

코드를 일관성 있게 작성하려면, 개발환경에 이미 만들어진 Code Style을 적용하는 것이 좋다. Code Style을 직접 만들기는 쉽지 않다. 때문에 널리 사용되고 있는 Code Style을 사용하는 것이 좋다. IntelliJ에서 필자가 권장하는 Code Style은 세 가지가 있다. Square 스타일, Google Style, Android Style이 그것이다.

## Square Java Code Style

[Square사](http://square.github.io/){:target="_blank"}의 Github 저장소에서 [Java-Code-Style](https://github.com/square/java-code-styles){:target="_blank"}을 다운로드 후 설치한다.

설치 방법은 매우 간단하다.

```
install.sh
```

```
Macui-MacBook-Pro:java-code-styles jaeho$ ./install.sh
Installing Square IntelliJ configs...
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/Square.xml -> /Users/jaeho/Library/Preferences/IntelliJIdea2017.1/codestyles/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/SquareAndroid.xml -> /Users/jaeho/Library/Preferences/IntelliJIdea2017.1/codestyles/SquareAndroid.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/inspection/Square.xml -> /Users/jaeho/Library/Preferences/IntelliJIdea2017.1/inspection/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/options/editor.codeinsight.xml -> /Users/jaeho/Library/Preferences/IntelliJIdea2017.1/options/editor.codeinsight.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/Square.xml -> /Users/jaeho/Library/Preferences/IdeaIC2017.1/codestyles/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/SquareAndroid.xml -> /Users/jaeho/Library/Preferences/IdeaIC2017.1/codestyles/SquareAndroid.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/inspection/Square.xml -> /Users/jaeho/Library/Preferences/IdeaIC2017.1/inspection/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/options/editor.codeinsight.xml -> /Users/jaeho/Library/Preferences/IdeaIC2017.1/options/editor.codeinsight.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/Square.xml -> /Users/jaeho/Library/Preferences/AndroidStudio2.3/codestyles/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/SquareAndroid.xml -> /Users/jaeho/Library/Preferences/AndroidStudio2.3/codestyles/SquareAndroid.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/inspection/Square.xml -> /Users/jaeho/Library/Preferences/AndroidStudio2.3/inspection/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/options/editor.codeinsight.xml -> /Users/jaeho/Library/Preferences/AndroidStudio2.3/options/editor.codeinsight.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/Square.xml -> /Users/jaeho/Library/Preferences/AndroidStudioPreview3.0/codestyles/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/codestyles/SquareAndroid.xml -> /Users/jaeho/Library/Preferences/AndroidStudioPreview3.0/codestyles/SquareAndroid.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/inspection/Square.xml -> /Users/jaeho/Library/Preferences/AndroidStudioPreview3.0/inspection/Square.xml
/Users/jaeho/Documents/workspace/java-code-styles/configs/options/editor.codeinsight.xml -> /Users/jaeho/Library/Preferences/AndroidStudioPreview3.0/options/editor.codeinsight.xml
Done.

Restart IntelliJ and/or AndroidStudio, go to preferences, and apply 'Square' or 'SquareAndroid'.
Macui-MacBook-Pro:java-code-styles jaeho$ 
```

진행은 순식간에 이루어진다. 그리고 **OS에 설치되어 있는 모든 IntelliJ기반의 IDE에 Code Style을 적용**한다.

## Google Java Code Style

[Google styleguide 저장소](https://github.com/google/styleguide){:target="_blank"}에서 **intellij-java-google-style.xml**을 다운로드

## Google Android Code Style

[Android 공식 스타일을 다운로드](https://github.com/android/platform_development/blob/master/ide/intellij/codestyles/AndroidStyle.xml){:target="_blank"}

## IntelliJ(Android Studio) 에서 Code Style 설정

![](https://ovso.github.io/images/2017-06-03-code-style-01.png)

Scheme 드랍다운메뉴 우측에 버튼을 눌러 *style.xml을 불러오면(import). 드랍다운메뉴에 스타일이 추가된다.

Scheme를 보면 이미 여러개의 스타일이 추가되어 있다. 하나씩 적용해보자. 자신에게 맞는 스타일을 사용하면 된다.  필자가 사용해본 바, Square사의 스타일이 가장 엄격하다.

혹시 적용되지 않는다면, IntelliJ를 재시작 하자.