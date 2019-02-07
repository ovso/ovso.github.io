---
layout: post
title: "구글 연관 검색어(google related search)"
categories: [Google]
tags: [Google]
---

# 구글 연관검색어 

구글에서 검색을 시작하면 연관검색어가 리스트로 표시된다. 브라우저에서 아래의 주소를 들어가면 확인할 수 있다. 

## 요청

```
https://www.google.com/complete/search?client=xml&q=라면
```

## 결과

```xml
<toplevel>
	<CompleteSuggestion>
		<suggestion data="라면"/>
	</CompleteSuggestion>
    <CompleteSuggestion>
    	<suggestion data="라면먹고갈래"/>
    </CompleteSuggestion>
    <CompleteSuggestion>
    	<suggestion data="라면 레시피"/>
    </CompleteSuggestion>    
...
..
.
</toplevel>
```

## 요청 2

client 부분을 opera 라고 하면 txt파일을 내려준다. 즉 자동으로 다운로드 된다.

```
https://www.google.com/complete/search?client=opera&q=라면
```

아래는 JSON 형태의 텍스트다.

```json
["%s",
 [
  "네이버",
  "sky캐슬",
  "sk엔카",
  "skyscanner",
  "sky캐슬 다시보기",
  "srt",
  "ssg",
  "sbs스페셜",
  "steam",
  "slack"],
 [],
 {
     "google:fieldtrialtriggered":true
 }
]
```

혹시 json형태로도 브라우저에서 확인 가능한지는 좀 더 테스트 해봐야 할 것 같다.