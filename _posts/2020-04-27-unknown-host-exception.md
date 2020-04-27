---
layout: post
title: "UnknownHostException"
categories: [Android]
tags: [Exception]
---

# 예외 발생

앱 테스트 중에 아래와 같은 예외가 발생했다.

```
java.net.UnknownHostException: Unable to resolve host “stg-oauth.xxxxx.co.kr”: No address associated with hostname
```

# 원인

와이파이에 접속했으나 인터넷 연결이 안됐기 때문이었다. 원인은 간단했지만 당시에는 잘 알 수 없었다. 하나는, 이런 예외가 오랜만이었다. 또 하나는, 단말에서 접속되어 있는 와이파이 주소가 맥에서는 잘 동작하고 있었으니까..

# 해결

접속 WIFI 명을 바꾸어 해결했다. 그러나 사전에 서버 개발자분께 문의해 놓은 상황이라 그 분의 시간을 한참 빼앗았다. 그 분도 오랜만에 보내는 예외라서 그런지 금방 알 수 없았다. 거의 동시에 서로가 알게 됐을땐... 나 스스로 [민폐](https://ko.dict.naver.com/#/entry/koko/4d043588f673487699adebf3ab004bb4)라고 생각했다.

와이파이가 동작하지 않았다.

# 결론

일을 할 때 한번 더 생각해보고 다른 팀원의 도움을 받아야 겠다.

# 

