---
layout: post
title: "Scope Functions"
categories: [Kotlin]
tags: [Kotlin]
---

# Scope Functions

Kotlin 표준라이브러리에는 객체 컨텍스트 내에서 코드 블록을 실행하는 것이 유일한 목적인 여러 함수가 포함되어 있습니다. 람다식이 제공된 객체에서 이러한 함수를 호출하면 임시 범위가 형성됩니다. 이 범위에서 이름없이 개체에 접근 할 수 있습니다. 이러한 기능을 범위 기능(Scope Functions)라고 합니다. 여기에는 다섯가지 함수가 있습니다. let, run, with, apply, also

기본적으로, 다섯가지 함수들은 동일한 기능을 수행합니다. 객체에서 코드 블록을 실행합니다. 다른 점은이 객체가 블록 안에서 어떻게 사용 가능해지고 전체 표현식의 결과가 달라진다는 것입니다.



## 스코프 함수의 일반적인 사용법

```kotlin
fun main() {
    Person("Alice", 20, "Amsterdam").let {
        println(it)
        it.moveTo("London")
        it.incrementAge()
        println(it)
    }
}
```

## 스코프 함수 없이 작성

```kotlin
val alice = Person("Alice", 20, "Amsterdam")
println(alice)
alice.moveTo("London")
alice.incrementAge()
println(alice)
```

스코프 함수는 새로운 기술 기능을 도입하지 않지만 코드를보다 간결하고 읽기 쉽게 만들 수 있습니다.

# Scope Functions의 특징

스코프 기능은 본질적으로 매우 유사하기 때문에 그 기능의 차이점을 이해하는 것이 중요합니다. 각 스코프 기능에는 두 가지 주요 차이점이 있습니다.

- 컨텍스트 객체를 참조하는 방법
- 반환값

## 컨텍스트 객체 참조는 this 또는 it

범위 함수의 람다 내에서 컨텍스트 오브젝트는 실제 이름 대신 짧은 참조로 사용 가능합니다. 각 범위 함수는 컨텍스트 오브젝트에 액세스하는 두 가지 방법 중 하나를 사용합니다. : 람다 **receiver(this)** 또는 람다 **argument (it)**. 둘 다 동일한 기능을 제공하므로 서로 다른 경우에 대한 각각의 장단점을 설명하고 사용에 대한 권장 사항을 제공합니다.

```kotlin
fun main() {
    val str = "Hello"
    // this
    str.run {
        println("The receiver string length: $length")
        //println("The receiver string length: ${this.length}") // does the same
    }

    // it
    str.let {
        println("The receiver string's length is ${it.length}")
    }
}
```

### this

`run`, `with` 및 `apply`는 컨텍스트 객체를 람다 *receiver*로 참조합니다. 따라서 람다에서 객체는 일반 클래스 함수에서와 같이 사용할 수 있습니다. 대부분의 경우 *receiver* 오브젝트의 멤버에 액세스 할 때 `this` 생략하여 코드를 더 짧게 만들 수 있습니다. 한편, `this`가 생략되면, *receiver* 부재와 외부 물체 또는 기능을 구별하기가 어려울 수있다. 따라서 컨텍스트 객체를 *receiver* (`this`)로 갖는 것은 객체 객체에서 주로 작동하는 람다에 권장됩니다. 함수를 호출하거나 속성을 할당하십시오.

```kotlin
val adam = Person("Adam").apply { 
    age = 20                       // same as this.age = 20 or adam.age = 20
    city = "London"
}
```

### it

`let` 과 `also`는 컨텍스트 객체를 람다 인수로 사용합니다. 인수 이름을 지정하지 않으면 내재 된 기본 이름 `it`으로 오브젝트에 액세스합니다. `it`은 `this`보다 짧으며 일반적으로 `it`표현식이 읽기 쉽습니다. 그러나 객체 함수 또는 속성을 호출 할 때 `this`와 같이 암시 적으로 사용 가능한 객체가 없습니다. 따라서 객체를 함수 호출에서 인수로 사용하는 경우 컨텍스트 객체를 `it`'으로하는 것이 좋습니다. 코드 블록에 여러 변수를 사용하는 경우 `it`도 좋습니다. 

```kotlin
fun getRandomInt(): Int {
    return Random.nextInt(100).also {
        writeToLog("getRandomInt() generated value $it")
    }
}

val i = getRandomInt()
// INFO: getRandomInt() generated value 55
```

또한 컨텍스트 개체를 인수로 전달할 때 범위 내에서 컨텍스트 개체의 사용자 지정 이름을 제공 할 수 있습니다.

```kotlin
fun getRandomInt(): Int {
    return Random.nextInt(100).also { value ->
        writeToLog("getRandomInt() generated value $value")
    }
}

val i = getRandomInt()
```

## Return value

범위 함수는 반환 결과에 따라 다릅니다.

- `apply`및 `also`컨텍스트 개체를 반환합니다.
- `let`,`run` 및`with`는 람다 결과를 반환합니다.

이 두 가지 옵션을 사용하면 코드에서 다음에 수행 할 작업에 따라 적절한 기능을 선택할 수 있습니다.

#### 컨텍스트 객체(Context object)

`apply`와`also`의 반환 값은 컨텍스트 객체 자체입니다. 따라서 이러한 단계는 호출 단계에 부가 단계로 포함될 수 있습니다. 함수 호출은 이후에 동일한 객체에서 함수 호출을 계속 연결할 수 있습니다. 따라서, 그것들은 부가 단계(*side step*)로서 콜 체인에 포함될 수 있습니다. 동일한 객체에서 함수 호출을 계속 연결 할 수 있습니다.

```kotlin
val numberList = mutableListOf<Double>()
numberList.also { println("Populating the list") }
    .apply {
        add(2.71)
        add(3.14)
        add(1.0)
    }
    .also { println("Sorting the list") }
    .sort()
```

```
Populating the list
Sorting the list
[1.0, 2.71, 3.14]
```

컨텍스트 객체를 반환하는 함수의 return 문에도 사용할 수 있습니다.

```kotlin
import kotlin.random.Random

fun writeToLog(message: String) {
    println("INFO: $message")
}

fun main() {
    fun getRandomInt(): Int {
        return Random.nextInt(100).also {
            writeToLog("getRandomInt() generated value $it")
        }
    }

    val i = getRandomInt()
}
```

```
INFO: getRandomInt() generated value 3
```

