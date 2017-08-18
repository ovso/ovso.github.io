---
layout: post
title: "UserAuthenticationSecurityCheck 클래스"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/

---



#### UserAuthenticationSecurityCheck 클래스

MFP Adapter	에서 사용하는 클래스다.

```java
/**
* CredentialsValidationSecurityCheck 클래스 위에 구현 된 사용자 인증을 구현하는 보안 검사를위한 편리한 기본 클래스입니다.
사용자 자격 증명이 유효하면이 클래스는 createUser() 메서드를 호출합니다.
* 이 사용자는 활성 사용자로 설정되며 등록 서비스에도 저장됩니다.
* 인증 된 사용자는 클라이언트의 챌린지 처리기에 "user"라는 필드 이름으로 반환 된 성공 데이터에 자동으로 추가됩니다.
* 이 클래스는 또한 등록 서비스에 저장된 사용자가 활성 사용자로 사용될 때 "remember me"기능을 구현합니다.
* 이 기능은 서버 측에서 rememberMeDurationSec 구성 속성에 의해 제어됩니다.
* 이 등록 정보가 0보다 크면 등록 된 사용자는 해당 간격 동안 활성 사용자로 사용됩니다.
* 이 간격이 만료 된 후 명시 적으로 로그 아웃 한 후 사용자는 다시 로그인하기 위해 자격 증명을 제공해야합니다.
* "remember me"가 보안 점검 구성에 의해 일반적으로 사용 가능할 때, 서브 클래스는 클라이언트 인스턴스마다 더 세밀한 구분을 할 수 있습니다.
* rememberCreatedUser () 메서드는 자격 증명 유효성 검사가 성공한 후에 호출되며이 특정 사용자를 기억해야할지 여부를 결정할 수 있습니다.
* 이 클래스에는 다음 구성 등록 정보가 필요합니다.
* rememberMeDurationSec - 등록 된 사용자가 활성 사용자로 사용되는 간격 (초).
0 값은 기능이 비활성화되었음을 의미합니다.
* 작성자 artem
* 작성일 11/10/15
*/
public abstract class UserAuthenticationSecurityCheck extends CredentialsValidationSecurityCheck {
	
    // 등록 속성 이름
    private static final String REMEMBERED_ATTTRIBUTE = "rememberedAt";

    private static final Logger logger = Logger.getLogger(UserAuthenticationSecurityCheck.class.getName());

    protected AuthenticatedUser user;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new UserAuthenticationSecurityCheckConfig(properties);
    }

    @Override
    public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
        // 초기 상태에서 클라이언트가 자격 증명을 제공하지 않으면 기억 된 사용자를 먼저 얻는다.
        if (getState().equals(STATE_EXPIRED) && credentials == null) {
            user = getRememberedMe();
            if (user != null)
                setState(STATE_SUCCESS);
        }
      
        // 기본 클래스는 과제 및 자격 증명 유효성 검사를 담당한다.
        super.authorize(scope, credentials, request, response);
		
        // 인증이 성공하면 사용자를 돌봅니다.
        if (response.getType() == AuthorizationResponse.ResponseType.SUCCESS) {
            // 새로 인증 된 사용자인 경우 - 생성하고, 등록하고, 기억한다.
            if (credentials != null) {
                user = activateCreateUser();
                registrationContext.setRegisteredUser(user);
                if (rememberCreatedUser())
                    rememberMe();
            }

          	// 기억되어 있는지 아니면 그냥 인증을 받았는지를 활성 사용자로 설정한다.
            authorizationContext.setActiveUser(user);
          	// 성공 응답에 사용자를 추가한다.
            response.addSuccess(scope, getExpiresAt(), getName(), "user", user);
        }
    }

    @Override
    public void logout() {
      	// 명시적 로그 아웃 시 기억 된 사용자 잊기
        forgetMe();
    }

    /**
     * 현재 기억된 시간을 등록 서비스에 저장한다.
     */
    protected void rememberMe() {
        if (getRememberMeDuration() > 0) {
            PersistentAttributes attr = registrationContext.getRegisteredPublicAttributes();
            attr.put(REMEMBERED_ATTTRIBUTE, System.currentTimeMillis());
        }
    }

    /**
     * 기억 된 시간 속성을 등록 서비스에서 제거한다.
     */
    protected void forgetMe() {
        if (getRememberMeDuration() > 0) {
            PersistentAttributes attr = registrationContext.getRegisteredPublicAttributes();
            attr.delete(REMEMBERED_ATTTRIBUTE);
        }
    }

    /**
     * 사용자가 너무 오래 전에 기억되지 않은 경우 - 등록 된 사용자를 반환한다.
     * 
     * @return 너무 오래 전 기억 된 경우 등록 된 사용자 또는 null을 반환한다.
     */
    protected AuthenticatedUser getRememberedMe() {
        if (getRememberMeDuration() > 0) {
            Long rememberedTime = registrationContext.getRegisteredPublicAttributes().get(REMEMBERED_ATTTRIBUTE);
            if (rememberedTime != null && rememberedTime + getRememberMeDuration() * 1000 >= System.currentTimeMillis()) {
                return registrationContext.getRegisteredUser();
            }
        }
        return null;
    }

    protected int getRememberMeDuration() {
        return ((UserAuthenticationSecurityCheckConfig)config).rememberMeDurationSec;
    }


    private AuthenticatedUser activateCreateUser() {
        AuthenticatedUser user = createUser();
        if(user == null) {
            logger.severe("createUser method must not return null");
            throw new RuntimeException("UserAuthenticationSecurityCheck must create a non-null user");
        }
        return user;
    }

    /**
     * 자격 증명 유효성 검사가 성공적으로 수행 된 후 즉시 호출되는 사용자 개체를 만든다.
     *
     * @return null이 아닌 생성된 사용자를 반환한다.
     */
    protected abstract AuthenticatedUser createUser();

    /**
     * 새로 생성 된 사용자를 기억해야 하는지 여부
     * 구현은 클라이언트 선택에 의존 할 수 있다. 예를 들어 클라이언트 자격 증명과 함께 "remember me"확인란의 값		을 보낼 수 있다.
     * 기본 구현은 true를 반환한다.
     * 
     * @return 사용자를 기억해야 하는 경우 true이고, 그렇지 않으면 false이다.
     */
    protected boolean rememberCreatedUser() {
        return true;
    }
}
```

#### 원본

```java
package com.ibm.mfp.security.checks.base;

import com.ibm.mfp.server.registration.external.model.AuthenticatedUser;
import com.ibm.mfp.server.registration.external.model.PersistentAttributes;
import com.ibm.mfp.server.security.external.checks.AuthorizationResponse;
import com.ibm.mfp.server.security.external.checks.SecurityCheckConfiguration;

import javax.servlet.http.HttpServletRequest;
import java.util.Map;
import java.util.Properties;
import java.util.Set;
import java.util.logging.Logger;

/**
 * Convenience base class for security checks implementing user authentication implemented on top of the class {@link CredentialsValidationSecurityCheck}.<br/>
 * When the user credentials are valid, this class invokes the method {@link #createUser()}.
 * This user is set as an active user, and also is stored in the Registration service.<br/>
 * The authenticated user is automatically added to the success data returned to the client's challenge handler under the field name "user".
 * <p/>
 * This class also implements the "remember me" functionality, when the user stored in the Registration service
 * can be used as an active user.<br/>
 * This functionality is controlled on the server side by <code>rememberMeDurationSec</code> configuration property.
 * If this property is greater than zero, the registered user is used as active user during that interval.<br/>
 * After expiration of that interval, as well as after explicit logout the user has to provide credentials to login again.<br/>
 * When "remember me" is generally enabled by the security check configuration, the subclasses may do a finer distinction per client instance.
 * The method {@link #rememberCreatedUser()} is called after a successful credentials validation, and allows to decide whether
 * this specific user should be remembered.
 * <p/>
 * This class requires the following configuration properties:
 * <ul>
 * <li/><b>rememberMeDurationSec</b> - interval in seconds during which the registered user is used as an active user.
 * 0 value means remember me feature is disabled
 * </ul>
 *
 * @author artem
 *         Date: 11/10/15.
 */
public abstract class UserAuthenticationSecurityCheck extends CredentialsValidationSecurityCheck {

    // registration attribute name
    private static final String REMEMBERED_ATTTRIBUTE = "rememberedAt";

    private static final Logger logger = Logger.getLogger(UserAuthenticationSecurityCheck.class.getName());

    protected AuthenticatedUser user;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new UserAuthenticationSecurityCheckConfig(properties);
    }

    @Override
    public void authorize(Set<String> scope, Map<String, Object> credentials, HttpServletRequest request, AuthorizationResponse response) {
        // in the initial state, if the client provided no credentials, get the remembered user first
        if (getState().equals(STATE_EXPIRED) && credentials == null) {
            user = getRememberedMe();
            if (user != null)
                setState(STATE_SUCCESS);
        }

        // base class takes care of challenges and credentials validation
        super.authorize(scope, credentials, request, response);

        // if the authentication was successful take care of the user
        if (response.getType() == AuthorizationResponse.ResponseType.SUCCESS) {
            // if it is a newly authenticated user - create it, register, and remember
            if (credentials != null) {
                user = activateCreateUser();
                registrationContext.setRegisteredUser(user);
                if (rememberCreatedUser())
                    rememberMe();
            }

            // set the active user whether it is remembered or just authenticated one
            authorizationContext.setActiveUser(user);
            // add the user to the success response
            response.addSuccess(scope, getExpiresAt(), getName(), "user", user);
        }
    }

    @Override
    public void logout() {
        // forget the remembered user on explicit logout
        forgetMe();
    }

    /**
     * Store the current (remembered at) time in the registration service.
     */
    protected void rememberMe() {
        if (getRememberMeDuration() > 0) {
            PersistentAttributes attr = registrationContext.getRegisteredPublicAttributes();
            attr.put(REMEMBERED_ATTTRIBUTE, System.currentTimeMillis());
        }
    }

    /**
     * Remove the remembered time attribute from registration service
     */
    protected void forgetMe() {
        if (getRememberMeDuration() > 0) {
            PersistentAttributes attr = registrationContext.getRegisteredPublicAttributes();
            attr.delete(REMEMBERED_ATTTRIBUTE);
        }
    }

    /**
     * if the user was remembered not too long ago - return the registered user
     *
     * @return the registered user or null if remembered too long ago
     */
    protected AuthenticatedUser getRememberedMe() {
        if (getRememberMeDuration() > 0) {
            Long rememberedTime = registrationContext.getRegisteredPublicAttributes().get(REMEMBERED_ATTTRIBUTE);
            if (rememberedTime != null && rememberedTime + getRememberMeDuration() * 1000 >= System.currentTimeMillis()) {
                return registrationContext.getRegisteredUser();
            }
        }
        return null;
    }

    protected int getRememberMeDuration() {
        return ((UserAuthenticationSecurityCheckConfig)config).rememberMeDurationSec;
    }


    private AuthenticatedUser activateCreateUser() {
        AuthenticatedUser user = createUser();
        if(user == null) {
            logger.severe("createUser method must not return null");
            throw new RuntimeException("UserAuthenticationSecurityCheck must create a non-null user");
        }
        return user;
    }

    /**
     * Create the user object, called immediately after successful credentials validation.
     *
     * @return the created user, not null
     */
    protected abstract AuthenticatedUser createUser();

    /**
     * Whether the newly created user should be remembered.
     * The implementation may rely on the client choice,
     * for example the client may send the value of "remember me" checkbox with the credentials.<br/>
     * The default implementation returns true.
     *
     * @return true if the user should be remembered, false otherwise.
     */
    protected boolean rememberCreatedUser() {
        return true;
    }
}
```