---
layout: post
title: "앱 서명(App signing)"
categories: [Android]
tags: [Android]
---

# .pem 파일 생성

```
keytool -export -rfc -alias xx -file upload_certificate.pem -keystore xx.jks (enter)
password : xxxxxxx (enter)
인증서가 <upload_certificate.pem> 파일에 저장되었습니다.
```

