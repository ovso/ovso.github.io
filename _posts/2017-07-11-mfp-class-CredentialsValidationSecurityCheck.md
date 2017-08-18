---
layout: post
title: "CredentialsValidationSecurityCheck 클래스"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---



#### CredentialsValidationSecurityCheck 클래스

MFP Adapter	에서 사용하는 클래스로, [UserAuthenticationSecurityCheck](https://ovso.github.io/mfp/mfp-class-UserAuthenticationSecurityCheck/){:target="_blank"} 클래스의 부모 클래스다.

```java
/**
 * 자격 증명 유효성 검사를 구현하는 보안 검사를위한 편리한 기본 클래스이다.
 * 이 클래스는 특정 간격 동안 제한된 수의 로그인 시도를 허용하는 공통 논리를 구현한다.
 * 시도가 초과되면 구성된 간격 동안 검사가 차단된다.
 * 로그인이 성공하면 다른 구성된 간격 동안 결과가 유효하다.
 * 구성 등록 정보 및 기본값은 CredentialsValidationSecurityCheckConfig 클래스에 의해 정의된다.
 * 작성자 : artem
 * 날짜 : 8/24/15
 */
public abstract class CredentialsValidationSecurityCheck extends ExternalizableSecurityCheck {

    /**
	 * 챌린지가 보내졌고 보안 검사가 클라이언트 응답을 기다리고 있다.
	 * 시도 횟수는이 상태 동안 유지된다.
	 */
    protected static final String STATE_ATTEMPTING = "attempting";

    /**
     * 자격 증명이 성공적으로 확인
     */
    protected static final String STATE_SUCCESS = "success";

    /**
     * 최대 시도 횟수를 초과했으며 검사가 잠긴 상태
     */
    protected static final String STATE_BLOCKED = "blocked";

    private int usedAttempts;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new CredentialsValidationSecurityCheckConfig(properties);
    }

    @Override
    public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
        // 새 자격 증명이 있고 상태가 차단되지 않은 경우 - 자격 증명의 유효성을 검사하고 상태를 업데이트한다.
        String state = getState();
        if (credentials != null && !state.equals(STATE_BLOCKED)) {
            if (validateCredentials(credentials)) {
                setState(STATE_SUCCESS);
            } else {
                // 우리가 상태를 시도하고 있으며 시도 횟수를 늘리고 있는지 확인
                if (!state.equals(STATE_ATTEMPTING)) setState(STATE_ATTEMPTING);
                usedAttempts++;

                // 시도가 초과되면 검사를 차단
                if (getRemainingAttempts() == 0) setState(STATE_BLOCKED);
            }
            state = getState();
        }

        // 상태에 따라 응답을 반환
        switch (state) {
            case STATE_SUCCESS:
                response.addSuccess(scope, getExpiresAt(), getName());
                break;
            case STATE_BLOCKED:
                Map<String, Object> failure = new HashMap<>();
                failure.put("failure", "Account blocked");
                response.addFailure(getName(), failure);
                break;
            default:
            	// 시도하고 있는지 상태를 확인하여 상태를 보존하고 보낸다.
                if (!state.equals(STATE_ATTEMPTING)) setState(STATE_ATTEMPTING);
                Map<String, Object> challenge = createChallenge();
                response.addChallenge(getName(), challenge != null ? challenge : new HashMap<String, Object>());
                break;
        }
    }

    @Override
    public void introspect(Set<String> scope, IntrospectionResponse response) {
        if (getState().equals(STATE_SUCCESS))
            response.addIntrospectionData(getName(), scope, getExpiresAt(), getCustomIntrospectionData());
    }

    @Override
    protected CredentialsValidationSecurityCheckConfig getConfiguration() {
        return (CredentialsValidationSecurityCheckConfig) super.getConfiguration();
    }

    /**
     * 사용자 정의 introspection(내성, 자기성찰) 데이터를 가져온다.
     * 기본 구현은 null을 반환한다.
     *
     * @return 이 검사로 제공되는 사용자 정의 introspection 데이터를 반환한다. 보안 검사가 사용자 정의 데이터		* 를 제공하지 않는다면 null이다.
     */
    protected Map<String, String> getCustomIntrospectionData() {
        return null;
    }

    public int getRemainingAttempts() {
        return Math.max(0, getConfiguration().maxAttempts - usedAttempts);
    }

    protected void setState(String state) {
        super.setState(state);
        usedAttempts = 0;
    }

    public String getState() {
        String state = super.getState();
        // 시도중인 상태이고 구성에서 시도가 남아 있지 않으면 차단 된 상태로 이동한다.
        if (state.equals(STATE_ATTEMPTING) && getRemainingAttempts() == 0) {
            state = STATE_BLOCKED;
            setState(state);
        }

        return state;
    }

    @Override
    protected void initStateDurations(Map<String, Integer> durations) {
        CredentialsValidationSecurityCheckConfig config = getConfiguration();
        durations.put(STATE_ATTEMPTING, config.attemptingStateExpirationSec);
        durations.put(STATE_SUCCESS, config.successStateExpirationSec);
        durations.put(STATE_BLOCKED, config.blockedStateExpirationSec);
    }

    /**
     * 자격 증명 유효성 검사
     * 이 메서드는 자격 증명이 클라이언트에 의해 전송 된 경우에만 호출된다. 예를 들어 선제 승인 또는 챌린지 	 
     * 답변 제출
     *
     * @param 자격 증명 정보 객체, null이 아님
     * @return 자격이 유효한 경우는 true, 그렇지 않은 경우는 false를 반환
     */
    protected abstract boolean validateCredentials(Map<String, Object> credentials);

    /**
     * 클라이언트에게 보내야 하는 챌린지를 만든다.
     *
     * @return 챌린지 객체 반환
     */
    protected abstract Map<String, Object> createChallenge();
}
```

#### 원본

```java
/**
 * Convenience base class for security checks implementing credentials validation.<br/>
 * This class implements a common logic that allows a limited number of login attempts during a certain interval.
 * If the attempts are exceeded the check is blocked for a configured interval.
 * If the login succeeds, the result is valid for another configured interval.<br/>
 * Configuration properties and their defaults are defined by the class {@link CredentialsValidationSecurityCheckConfig}
 *
 * @author artem
 *         Date: 8/24/15
 */
public abstract class CredentialsValidationSecurityCheck extends ExternalizableSecurityCheck {

    /**
     * A challenge has been sent and the security check awaits to the client response.
     * The attempt count is maintained during this state.
     */
    protected static final String STATE_ATTEMPTING = "attempting";

    /**
     * The credentials have been successfully validated
     */
    protected static final String STATE_SUCCESS = "success";

    /**
     * The max number of attempts exceeded and the check is in locked state
     */
    protected static final String STATE_BLOCKED = "blocked";

    private int usedAttempts;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new CredentialsValidationSecurityCheckConfig(properties);
    }

    @Override
    public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
        // if got new credentials and the state is not blocked - validate the credentials and update the state
        String state = getState();
        if (credentials != null && !state.equals(STATE_BLOCKED)) {
            if (validateCredentials(credentials)) {
                setState(STATE_SUCCESS);
            } else {
                // make sure we are in attempting state, and increase attempts count
                if (!state.equals(STATE_ATTEMPTING)) setState(STATE_ATTEMPTING);
                usedAttempts++;

                // if the attempts were exceeded block the check
                if (getRemainingAttempts() == 0) setState(STATE_BLOCKED);
            }
            state = getState();
        }

        // return response according to the state
        switch (state) {
            case STATE_SUCCESS:
                response.addSuccess(scope, getExpiresAt(), getName());
                break;
            case STATE_BLOCKED:
                Map<String, Object> failure = new HashMap<>();
                failure.put("failure", "Account blocked");
                response.addFailure(getName(), failure);
                break;
            default:
                // make sure we are in attempting state, so that the state would be preserved, and send a challenge
                if (!state.equals(STATE_ATTEMPTING)) setState(STATE_ATTEMPTING);
                Map<String, Object> challenge = createChallenge();
                response.addChallenge(getName(), challenge != null ? challenge : new HashMap<String, Object>());
                break;
        }
    }

    @Override
    public void introspect(Set<String> scope, IntrospectionResponse response) {
        if (getState().equals(STATE_SUCCESS))
            response.addIntrospectionData(getName(), scope, getExpiresAt(), getCustomIntrospectionData());
    }

    @Override
    protected CredentialsValidationSecurityCheckConfig getConfiguration() {
        return (CredentialsValidationSecurityCheckConfig) super.getConfiguration();
    }

    /**
     * Get custom introspection data.<br/>
     * Default implementation returns null. Subclasses may override this method to provide their own data.
     *
     * @return the custom introspection data provided by this security check, null if the security check provides no custom data
     */
    protected Map<String, String> getCustomIntrospectionData() {
        return null;
    }

    public int getRemainingAttempts() {
        return Math.max(0, getConfiguration().maxAttempts - usedAttempts);
    }

    protected void setState(String state) {
        super.setState(state);
        usedAttempts = 0;
    }

    public String getState() {
        String state = super.getState();
        // if the attempting state has no remaining attempts (due to configuration change), move to blocked state
        if (state.equals(STATE_ATTEMPTING) && getRemainingAttempts() == 0) {
            state = STATE_BLOCKED;
            setState(state);
        }

        return state;
    }

    @Override
    protected void initStateDurations(Map<String, Integer> durations) {
        CredentialsValidationSecurityCheckConfig config = getConfiguration();
        durations.put(STATE_ATTEMPTING, config.attemptingStateExpirationSec);
        durations.put(STATE_SUCCESS, config.successStateExpirationSec);
        durations.put(STATE_BLOCKED, config.blockedStateExpirationSec);
    }

    /**
     * Validate the credentials
     * This method is called only if credentials were sent by the client, i.e preemptive authorization
     * or submit challenge answer
     *
     * @param credentials credentials object, not null
     * @return true if the credentials are valid, false otherwise
     */
    protected abstract boolean validateCredentials(Map<String, Object> credentials);

    /**
     * Create the challenge that should be sent to the client
     *
     * @return the challenge object
     */
    protected abstract Map<String, Object> createChallenge();
}
```