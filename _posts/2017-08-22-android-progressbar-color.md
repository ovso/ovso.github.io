---
layout: post
title: "ProgressBar color"
description: 
categories: [Android]
tags: [Android]
redirect_from:
  - /2017/08/18/
---

#### ProgressBar Color 변경

```java
private ProgressBar progressbar;
...
progressbar.getIndeterminateDrawable().setColorFilter(Color.RED, Mode.MULTIPLY)
```

Nice!
