---
layout: post
title: "부모 메서드 호출 유도(@CallSuper)"
description: AndroidAnnotation
categories: [Android]
tags: [Android]
---

# @CallSuper

슈퍼 클래스의 메서드에서 [@CallSuper](https://developer.android.com/reference/android/support/annotation/CallSuper) 어노테이션을 사용한다.

```java
public class BaseViewHolder<T> extends RecyclerView.ViewHolder {

  public BaseViewHolder(View itemView) {
    super(itemView);
    ButterKnife.bind(this, itemView);
  }

  protected T data;

  @CallSuper // Here!!
  public void bind(T $data) {
    data = $data;
  }
}
```

자식 클래스에서, 부모 클래스에서 어노테이팅 한 메서드를 호출하지 않으면 컴파일 에러와 같은 표시를 볼 수 있다. 즉 bind 메서드에 빨간 밑줄이 나타난다. 그러나 빌드하는데는 문제없다.

```java
public class MainViewHolder extends BaseViewHolder<JsonElement> {
  ...
  ...
  private MainViewHolder(View itemView) {
    super(itemView);
  }

  @Override public void bind(JsonElement $data) {
    super.bind($data);
    ...
    ...
  }
}
```