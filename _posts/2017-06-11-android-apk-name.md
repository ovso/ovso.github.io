---
layout: post
title: "Android APK Auto sign and Change name"
description: 
categories: [Android]
tags: [Gradle]
redirect_from:
  - /2017/08/18/
---

(개인적인)쎄미프로젝트가 아니라면 APK를 생성하기 위해 서명하고, 이름을 바꾸는 것은 필수다. 간단하게 보이는 이 과정을 매번 손수 하는 것은 불편하고 귀찮다. 프로젝트 수가 늘어나면 더하다. 때문에 자동으로, APK를 생성하고 이름을 바꾸는 것은 필수다.

# APK 자동 생성

### keystore 정보 정의

프로젝트의 root 경로에 keystore.properties 파일 생성 후 아래와 같이 keystore 정보를 정의한다. 물론, keystore.properties 파일은 외부에 노출되어서는 안된다. 

```
storePassword = 1234
keyPassword = 1234
keyAlias = jaeho
storeFile = /Users/jaeho/Documents/workspace/Demo/demo.jks
```

### (Module)build.gradle에서 keystore 정보 가져와서 buildTypes 정의

```groovy
apply plugin: 'com.android.application'

// keystorePropertiesFile 변수를 만들고 프로젝트 폴더의 keystore.properties 파일로 초기화 한다.
def keystorePropertiesFile = rootProject.file("keystore.properties")
// keystoreProperties 변수를 만들고 Properties() 객체를 초기화 한다.
def keystoreProperties = new Properties()
// keystore.properties 파일을 keystoreProperties 객체에 로드한다.
keystoreProperties.load(new FileInputStream(keystorePropertiesFile))

android {
  ...
  signingConfigs {
    release {
      keyAlias keystoreProperties['keyAlias']
      storePassword keystoreProperties['storePassword']
      keyPassword keystoreProperties['keyPassword']
      storeFile file(keystoreProperties['storeFile'])
    }
  }
  
  buildTypes {
    release {
      signingConfig signingConfigs.release
    }
    debug {}
  }
  ...
}
```

### APK 확인

build APK가 성공하면,

```
모듈/build/outputs/apk에서 모듈-release.apk를 확인할 수 있다.
```

![](https://ovso.github.io/images/2017-06-11-apk-name-01.png)

# APK명 변경

### (Module)build.gradle의 buildTypes 변경

```groovy
android {
  ...
  buildTypes {
    release {
      signingConfig signingConfigs.release
      applicationVariants.all { variant ->
        variant.outputs.each { output ->
          def formattedDate = new Date().format('yyyyMMdd_HHmmss')
          def apkName = "Demo_" + variant.versionName +
              "_" +
              variant.versionCode +
              "_" +
              formattedDate +
              "_release.apk"
          output.outputFile = new File(output.outputFile.parent, apkName)
        }
      }
    }
    debug {}
  }
  ...
}
```

### 변경 된 APK명

```
Demo_0.0.1_1_20170611202730_release.apk // Demo_버전명_버전코드_년월일시분초_release.apk
```

