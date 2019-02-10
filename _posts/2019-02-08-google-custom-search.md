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

# Custom Search API 사용해보기

[https://developers.google.com/custom-search/v1/cse/list](https://developers.google.com/custom-search/v1/cse/list)

key는 필요없지만 cx(검색 엔진 ID)가 필요하다.

# Google Custom Search 설정

Coming soon..

