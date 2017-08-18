---
layout: post
title: "SecurityCheck 클래스"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---



#### SecurityCheck 클래스

[ExternalizableSecurityCheck](https://ovso.github.io/mfp/mfp-class-ExternalizableSecurityCheck/){:target="_blank"}클래스의 부모 클래스다.

```java
/**
 * 보안 검사의 서버 측 상태를 나타낸다.
 * 보안 검사는 분산 캐시에 보관되며 상태 유지이므로 영구 상태를 처리하는 것은 구현자의 책임이다.
 *
 * 작성 :  artem
 * 날짜 : 6/28/15
 */

public interface SecurityCheck extends Externalizable {

    /**
     * 구성 객체를 만들고 제공된 속성에서 필드를 채운다.
     * 오류 및 경고 맵을 사용하여 문제를 보고한다.
     * 배포 중에 호출된다.
     *
     * @param properties 병합된 속성을 읽고 유효성을 검사한다.
     * @return 새로운 설정 객체, 널이 아님
     */
    SecurityCheckConfiguration createConfiguration(Properties properties);

    /**
     * 컨텍스트 및 구성 등록 정보로 보안 점검을 초기화 한다.
     * 이 메소드느느 인스턴스화 후에 호출되며 각 검색시 호출된다.
     * 보안 검사가 구성 데이터를 영구 상태로 유지하면 안된다.
     *
     * @param name                 보안 검사의 이름
     * @param config               createConfiguration(Properties)에 의해 생성된 보안 검사 구성
     * @param authorizationContext 호출 클라이언트의 일시적인 상태에 대한 액세스를 제공
     * @param registrationContext  호출 클라이언트의 영구 상태에 대한 액세스를 제공
     */
    void setContext(String name, SecurityCheckConfiguration config, AuthorizationContext authorizationContext, RegistrationContext registrationContext);

    /**
     * 보안 체크 비활성 시간 제한, 일반적으로 구성된 값을 가져온다.
     * 0은 이 검사에 대해 정의된 비활성 시간 초과 없음을 의미한다.
     *
     * @return 비활성 시간 초과(초), 0(없는 경우)
     */
    int getInactivityTimeoutSec();

    /**
     * 보안 검사의 현재 상태의 만료를 가져온다.
     * 상태(success, failure, any other)에 대해 어떠한 가정도 하지 않는다.
     * 만기가 지나면 보안 검사 상태가 손실된다.
     *
     * @return 만료시간(밀리초)
     */
    long getExpiresAt();

    /**
     * 이 보안 검사에서 주어진 범위를 요청한다.
     * 체크 성공, 실패, 시도를 반환 할 수 있다.
     *
     * @param scope       요청 된 범위는, 범위 매핑 처리로부터 온다.
     *                    어떤 지점에서 어떤 범위를 부여해야하는지 정확히 알고있는 사용자 정의 검사의 경우를 제외하고는 일반적으로 보안 검사로 분석해서는 안된다.
     * @param credentials 클라이언트가 보낸 자격 증명 - 챌린지 응답 또는 선제적
     * @param request     클라이언트가 보낸 사전 승인 또는 등록 요청
     * @param response    이 상태가 성공, 실패, 시도를 추가하는 응답
     */
    void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response);

    /**
     * 현재 이 상태가 요청 된 범위를 부여하는지 확인한다.
     * 범위가 부여되면 구현은 부여 된 범위, 만료 및 사용자 정의 인트로스펙션 데이터를 응답 매개 변수에 추가해야한다.
     * 범위가 부여되지 않으면 구현이 자동으로 반환된다.
     *
     * @param scope    이 상태에 의해 부여 될 것으로 예상되는 범위
     * @param response 이 검사가 부여한 범위와 사용자 정의 인트로스펙션 데이터를 추가하는 응답
     */
    void introspect(Set<String> scope, IntrospectionResponse response);

    /**
     * 이 보안 검사에서 명시적으로 로그아웃 할 때 호출된다.
     * 구현시 영구(등록 된)상태 또는 다른 사용자 정의 논리를 수정하거나 삭제하도록 선택할 수 있다.
     * 이 보안 검사의 일시적인 상태는 자동으로 삭제된다.
     */
    void logout();
}

```

#### 원본

```java
/**
 * Represents server-side state of a security check.<br/>
 * Security checks are kept in a distributed cache, and are stateful, so it's the responsibility of the implementor
 * to handle the persistent state.
 *
 * @author artem
 *         Date: 6/28/15
 */

public interface SecurityCheck extends Externalizable {

    /**
     * Create configuration object and populate fields from the given properties.
     * Use errors and warnings maps to report problems.<br/>
     * Called during deployment.
     *
     * @param properties the merged properties to read and validate
     * @return new configuration object, not null
     */
    SecurityCheckConfiguration createConfiguration(Properties properties);

    /**
     * Initialize the security check with the context and configuration properties.
     * This method is called after instantiation and also on each retrieve.
     * The security checks should not keep the configuration data in the persistent state
     *
     * @param name                 name of the security check
     * @param config               security check configuration created by {@link #createConfiguration(Properties)}
     * @param authorizationContext provides access to the transient state of the calling client
     * @param registrationContext  provides access to the persistent state of the calling client
     */
    void setContext(String name, SecurityCheckConfiguration config, AuthorizationContext authorizationContext, RegistrationContext registrationContext);

    /**
     * Get the security check inactivity timeout, usually the configured value.
     * 0 means no inactivity timeout defined for this check
     *
     * @return inactivity timeout in seconds, 0 if none
     */
    int getInactivityTimeoutSec();

    /**
     * Get the expiration of the current state of the security check.
     * No assumption is made about the meaning of the state (success, failure, or any other).
     * After the expiration the state of the security check is lost.
     *
     * @return the time of expiration in millis
     */
    long getExpiresAt();

    /**
     * Request the given scope from this security check.
     * The check can return success, challenge, or failure
     *
     * @param scope       the requested scope, comes from the scope mapping processing.
     *                    Usually should not be analyzed by the security check, except of the case of a custom check
     *                    that knows exactly what scopes should be granted at any point.
     * @param credentials the credentials sent by the client - either as a challenge response or pre-emptively
     * @param request     the pre-authorization or registration request sent by the client
     * @param response    the response to which this check adds its success, challenge, or failure
     */
    void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response);

    /**
     * Make sure this check currently grants the requested scope.<br/>
     * If the scope is granted, the implementation should add the granted scope, its expiration, and custom introspection data to the response parameter.
     * If the scope is not granted, the implementation should return silently.
     *
     * @param scope    scope expected to be granted by this check
     * @param response the response to which this check adds its granted scope and custom introspection data
     */
    void introspect(Set<String> scope, IntrospectionResponse response);

    /**
     * Called upon explicit logout from this security check.
     * The implementation may choose to modify or delete its persistent (registered) state, or other custom logic.<br/>
     * The transient state of this security check is destroyed automatically.
     */
    void logout();
}
```