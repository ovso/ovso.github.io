---
layout: post
title: "(흔한)플레이스토어 등록거부 사유(reject)"
categories: [개발자정책]
tags: [google]
---

# 사기성 광고

```
광고가 앱의 사용자 인터페이스 또는 운영체제의 알림 또는 경고인 것처럼 가장하거나 속여서는 안 됩니다. 각 광고가 어떤 앱에서 게재되었는지 사용자가 명확히 알 수 있어야 합니다. - 구글 -
```

![](https://services.google.com/fh/files/helpcenter/playpolicy-ads01.gif)





[https://play.google.com/intl/ko_ALL/about/monetization-ads/ads/disruptive/](https://play.google.com/intl/ko_ALL/about/monetization-ads/ads/disruptive/)

# 실제 사례

카카오톡 공유를 하기 위해 카카오톡 공유 버튼을 만들었다. 버튼을 누르면 앱을 실행한다. 앱이 없다면 플레이스토 다운로드 페이지로 이동한다. 이를 사기성 광고 사유로 '등록 거부' 당하거나 콘솔에서 앱이 삭제 되었다.

# 해결책

앱 외부 화면을 호출할 때는 반드시 별도의 팝업으로 공지를 하고 이동하는 UI를 사용한다.


![](https://developer.android.com/images/ui/dialog_buttons.png?hl=ko)