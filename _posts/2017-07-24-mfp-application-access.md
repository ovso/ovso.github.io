---
layout: post
title: "Application Access(App Version Update)"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---

## 앱 관리 > 애플리케이션 액세스

*활성, 활성 및 알림, 액세스 사용 안함* 이라는 세 가지의 상태 중 하나를 선택할 수 있다. '활성'이 기본값이다.

*활성 및 알림, 액세스 사용 안함*은 보통 애플리케이션 업데이트를 유도하기 위해 사용한다.

### 액세스 사용 안함

액세스 사용 안함 상태에는 '최신 버전 URL'을 적는 란이 나온다. URL을 적으면, 업데이트 유도 다이얼로그의 UI가 변한다.

Upgrade 버튼과 Close버튼이 노출된다.

URL은 아래와 같이 적는다.

```
ibmappctr://show-app?id=패키지명(iOS:id)
```

Upgrade버튼을 누르면 외부의 앱센터로 이동한다.