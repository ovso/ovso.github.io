---
layout: post
title: "Google Custom Search(구글 맞춤 검색, CES)"
categories: [Google]
tags: [Google]
---

# Google Custom Search

구글 맞춤 검색 API 를 말한다. 줄여서 CSE..

# 빠른 테스트

```
https://www.googleapis.com/customsearch/v1?q=라면&key=xxxxx&cx=xxxxx&alt=json
```

브라우저 주소창에 위(⬆️)와 같이 입력하면 아래(⬇️)와 같은 결과를 얻을 수 있다. key, cx(검색 엔진 ID)는 Custom Search API를 설정하는 단계에서 얻을 수 있다.

```json
{
    "kind": "customsearch#search",
    "url": {..},
    "queries": {..},
    "context": {..},
    "searchInformation": {..},
    "items":[
        {...},
        {...},
        {...}
    ]
}
```

CSE 검색결과에서, **items** 의 이미지 필드( ..> src) 값이 **x-raw-image**로 시작한다면 이미지 불러오기가 실패한다. 정상적인 이미지가 아니기 때문이다.  [https://groups.google.com/forum/#!topic/google-apis-explorer-users/sUKqX7HD5B0](https://groups.google.com/forum/#!topic/google-apis-explorer-users/sUKqX7HD5B0)

# Custom Search API 사용해보기

[https://developers.google.com/custom-search/v1/cse/list](https://developers.google.com/custom-search/v1/cse/list)

key는 필요없지만 cx(검색 엔진 ID)가 필요하다.

# API 및 서비스

[https://console.developers.google.com](https://console.developers.google.com)

key를 확인할 수 있다. 

[라이브러리](https://console.developers.google.com/apis/library) 메뉴는 사용할 수 있는 모든 API 목록을 보여준다.

# 맞춤 검색 - 검색 엔진 수정

https://www.google.co.kr/cse/

검색 엔진을 설정(수정)할 수 있다.

## 기본사항

### 이미지 검색

이미지 검색이 필요하다면 스위치를 on 한다.

### 검색할 사이트

여러 사이트를 등록한다. 그래야 일반(웹) 검색이든 이미지 검색이든 잘 된다.

![](https://ovso.github.io/images/custom_search_add_site.png)
