---
layout: post
title: "Fragment의 onActivityResult"
categories: [Android]
tags: [Android]
---

onActivityResult를 Fragment에서 받으려면 어떻게 해야 할까?

쉽다. Fragment에서 startActivityForResult 하면 된다.

# onActivityResult가 불리지 않는다면?

```
activity.startActivityForResult(...)
```

startActivityForResult를 액티비티를 참조해서 호출하지 않았는지 살펴보라.