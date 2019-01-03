---
layout: post
title: "DimensionsUtils"
description: Dimensions
categories: [Android]
tags: [Android]
---

# DimensionsUtils

```java
public final class DimensionUtils {
  private DimensionUtils() {
  }

  public static int dpToPx(float dp, Context context) {
    return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dp,
        context.getResources().getDisplayMetrics());
  }

  public static int spToPx(float sp, Context context) {
    return (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, sp,
        context.getResources().getDisplayMetrics());
  }

  public static int dpToSp(float dp, Context context) {
    return (int) (dpToPx(dp, context) / context.getResources().getDisplayMetrics().scaledDensity);
  }
}
```

유용하게 사용하자.