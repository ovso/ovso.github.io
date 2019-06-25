---
layout: post
title: "Dagger2 에러(ButterKnife편)"
categories: [Android]
tags: [Android]
---

# 에러

```
error: cannot find symbol class DaggerAppComponent
```

```
error: [ComponentProcessor:MiscError] dagger.internal.codegen.ComponentProcessor was unable to process this interface because not all of its dependencies could be resolved. Check for compilation errors or a circular dependency with generated code.
```

도무지 알 수 없는 Dagger2 에러..

# 원인

```java
  // Inner
	private static class BridgeRevViewHolder extends BaseViewHolder<JsonElement> {

    @BindView(R.id.imageview_bridge_all_thumb) ImageView thumbImageView;
    @BindView(R.id.textview_bridge_all_title) TextView titleTextView;
    @BindView(R.id.textview_bridge_all_city) TextView cityTextView;
		...
    ..
    .
  }
    
```

# 해결

```java
	static class BridgeRevViewHolder extends BaseViewHolder<JsonElement> {

    @BindView(R.id.imageview_bridge_all_thumb) ImageView thumbImageView;
    @BindView(R.id.textview_bridge_all_title) TextView titleTextView;
    @BindView(R.id.textview_bridge_all_city) TextView cityTextView;
		...
    ..
    .
  }
```

# 결론

액티비티에서 Dagger를 사용한다. 프래그먼트에서는 대거를 사용하지 않는다. 오류는 