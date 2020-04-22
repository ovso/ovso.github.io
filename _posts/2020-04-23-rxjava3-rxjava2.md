---
layout: post
title: "RxJava3의 차이점"
categories: [Android]
tags: [RxJava]
---

# RxJava3 릴리즈

[RxJava3가 2020년 2월 14일에 출시됐다.](https://github.com/ReactiveX/RxJava/releases/tag/v3.0.0)

# 차이점?

RxJava2와의 차이점이 어떻게 될까? 결론부터 말하자면, [Major]([https://velog.io/@slaslaya/Semantic-Versioning-2.0.0-MAJOR-MINOR-PATCH%EC%99%80-%EB%AA%85%EC%84%B8%EC%97%90-%EA%B4%80%ED%95%98%EC%97%AC](https://velog.io/@slaslaya/Semantic-Versioning-2.0.0-MAJOR-MINOR-PATCH와-명세에-관하여))가 바뀔 정도의 차이는 아직(?) 없다.

RxJava2로부터 사소한 변경, 정리, 개선을 진행한 버전이다. 좀 더 구체적으로는, RxJava 3.0.0 릴리즈에서는 이전 버전 [RxJava2.2.x의 유지 보수 모드](https://github.com/ReactiveX/RxJava/wiki/What's-different-in-3.0#2x-now-in-maintenance-mode)라는 것이다. 이것은 버그 수정만 받아들이고 적용됨을 의미한다. 새로운 연산자나(operator) 문서 변경 사항은 없다.

# RxJava2의 행보는?

RxJava 2.x는 2021 년 2 월 28 일까지 지원되며, 이후 해당 지점의 모든 개발이 중단된다.

# 참고

[https://github.com/ReactiveX/RxJava/wiki/What's-different-in-3.0](https://github.com/ReactiveX/RxJava/wiki/What's-different-in-3.0)