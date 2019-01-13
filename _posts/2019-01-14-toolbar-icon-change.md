---
layout: post
title: "툴바 뒤로가기 아이콘 변경(Toolbar)"
categories: [Android]
tags: [Android]
---

간단하다.

```java
public void setNavigationIcon (Drawable icon)

// or
    
public void setNavigationIcon (int resId)
```

디자이너가 넘겨준 아이콘의 사이즈가 크다면 아이콘을 Drawable 타임으로 변환 후 사이즈를 변경해서 반영해야 한다. 아래와 같은 유틸이 있다면 편하다. scale 계산 부분을 수정하면 픽셀이나 DP로 바꿀 수도 있겠다.

```java
public final class DrawableUtils {
  private DrawableUtils() {
  }

  public static Drawable resize(Resources res, Drawable drawable, int scale) {
    Bitmap bitmap = ((BitmapDrawable) drawable).getBitmap();
    Drawable newDrawable = 
        new BitmapDrawable(
        	res, 
        	Bitmap.createScaledBitmap(
                bitmap, 
                bitmap.getWidth() / scale,bitmap.getHeight() / scale, true));

    return newDrawable;
  }
}
```



# 참고

[setNavigationIcon](https://developer.android.com/reference/androidx/appcompat/widget/Toolbar.html#setNavigationIcon(android.graphics.drawable.Drawable))