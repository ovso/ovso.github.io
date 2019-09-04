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

작성법은 거창할게 없다. 스스로 보기에 좋아보이는, 다른 개발자의 스타일을 차용(베껴?)하면 된다. 필자가 알고 있는 작성법은 세 가지로 나뉜다.

### 작성법 1

```groovy
dependencies {
	implementation 'com.jakewharton:butterknife:10.1.0'
  annotationProcessor 'com.jakewharton:butterknife-compiler:10.1.0'  
}  
```

### 작성법 2,

```groovy
dependencies {
	implementation "com.jakewharton:butterknife:$butterknife_version"
  annotationProcessor 'com.jakewharton:butterknife-compiler:$butterknife_version"
}
```

이와 같이 작성하기 위해서는, 프로젝트 최상위에 gradle파일이 존재해야 한다.( ex - versioning.gradle)

```groovy
ext {
  .
  ..
  ...
  butterknife_version = '10.1.0'
  ...
  ..
  .
}
```

그리고, 프로젝트 수준의 build.gradle에 versioning.gradle에 대해서 선언해야 한다. 그래야 모듈 수준의 build.gradle에서 참조 할 수 있다.

모듈 수준의 build.gradle

```groovy
buildscript {
	...
}

allprojects {
	...
}
...
  
apply from: 'versioning.gradle'
```



### 작성법 3,

```groovy
dependencies {
	implementation deps.butterknife
  annotationProcessor deps.butterknife
}
```

필자가 최근에 사용하기 시작했고, 앞으로도 자주 사용할 작성법이다.

versions.gradle이 필요하고, 모듈 수준의 build.gradle에서 이를 참조할 수 있도록, 프로젝트 수준의 build.gradle에 선언해야 한다.

먼저, versions.gradle 이다.

```groovy
ext.deps = [:]
def versions = [:]
versions.room = "2.0.0"
versions.junit = '4.12'
versions.runner = '1.0.2'
versions.espressocore = '3.1.0'
versions.appcompat = '1.0.2'
versions.lifecycle = "2.0.0"
versions.support = "28.0.0"
...
..
.
def deps = [:]

def jetbrains = [:]
jetbrains.kotlin_reflect = "org.jetbrains.kotlin:kotlin-reflect:$versions.koltin_reflect"
jetbrains.anko = "org.jetbrains.anko:anko:$versions.anko"
deps.jetbrains = jetbrains
...
..
.
static def addRepos(RepositoryHandler handler) {
  handler.google()
  handler.jcenter()
  handler.mavenCentral()
  handler.maven { url 'https://oss.sonatype.org/content/repositories/snapshots' }
  handler.maven { url 'https://maven.fabric.io/public' }
  handler.maven { url "https://jitpack.io" }
}

ext.addRepos = this.&addRepos
```

상단부에 버전을 정의했다. 그 아래는 applicationId를 정의했고, applicationId와 버전을 조합한 것을 볼 수 있다. dependencies 선언한 완전한  경로(?)를 만들어둔 셈이다.

다음은, 프로젝트 수준의 build.gradle이다.

```groovy
buildscript {
    apply from: 'versions.gradle'
    addRepos(repositories)

    repositories {
        google()
        jcenter()
    }
    dependencies {
        classpath class_path.android_gradle
        classpath class_path.kotlin_plugin

        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    addRepos(repositories)
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```

상단부에 versions.gradle 참조를 위한 선언이 있다. 그 아래, addRepos에 repositories를 넘겨준다. addRepos는 versions.gradle에 선언한 함수다. 그 함수에서, 각종 레파지토리를 설정한다.

# 결론

작성법 1은, 오픈소스가 많아지면 많아질수록 가독성이 정말 떨어진다. 간단한 프로토타입을 만들기 위해서라면 가독성이 떨어지더라도 괜찮을것 같다. 오픈소스가 업데이트 됐을때 개발자에게 알려준다.



작성법 2는, 참조형태지만 추가한 오픈소스가 많아지면 역시 가독성이 떨어진다. 그러나 오픈소스가 업데이트 됐을때 안드로이드 스튜디오에서 친절하게 알려준다. 그게 장점이다.



작성법 3은, 작성법 2보다 gradle 편집을 좀 더 해야 하는 불편함은 있지만, 앱 수준 build.gradle의 dependencies에서 눈에 띄게 가독성이 좋아졌다.  가독성도 좋지만, dependencies에 추가하기가 아주 편하다는게 필자가 생각하는 가장 큰 장점이다. 완전체(?)를 versions.gradle에 모두 만들어 놓고, 필요할때마다 앱수준 build.gradle에 간단히 선언만 하면 된다.

versions.gradle에 미리 만들어 놓는다는 것은, 오픈소스를 당장 사용하지 않더라도, 쓸모 있어 보이는 오픈소스나, 미래에 사용할 가능성이 있는 오픈소스를 applicationId, version 조합으로 미리 정의해 놓는다는 말이다.

한가지 단점이라면, 오픈소스가 업데이트했을때 안드로이드 스튜디오가 그것을 감지하지 못한다는 것인데 반드시 최신버전을 설치해야 하는 것이 아니라면, 작성법 3이 가장 쓸만하다.



선택은 개발자의 몫