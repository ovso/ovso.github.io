---
layout: post
title: "JsonArray를 객체로 변환하기"
categories: [Android]
tags: [Android]

---

# 오픈소스

오픈소스로, [Gson](https://github.com/google/gson)을 사용한다. 대부분의 개발자가 사용하고 있는 오픈소스다.

# 확장함수 선언

```kotlin
inline fun <reified T> Gson.fromJsonArray(jsonArray: String): List<T> {
    val listType: Type = object : TypeToken<List<T>>() {}.type
    return Gson().fromJson(jsonArray, listType)
}
```

`inline` 과 `reified` 는 런타임시에 타입에 접근할 수 있도록 해준다. 구체적으로, 컴파일러가 함수의 바이트코드를 함수가 사용되는 모든 곳에 복사하도록 만든다.  

# 사용하기

```kotlin
data class Card(val rank:String, val suit:String)

fun jsonArrayTest() {
  val jsonStr = "[\n" +
  "    {\n" +
  "      \"rank\": \"4\",\n" +
  "      \"suit\": \"CLUBS\"\n" +
  "    },\n" +
  "    {\n" +
  "      \"rank\": \"A\",\n" +
  "      \"suit\": \"HEARTS\"\n" +
  "    }\n" +
  "  ]"
  val cards = Gson().fromJsonArray<Card>(jsonStr)
  println(cards.size)
  println(cards[0].suit)
}

```

출력:

```
2
CLUBS
```

# 결론

오벌엔지니어링 하지만고, 효율엔지니어링하자