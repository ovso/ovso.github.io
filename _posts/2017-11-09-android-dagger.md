---
layout: post
title: "Dagger2"
description: 
categories: [android]
tags: [Dagger]
redirect_from:
  - /2018/05/01/
---

# Dagger

Dagger가 무엇인지는 구글링하면 다 나온다. 많은 개발자들이 쓰고 있고, 구글샘플에도 필수적으로 사용되고 있다. 나도 얼마 전부터 이를 사용하고 있다. 사용하면 작성 클래스는 늘지만 좀 있어(?) 보인다. 그리고 뭔가 편하다는 것을 조금 느낀다. 각종 문서를 훑어봐도 원리를 제대로 이해하기는 힘들다. 여유를 갖자.(유연성을 갖자)



# @Inject

구글링 하면 @Inject가 뭔지 알 수 있다. class상에서 @Inject의 순서는 중요하다. 초기화(객체 생성)는 선언한 순서대로다.

```java
@Inject Adapter adapter;
@Inject Presenter presenter;
.
.
```

# @Inject(2)

클래스 생성자에 @Inject 해주면, Dagger 모듈에서 자동으로 생성해준다.

```java
public class SchedulersFacade {

  @Inject
  public SchedulersFacade() {}
    
}
```

아래와 같이 모듈을 생성할때, SchedulersFacade 매개변수를 선언해 준 것 만으로 객체가 생성되어 넘어온다.

```java
@Module public class SearchActivityModule {
    
  @Singleton @Provides
  public SearchViewPresenter provideSearchViewPresenter(SearchViewPresenter.View view,
      SchedulersFacade schedulers) {
    return new SearchViewPresenterImpl(view, schedulers);
  }
    
}
```



# @Singleton

@Singleton은 Dagger에서 제공하는 Annotation이 아니다.

**@Singleton을 사용하지 않았을 때**, Dagger가 주입한 객체를 재사용하는 것이 조금 부담이었다. 가령, @Inject Adapter adapter 이후 Module에서 재사용 할 때, View에서 주입한 Adapter를 다시 가져오는 방식이었다.

```Java
@Inject @Getter CustomAdapter adapter;
@Inject Presenter presenter;
```

그러나 Module에서 @Singleton을 사용하면 그럴 필요가 없다. Singleton이기 때문에..

**주의할 점**이 있다. Module에서 사용하기 전, Component에서 반드시 선언해 주어야 한다. 안해주면 에러를 뿜는다.

```java
// Component, Module, Provider 클리스 구조일 경우
@Singleton
@Subcomponent(modules = { EmpListFragmentModule.class }) public interface EmpListFragmentComponent
    extends AndroidInjector<EmpListFragment>
```

# @ContributesAndroidInjector

Dagger에서 이것을 사용하면, Component, Provider등의 클래스를 없앨 수 있다. 자동으로 만들어지는 것 같다.

대신, @Provides를 사용하여 메서드를 선언하고 구현하려면 반드시 **static**을 사용해야 한다.

```Java
@Module public abstract class TeamReportActivityModule {

  @ContributesAndroidInjector(modules = TeamReportDialogModule.class)
  abstract TeamReportDialog bindTeamReportDialog();

  @Provides static TeamReportPresenter.View provideTeamReportView(TeamReportActivity TeamReportActivity) {
    return TeamReportActivity;
  }
....
```



# A is bound multiple times

Dagger에서 A 객체를 너무 많이 사용했다는 의미다. A를 B(또는 'B extends A')다른 클래스 또는 인터페이스로 바꾸어 사용하면 문제 없이 동작한다. 좀 더 명확하게 사용하라는 메시지 인것 같다.