---
layout: post
title: "틱톡 메인화면 만들기(중첩 뷰페이저)"
categories: [Android]
tags: [Android]
---

[세로 뷰페이저 어댑터 만들기](세로 뷰페이저 어댑터 만들기)

# 틱톡 메인 화면 분석

틱톡의 메인화면은 (전체화면)세로 뷰페이저로 구성되어 있다. 그리고 뷰페이저의 각 화면마다 우측으로 스와이프하여 공통된 하나의 화면을 보여준다.

# 세로 뷰페이저 만들기

세로 뷰페이저가 핵심이다.

```java
public class VerticalViewPager extends ViewPager {

  public static boolean enabled = true;

  public VerticalViewPager(Context context) {
    super(context);
    init();
  }

  public void init() {
    setPageTransformer(true, new VerticalViewPagerTransform());
    setOverScrollMode(OVER_SCROLL_NEVER);
  }

  public VerticalViewPager(Context context, AttributeSet attributeSet) {
    super(context, attributeSet);
    init();
  }

  private MotionEvent swapXY(MotionEvent event) {
    float x = getWidth();
    float y = getHeight();

    float newX = (event.getY() / y) * y;
    float newY = (event.getX() / x) * x;

    event.setLocation(newX, newY);
    return event;
  }

  @Override
  public boolean onInterceptTouchEvent(MotionEvent ev) {
    if (enabled) {
      boolean intercept = super.onInterceptTouchEvent(swapXY(ev));
      swapXY(ev);
      return intercept;
    } else {
      return false;
    }
  }

  @Override
  public boolean onTouchEvent(MotionEvent ev) {
    if (enabled) {
      return super.onTouchEvent(swapXY(ev));
    } else {
      return false;
    }
  }

  private class VerticalViewPagerTransform implements ViewPager.PageTransformer {

    private static final float Min_Scale = 0.65f;

    @Override
    public void transformPage(View page, float position) {

      if (position < -1) {
        page.setAlpha(0);
      } else if (position <= 0) {
        page.setAlpha(1);
        page.setTranslationX(page.getWidth() * -position);
        page.setTranslationY(page.getHeight() * position);
        page.setScaleX(1);
        page.setScaleY(1);
      } else if (position <= 1) {
        page.setAlpha(1 - position);
        page.setTranslationX(page.getWidth() * -position);
        page.setTranslationY(0);
        float scaleFactor = Min_Scale + (1 - Min_Scale) * (1 - Math.abs(position));
        page.setScaleX(scaleFactor);
        page.setScaleY(scaleFactor);
      } else if (position > 1) {
        page.setAlpha(0);
      }
    }
  }
}
```

## Enabled의 역할 및 설정

### 역할

세로 뷰페이저 화면에서 우측으로 스와이프하여 나오는 화며네서는 세로 뷰페이저를 사용할 없게 하는 역할을 한다.

### 설정

enabled의 값은 세로 뷰페이저 안의 가로 뷰페이저에 등록한 OnPageChangeListener를 통해 할당한다. position이 0이라면 enabled에 true를, 1이라면 enabled에 false를 넣어주면 된다. enabled는 정적 변수를 사용하여 참조하거나 RxBus를 사용하여 참조할 수 있다.

# 세로 뷰페이저 어댑터 만들기

세로 뷰페이저에 설정할 어댑터를 만든다. FragmentPagerAdapter를 구현하여 만들거나, PagerAdapter를 구현하여 만들 수 있다. FragmentPagerAdapter로 구현하면 사용하지 않는 Fragment를 가지고 있어야 하는 부담이 있다. PagerAdapter로 구현하면 ViewHolder 패턴을 사용하여 구현할 수 있다.

# 가로 뷰페이저 만들기

세로 뷰페이저(세로 뷰페이저 + 뷰페이저 어댑터)를 완성했다면 각 화면에 들어갈 가로 뷰페이저를 만들자

```java
public class HorizontalViewPager extends ViewPager {

  public HorizontalViewPager(Context context, AttributeSet attrs) {
    super(context, attrs);
  }

  public HorizontalViewPager(Context context) {
    super(context);
  }

  @Override
  public boolean canScrollVertically(int direction) {
    return false;
  }

  @Override
  public boolean canScrollHorizontally(int direction) {
    return false;
  }
}
```

세로 뷰페이저가 canScrollVertically, canScrollHorizontally 설정에 영향을 받지 않는다면 xml로 설정해도 될 것 같다.

# 가로 뷰페이저 어댑터 만들기

가로 뷰페이저 어댑터는 FragmentPagerAdapter를 구현한다. FragmentStatePagerAdapter를 구현해도 되지만 신경써야 할 부분이 FragmentPagerAdapter보다 많다. 틱톡과 같이 여러 화면을 구현해야 한다면 FragmentStatePagerAdapter 구현을 고려해야 한다. 화면을 효율적으로 관리하기 때문이다.

# 두 번째 화면에서 세로 뷰페이저 전환 막기

# 가로 뷰페이저에 OnPageChangeListener 등록

가로 뷰페이저에 onPageChangeListener를 등록한다. onPageSelected(int position) 에서 position이 0이라면 세로 뷰페이저의 enabled를 true로, 1이라면 enabled를 false로 설정한다. 그래야 두번째 화면에서 세로 뷰페이저의 화면 전환 기능이 동작하지 않는다.