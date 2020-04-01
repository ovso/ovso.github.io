---
layout: post
title: "오류 유형의 힘(The Power of Types for Errors)"
categories: [Android]
tags: [Kotlin]
---

# 오류 유형의 힘

KotlinConf 2019에서 유형의 힘에 대해 이야기했습니다. 본질적으로, 나는 우리가 코드에서 사용하는 프리미티브의 수를 제한하여 커스텀 타입을 대신 사용하는 것에 대해 논의했습니다. 이러한 방식으로 개발자는 컴파일러를 사용하여 버그 가능성을 줄일 수있을뿐만 아니라 더 읽기 쉽고 자체 문서화 된 유형을 얻을 수 있습니다. 오늘 나는이 대화에서 내가 말한 것을 상기시키는 상황에 부딪쳤다.

## 모든 것이 시작된 곳

Android 앱 가입 절차의 일부인 추적 코드를 작성하고있었습니다. 사용자가 겪을 수있는 오류 상태로부터 통찰력을 향상시키는 것이 목표였습니다. 이는 로컬 UX 결함 일 수 있지만 타사를 통해 로그인 할 때 발생하는 문제 일 수도 있습니다.

내 초기 코드는 다음과 같습니다:

```kotlin
fun onError(type: ErrorType, details: String? = null) {
  // ...
}
```

**ErrorType**은 정확한 오류 유형을 지정하는 열거 형입니다.

```kotlin
enum class ErrorType {
   IO,
   PASSWORD_INVALID,
   RECAPTCHA_FAILED
// ...
}
```

추가 매개 변수 세부 사항은 일부 오류가 필요할 수있는 추가 정보입니다. 그러나 우리가 가진 메소드 서명을 자세히 살펴 보겠습니다.

```kotlin
fun onError(type: ErrorType, details: String? = null)
```

세부 사항이 오류를 설명하기 때문에이 두 매개 변수가 함께 있다고 말합니다. 즉, 커플 링이 매우 빡빡하지만 여전히 관계가 없습니다! 오히려 두 개의 독립적 인 매개 변수 일뿐입니다.

## 이 관계를 표현합시다

이 상황을 개선하기위한 첫 번째 아이디어는 두 매개 변수를 한 유형으로 랩핑하는 것입니다.

```kotlin
data class Error(val type: ErrorType, val details: String?)
// Our tracking method now looks much nicer:
fun onError(error: Error) {
  // ...
}
```

이 작업을 수행하면 메소드에 매개 변수가 하나만 있으므로 독자에게 전달할 내용이 명확 해집니다.

```kotlin
fun onError(error: Error) {
  //...
}
```

두 개의 독립적 인 매개 변수가 있었을 때와 마찬가지로 잘못된 것을 전달할 수 없습니다.

이것은 한때 영국 컴퓨터 과학자 로빈 밀너 (Robin Milner)의 말에서 내가 한 말을 인용 한 것이다. : 

> 잘 입력 된 표현은 “잘못 될 수 없습니다.”

## 우리는 더 잘할 수있다

그러나 여기서 멈추지 말아야합니다! 메소드가 저장되었지만 두 매개 변수가 단순히 **Error** 클래스로 이동되었으므로 여전히 숨겨진 관계가 있습니다!

그래서 우리는 이것을 표현해야합니다. 개발자가 언제, 언제 또는 무엇을 통과해야하는지 추측해서는 안됩니다!

열거 형 값은 단순히 상수이며 정의에 따라 불변이므로 열거 형 유형은 이러한 개별 세부 정보를 보유 할 수 없습니다. 그러나 봉인 클래스를 사용하면이를보다 정확하게 모델링 할 수 있습니다. :

```kotlin
sealed class Error {
  object InvalidPassword : Error()
  object RecaptchaFailed : Error()
  class IOError(val details: String): Error()
// ...
}
```

## 다음 단계

열거 형이 지원하지 않는 또 다른 것은 계층 구조입니다. 열거 형 값은 한 수준의 추상화에서 평평합니다.

이를 보완하기 위해 일반적으로 이름을 통해이를 표현합니다.

```kotlin
enum class ErrorType {
   IO,
   EMAIL_DENIED,
   EMAIL_EXISTING,
   EMAIL_INVALID,
   PASSWORD_INVALID,
   RECAPTCHA_FAILED
}
```

그러나 내가 이야기에서 언급했듯이 :

> 유형이 될 수있는 이름을 입력하지 마십시오.

이제 열거 형을 제거하면 계층 구조를 쉽게 도입 할 수 있습니다.

```kotlin
sealed class Error {
  sealed class SignInError {
     sealed class EmailError : SignInError() {
        object Denied : EmailError()
        object Invalid : EmailError()
        object Existing : EmailError()
     }
     class IOError(val message: String): SignInError()
  }
//...
}
```

알다시피, 이메일 관련 오류를 로그인 사용 사례로 제한 할 수도 있습니다. 이런 식으로 우리는 단순히 유형을 사용하여 한 시점에서 하나의 유스 케이스와 관련된 모든 오류를 처리 할 수 있습니다.

## 그리고 나는 보았다…

코드의 모든 기능을 사용하지 않는 또 다른 코드 영역이있었습니다. 소셜 네트워크를 통해 로그인을 시도 할 때 코드는 일반적으로 먼저 타사 라이브러리 호출을 처리 한 다음 사용자 시스템으로의 로그인 과정을 진행합니다. 반면에 이메일을 통해 직접 로그인하면이 첫 단계가 없습니다.

코드에는 다음과 같은 것이 있습니다.:

```kotlin
// An error on Facebook/Google appeared.
fun onAuthenticationError(method: Method, error: Error)
```

여기서 `Method`는 또 다른 열거 형입니다.

```kotlin
enum class Method {
   FACEBOOK, GOOGLE, EMAIL
}
```

그러나 코드 주석에서 알 수 있듯이 `onAuthenticationError ()`는 Google 및 Facebook 전용입니다. 개발자는 `Method.EMAIL`을 사용하여이를 호출 할 수 없어야합니다. 그러나 아무것도 우리를 방해하지 않습니다. 유효하지 않은 입력에서 어설 션을 추가하고 충돌을 일으킬 수 있지만 런타임 충돌과 숨겨진 코드 주석은 모두 적합하지 않습니다. 이상적인 시나리오는 컴파일러가이를 미리 확인할 수 있다는 것입니다.

새로운 오류 계층 구조 덕분에 다음과 같은 유형으로 쉽게 해결할 수 있습니다.:

```kotlin
fun onAuthenticationError(error: Error.SocialError)
```

여기서 `SocialError`는 봉인 된 `Error`의 또 다른 전문 분야입니다.

```kotlin
sealed class Error {
 sealed class SignInError {
   sealed class SocialError(val message: String) : SignInError() {
       class GoogleError(message: String) : SocialError(message)
       class FacebookError(message: String) : SocialError(message)
   }
  // ...
 }
}
```

## 결론

요약하면 유형은 매우 강력합니다. 위에서 언급 한 변경 사항으로 인해 코드가 읽기 쉽고 표현력이 향상되었으며 새로운 개발자는 인수로 필요한 유형별로 추적 방법을 사용하는 방법을 간단하게 알 수 있습니다.

다음은이 블로그 게시물에서 기억해야 할 몇 가지 주요 내용입니다.

- 함께 속해 있지만 별도로 취급되는 매개 변수가 있으면 숨겨진 유형이 있는지 확인하십시오.
- 특정 경우에만 사용되는 필드가있는 클래스가 표시되면 숨겨진 유형 계층이있을 수 있습니다.
- 특정 조합 만 허용하는 방법이 있으면 유형으로 고정하십시오.

물론 이에 대해 더 자세히 알고 싶다면 [내 슬라이드](https://speakerdeck.com/dpreussler/the-power-of-types-kotlinconf-2019)에서 더 많은 예제를 보고 [대화 내용](https://www.youtube.com/watch?v=t3DBzaeid74)을 확인하십시오.

