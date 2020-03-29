---
layout: post
title: "안드로이드 메모리 덤프하기(Memory dump)"
categories: [Android]
tags: [Memory dump]
---

# 메모리 덤프란?

## 사전적 의미

* [(특히 적절치 않은 곳에 쓰레기 같은 것을) 버리다](https://en.dict.naver.com/#/entry/enko/11ea47a497684a82aef7f2a797d7f807), ~을 (~에게) 떠넘기다

## 과학사전의 의미

* [컴퓨터 주기억장치와 레지스터에 기억시킨 내용을 보조기억장치에 복사하는 조작. 코어덤프의 준말이다.]([https://www.scienceall.com/%eb%8d%a4%ed%94%84dump/](https://www.scienceall.com/덤프dump/))

## 개발에서의 의미

* 메모리 덤프는 RAM의 모든 정보 내용을 가져 와서 저장소 드라이브에 쓰는 프로세스를 말한다.

# 메모리 덤프하기

메모리를 덤프하는 목적은 다양하다. 필자는 보안취약점을 해결하기 위해 MAC에서 메모리를 덤프했다. 루팅은 윈도우 OS에서 진행했다.

# 전제조건(Precondition)

메모리를 덤프하려면 반드시 단말을 루팅해야한다. 물론 루팅을 하지 않고 진행할 수도 있다고 한다. 다만, 덤프 정보가 제한적이다. 

# 루팅 개요

루팅은 윈도우 OS에서 진행했다. MAC에서 진행하는 방법은 잘 모르겠다. 윈도우 OS에서 루팅을 하려면, 오딘(Odin)이라는 프로그램과 CF-Auto-Root.md5라는 파일이 필요하다.

* 안드로이드 단말을 루팅 가능한 상태로 만든다.(검색)
* 안드로이드 단말의 디버깅 모드를 켠다.
* 단말과 PC를 데이터 케이블로 연결한다.
* 오딘(Odin)프로그램을 실행한다.
* CF-Auto-Root.md5 파일을 불러온다.
* 오딘 프로그램에서 Start 한다.
* 결과는 Pass 또는 Fail로 알려준다.
* Fail 했을때는, 재시도 해보자!

# 루팅한 단말에서 메모리 덤프하기

## ADB(Android Debug Bridge) 설치

ADB는 환경변수로 설정한다. 안드로이드 개발자라면 필수다.

## Python 설치

나중에.. 메모리를 덤프하는 fridump.py를 실행하기 위해 python이 필요하다.

파이썬은Mac 시스템 전용 버전이 있다. 2.7.x 버전이다. 하지만 Deprecated 되었다. 새로운 버전을 설치하라고 권하지만 메모리를 덤프하려는 용도로는 문제가 없다. 새로운 버전을 설치하면 3.x 버전이다. 필자는 결국 2.7.x버전으로 메모리 덤프를 진행했다.

## Frida 설치

```
pip install frida
```

## Frida-server 바이너리 다운로드 및 파일명 수정

### 다운로드

[https://github.com/frida/frida/releases](https://github.com/frida/frida/releases)

```
...
frida-server-12.8.19-android-arm.xz
frida-server-12.8.19-android-arm64.xz
frida-server-12.8.19-android-x86.xz
frida-server-12.8.19-android-x86_64.xz
frida-server-12.8.19-ios-arm.xz
frida-server-12.8.19-ios-arm64.xz
frida-server-12.8.19-ios-arm64e.xz
...
```

본인이 가지고 있는 루팅 대상 단말 사양(cpu)에 따라 파일을 다운로드 한다. 필자의 단말은 갤럭시노트3이기 때문에, frida-server-12.8.19-android-arm.xz 파일을 다운로드했다.

### 파일명 수정

frida-server-12.8.19-android-arm.xz는 압축파일이다. 압축을 풀면 동일한 이름의 파일이 생기는데, *frida-server* 라는 이름으로 수정한다. 확장자는 적지 않는다.

## frida-server 파일을 단말에 넣기, 확인하기, 실행하기

### frida-server 파일을 단말에 넣기

```
adb push frida-server /data/local/tmp
```

```
adb push <파일명> <단말의 경로명>
```

### Frida-server 파일이 잘 들어갔는지 확인하기

adb를 사용한다.

```
adb shell
su          				// 루팅한 앱에서 루트 권한을 얻는 명령어
cd data/local/tmp 	// frida-server 파일이 존재해야 하는 경로
ls									// frida-server가 있는지 확인
```

### Frida-server 프로세스 실행하기

```
cd data/local/tmp		// frida-server 파일이 있는 경로
ls									// 목록 나열
./frida-server &		// 메모리 덤프를 가능하게 하는 프로세스 실행
```

#### 성공

```
12045				// 프로세스의 유니크한 숫자가 표시된다.
```

#### 실패

```
Permission deneied			// 실패 했다면, frida-server에 권한이 없는 것이다.
Chmod 755 frida-server	// frida-server에 권한을 부여한다.
./frida-server & 				// 재시도 한다.
```

### Frida-server 프로세스가 동작중인지 확인하기

```
ps		// Process Status를 뜻하며, frida-server프로세스를 번호로 현재 동작중인지 확인한다.
```

## fridump 다운로드

메모리 덤프를 실제로 실행하기 위해서, 파일이 필요하다.

[https://github.com/Nightbringer21/fridump](https://github.com/Nightbringer21/fridump)

* dumper.py
* fridump.py
* utils.py

## 메모리 덤프하기

터미널에서, frimdump 파일들이 있는 곳으로 이동한다. 아래와 같이 작성하면 메모리 덤프가 진행된다.

```
python fridump.py -U -s -o dump com.xxx.xxx.beta
```

* -U : usb 모드
* -s : Hex(16진수) 파일들의 텍스트만 모아 strings.txt에 출력
* -o : 지정한 디렉토리 경로에 덤프데이터 파일 생성
* dump : 지정한 디렉토리
* com.xxx.xxx.beta : 메모리 덤프 할 대상 앱

명령어를 실행하면, dump 디렉토리에 Hex 파일들이 생성되고 마지막에 strings.txt가 만들어진다. strings.txt 파일에서 중요 정보를 찾아보면 된다. 나왔다면, 보안에 취약한 것이다. 주민번호 뒷자리, 비밀번호, 공인인증서 비밀번호 등이 나올 수 있다.

# 배운 점

* 중요 문자열을 멤버 변수에 담는다면, 메모리에 노출된다.
* 중요 문자열을 Stack, char[] 등으로 관리하면 메모리 상에 노출되지 않는다.
* Stack이나 char[] 등으로 관리 중인 문자열을 합치는 작업은 지역 변수에서 해야만 메모리에 평문으로 노출되지 않는다. 단, 메서드 내에서 참조(클래스, 멤버변수 등..)를 어떻게 하느냐에 따라 중요 문자열이 노출될 수 있다.  GC의 수집 대상이 되도록 개발해야 한다는 말이다.
* 어떤 보안업체는 3초에 한번씩 GC를 돌린다는 곳도 존재한다.(검색)
* EditText에 중요 문자를 입력 중일때는 평문이 노출되지 않는다. 물론, EditText에 TextWatcher를 등록해 멤버변수에 연속적인 텍스트를 담고 있다면, 메모리 상에 노출될 것이다. 그러므로, Stack이나 배열 형태로 문자열을 관리해야 한다.

# 결론

* 중요 정보는 메모리에 노출되지 않도록 시큐어 코딩을 해야 한다.

# 참고

[https://koyoungil.blogspot.com/2019/09/frida.html](https://koyoungil.blogspot.com/2019/09/frida.html)