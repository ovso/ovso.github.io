---
layout: post
title: "Orientation Aware RecyclerView(스크롤 최적화 리싸이클러뷰)"
categories: [Android]
tags: [Android]
---

# Orientation Aware RecyclerView

오픈소스는 항상 나를 살린다. [OrientationAwareRecyclerView](https://github.com/ovso/RecyclerViewSnap/blob/master/gravitysnaphelper/src/main/java/com/github/rubensousa/gravitysnaphelper/OrientationAwareRecyclerView.java)도 나를 살렸다.

리싸이클러뷰를 중첩(리싸이클러뷰 in 뷰홀더)해서 사용할때 가로 스크롤링이 잘 안된다. HorizontalScrollView를 사용해도 마찬가지다. OrientationAwareRecyclerView를 사용해 보자. 잘 된다.

```java
/**
 * A RecyclerView that only handles scroll events with the same orientation of its LayoutManager.
 * Avoids situations where nested recyclerviews don't receive touch events properly:
 */
public class OrientationAwareRecyclerView extends RecyclerView {

    private float lastX = 0.0f;
    private float lastY = 0.0f;
    private int orientation = RecyclerView.VERTICAL;
    private boolean scrolling = false;

    public OrientationAwareRecyclerView(@NonNull Context context) {
        this(context, null);
    }

    public OrientationAwareRecyclerView(@NonNull Context context, @Nullable AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public OrientationAwareRecyclerView(@NonNull Context context, @Nullable AttributeSet attrs,
                                        int defStyle) {
        super(context, attrs, defStyle);
        addOnScrollListener(new OnScrollListener() {
            @Override
            public void onScrollStateChanged(@NonNull RecyclerView recyclerView, int newState) {
                super.onScrollStateChanged(recyclerView, newState);
                scrolling = newState != RecyclerView.SCROLL_STATE_IDLE;
            }
        });
    }

    @Override
    public void setLayoutManager(@Nullable LayoutManager layout) {
        super.setLayoutManager(layout);
        if (layout instanceof LinearLayoutManager) {
            orientation = ((LinearLayoutManager) layout).getOrientation();
        }
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent e) {
        boolean allowScroll = true;

        switch (e.getActionMasked()) {
            case MotionEvent.ACTION_DOWN: {
                lastX = e.getX();
                lastY = e.getY();
                // If we were scrolling, stop now by faking a touch release
                if (scrolling) {
                    MotionEvent newEvent = MotionEvent.obtain(e);
                    newEvent.setAction(MotionEvent.ACTION_UP);
                    return super.onInterceptTouchEvent(newEvent);
                }
                break;
            }
            case MotionEvent.ACTION_MOVE: {
                // We're moving, so check if we're trying
                // to scroll vertically or horizontally so we don't intercept the wrong event.
                float currentX = e.getX();
                float currentY = e.getY();
                float dx = Math.abs(currentX - lastX);
                float dy = Math.abs(currentY - lastY);
                allowScroll = dy > dx ? orientation == RecyclerView.VERTICAL
                        : orientation == RecyclerView.HORIZONTAL;
                break;
            }
        }

        if (!allowScroll) {
            return false;
        }

        return super.onInterceptTouchEvent(e);
    }

}
```

