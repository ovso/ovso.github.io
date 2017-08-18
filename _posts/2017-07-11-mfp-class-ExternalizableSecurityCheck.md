---
layout: post
title: "ExternalizableSecurityCheck 클래스"
description: 
categories: [MFP]
tags: [MFP]
redirect_from:
  - /2017/08/18/
---



#### ExternalizableSecurityCheck 클래스

[CredentialsValidationSecurityCheck](https://ovso.github.io/mfp/mfp-class-CredentialsValidationSecurityCheck/){:target="_blank"} 클래스의 부모 클래스이다.

```java
/**
 * 보안 검사 구현을위한 편리한 기본 클래스. 다음 기능을 제공합니다.
 * JSON 객체로 자동 외부화. 외부화 가능 필드는 일시적으로 표시해야합니다.
 * 구성 속성 inactivityTimeoutSec
 * initStateDurations (Map), setState (String), 및 getState ()를 포함한 상태 관리기구를 개입시켜getExpiresAt ()를 구현합니다.
 * logout () 메소드의 비어있는 구현
 * 저자 artem
 * 날짜 : 8/17/15
 */

public abstract class ExternalizableSecurityCheck implements SecurityCheck {

    private static final Logger logger = Logger.getLogger(ExternalizableSecurityCheck.class.getName());

    /**
     * 미리 정의 된 상태 이름은 보안 검사가 만료되어 상태가 유지되지 않는다는 의미입니다.
     */
    protected static final String STATE_EXPIRED = "expired";

    protected transient ExternalizableSecurityCheckConfig config;
    protected transient AuthorizationContext authorizationContext;
    protected transient RegistrationContext registrationContext;

    private transient Map<String, Integer> stateDurations;

    // 런타임 상태
    private String checkName;
    private int inactivityTimeoutSec;
    private String stateName = STATE_EXPIRED;
    private long stateSetAt;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new ExternalizableSecurityCheckConfig(properties);
    }

    /**
	 * initStateDurations (Map)에 의해 리턴 된 상태 기간에 근거 해, setState (String)를 개입시켜 현재의 상태 세트의 만료를 계산합니다.
	 * 현재 상태가 STATE_EXPIRED이면이 메서드는 0을 반환합니다.
	 * @return 현재 상태가 만료되는 시간을 반환하거나 현재 상태가 null 인 경우 0을 반환합니다.
     */
    @Override
    public long getExpiresAt() {
        if (stateName.equals(STATE_EXPIRED)) return 0;

        Integer duration = stateDurations.get(stateName);
        if (duration == null) {
            logger.severe("Unsupported state name '" + stateName + "' for security check '" + checkName + "'. The state is dropped.");
            stateName = STATE_EXPIRED;
            return 0;
        }

        return stateSetAt + duration * 1000;
    }

    @Override
    public int getInactivityTimeoutSec() {
        return inactivityTimeoutSec;
    }

    @Override
    public void setContext(String name, SecurityCheckConfiguration config, AuthorizationContext authorizationContext, RegistrationContext registrationContext) {
      	//이름 보안 확인을위한 컨텍스트 설정
        logger.log(Level.FINEST, "Setting context for '" + name + "' security check.");
        this.checkName = name;
        this.authorizationContext = authorizationContext;
        this.registrationContext = registrationContext;
        this.config = (ExternalizableSecurityCheckConfig) config;
        inactivityTimeoutSec = getConfiguration().inactivityTimeoutSec;

        stateDurations = new HashMap<>();
        initStateDurations(stateDurations);
        if (stateDurations.containsKey(STATE_EXPIRED))
            throw new RuntimeException("The state '" + STATE_EXPIRED + "' is predefined and should not be added to the durations map.");
      //STATE_EXPIRED 상태는 미리 정의되어 있으므로 기간 맵에 추가하면 안됩니다.
    }

    @Override
    public void logout() {
        // 아무것도하지 않으며, 서브 클래스가 오버라이드하여 커스텀 로직을 구현할 수있다.
    }

    public void writeExternal(ObjectOutput out) throws IOException {
        String jsonStr = JSONUtils.FIELDS_OBJECT_MAPPER.writeValueAsString(this);
        out.writeObject(jsonStr);
    }

    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        String jsonStr = (String) in.readObject();
        JSONUtils.FIELDS_OBJECT_MAPPER.readerForUpdating(this).readValue(jsonStr);
    }

    protected ExternalizableSecurityCheckConfig getConfiguration() {
        return config;
    }

    protected String getName() {
        return checkName;
    }

    /**
     * 주어진 상태를 설정하고 시간을 표시한다.
     * 상태기 이미 지정된 값으로 설정된 경우 변경이 수행되지 않는다.
     *
     * @param state의 이름을 지정하고, initStateDurations(Map)에 의해 정의된 이름 중 하나이거나 STATE_EXPIRED 여야 한다.
     * @see getExpiresAt()
     */
    protected void setState(String name) {
        if (!(name.equals(STATE_EXPIRED)) && !stateDurations.containsKey(name)) {
            throw new RuntimeException("Unsupported state '" + name + "' for security check '" + checkName + "'.");
        }

        //상태가 만료되면 상태 및 설정 시간을 업데이트 한다.
        if (!stateName.equals(name) || System.currentTimeMillis() >= getExpiresAt()) {
            stateName = name;
            stateSetAt = System.currentTimeMillis();
        }
    }

    /**
     * setState (String) 메소드를 개입시켜 현재 상태를 취득한다.
     *
     * @return 현재 상태 이름
     */
    public String getState() {
        // 상태가 만료 된 경우(구성 변경으로 인해) 상태이름에 만료를 할당한다.
        if (!stateName.equals(STATE_EXPIRED) && System.currentTimeMillis() >= getExpiresAt()) {
            stateName = STATE_EXPIRED;
        }

        return stateName;
    }

    /**
	 * 이 보안 점검에서 지원하는 상태 이름과 지속 시간을 초 단위로 입력 맵에 입력한다.
	 * 구현은 getConfiguration() 메서드를 사용하여 구성된 값에 액세스 할 수 있다.
	 * STATE_EXPIRED 상태는 미리 정의되어 있으므로 맵에 넣으면 안된다.
	 * 
	 * @param 기간은 상태 이름을 키로 사용하고 초 단위의 지속 시간을 값으로 사용한다.
     */
    protected abstract void initStateDurations(Map<String, Integer> durations);

}
```

#### 원본

```java
/**
 * Convenience base class for security check implementations. Provides the following features:
 * <ul>
 * <li/> Automatic externalization as JSON object. Not externalizable fields must be marked transient.
 * <li/> Configuration property <code>inactivityTimeoutSec</code>
 * <li/> Implementation of {@link #getExpiresAt()} via state management mechanism that includes
 *      {@link #initStateDurations(Map)}, {@link #setState(String)}, and {@link #getState()}
 * <li/> Empty implementation of the {@link #logout()} method
 * </ul>
 *
 * @author artem
 *         Date: 8/17/15
 */

public abstract class ExternalizableSecurityCheck implements SecurityCheck {

    private static final Logger logger = Logger.getLogger(ExternalizableSecurityCheck.class.getName());

    /**
     * Predefined state name that means the security check is expired and its state will not be preserved.
     */
    protected static final String STATE_EXPIRED = "expired";

    protected transient ExternalizableSecurityCheckConfig config;
    protected transient AuthorizationContext authorizationContext;
    protected transient RegistrationContext registrationContext;

    private transient Map<String, Integer> stateDurations;

    // runtime state
    private String checkName;
    private int inactivityTimeoutSec;
    private String stateName = STATE_EXPIRED;
    private long stateSetAt;

    @Override
    public SecurityCheckConfiguration createConfiguration(Properties properties) {
        return new ExternalizableSecurityCheckConfig(properties);
    }

    /**
     * Calculates the expiration for the current state set via {@link #setState(String)} based on state durations
     * returned by {@link #initStateDurations(Map)}<br/>
     * If the current state is {@link #STATE_EXPIRED}, this method returns 0
     *
     * @return the time when the current state will be expired, or 0 if the current state is null
     */
    @Override
    public long getExpiresAt() {
        if (stateName.equals(STATE_EXPIRED)) return 0;

        Integer duration = stateDurations.get(stateName);
        if (duration == null) {
            logger.severe("Unsupported state name '" + stateName + "' for security check '" + checkName + "'. The state is dropped.");
            stateName = STATE_EXPIRED;
            return 0;
        }

        return stateSetAt + duration * 1000;
    }

    @Override
    public int getInactivityTimeoutSec() {
        return inactivityTimeoutSec;
    }

    @Override
    public void setContext(String name, SecurityCheckConfiguration config, AuthorizationContext authorizationContext, RegistrationContext registrationContext) {
        logger.log(Level.FINEST, "Setting context for '" + name + "' security check.");
        this.checkName = name;
        this.authorizationContext = authorizationContext;
        this.registrationContext = registrationContext;
        this.config = (ExternalizableSecurityCheckConfig) config;
        inactivityTimeoutSec = getConfiguration().inactivityTimeoutSec;

        stateDurations = new HashMap<>();
        initStateDurations(stateDurations);
        if (stateDurations.containsKey(STATE_EXPIRED))
            throw new RuntimeException("The state '" + STATE_EXPIRED + "' is predefined and should not be added to the durations map.");
    }

    @Override
    public void logout() {
        // do nothing, subclasses may override to implement custom logic
    }

    public void writeExternal(ObjectOutput out) throws IOException {
        String jsonStr = JSONUtils.FIELDS_OBJECT_MAPPER.writeValueAsString(this);
        out.writeObject(jsonStr);
    }

    public void readExternal(ObjectInput in) throws IOException, ClassNotFoundException {
        String jsonStr = (String) in.readObject();
        JSONUtils.FIELDS_OBJECT_MAPPER.readerForUpdating(this).readValue(jsonStr);
    }

    protected ExternalizableSecurityCheckConfig getConfiguration() {
        return config;
    }

    protected String getName() {
        return checkName;
    }

    /**
     * Set the given state and mark the time.<br/>
     * If the state is already set to the given value, no change is done.
     *
     * @param name the name of the state, must be one of those defined by {@link #initStateDurations(Map)}, or {@link #STATE_EXPIRED}
     * @see #getExpiresAt()
     */
    protected void setState(String name) {
        if (!(name.equals(STATE_EXPIRED)) && !stateDurations.containsKey(name)) {
            throw new RuntimeException("Unsupported state '" + name + "' for security check '" + checkName + "'.");
        }

        //if state is expired update the state and the set time
        if (!stateName.equals(name) || System.currentTimeMillis() >= getExpiresAt()) {
            stateName = name;
            stateSetAt = System.currentTimeMillis();
        }
    }

    /**
     * Get the current state set via method {@link #setState(String)}
     *
     * @return the current state name
     */
    public String getState() {
        // if the state is expired (due to configuration change), drop it
        if (!stateName.equals(STATE_EXPIRED) && System.currentTimeMillis() >= getExpiresAt()) {
            stateName = STATE_EXPIRED;
        }

        return stateName;
    }

    /**
     * Put the state names supported by this security check and their durations in seconds into the input map.<br/>
     * Implementations may use {@link #getConfiguration()} method to access configured values.
     * The state {@link #STATE_EXPIRED} is predefined, and should not be put into the map.
     *
     * @param durations map with the state names as keys and their durations in seconds as values
     */
    protected abstract void initStateDurations(Map<String, Integer> durations);

}
```