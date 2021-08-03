# KOTLIN

## HELLO WORLD

1. package 를 정의한다. 
2. ,main에서 시작한다. 
3. println 으로 출력을 만든다.

```kotlin
package session01

fun main(args: Array<String>) {
    println("hello world " + args.size)   
}
```

## 문자내용을  Parsing

```kotlin
package session01

fun main(args: Array<String>) {
    println("Hello")
    printMessageWithPrefix(message = "hello world")
    printMessageWithPrefix(message = "hello world", prefix = "it is ")
    val result = sum (1, 2)
    println("result is $result")
    println(multiply(2, 4))
}

fun printMessage(message: String): Unit {
    println(message)
}

fun printMessageWithPrefix(message: String, prefix: String = "Info") {
    println("$prefix $message")
}

fun sum(x: Int, y: Int):Int {
    return x + y
}

fun multiply(x: Int, y: Int) = x * y


```

* 아무것도 Return 하지 못할 경우 Unit 을 입력한다.
* parameter에 기본값을 설정할 수 있다.
* 1줄로 함수를 만들 수 있다.
* 함수를 호출할 때, 파라미터를 설정할 수 있다.

## Infix Function

* Infix 란 수식을 이야기한다. 흔히 수식에 있는 +, -, *, / 같은 수식 기호다.
  *  Kotlin 은 이런 수식을 만들 수 있다. 

```kotlin
package session01

fun main() {
    infix fun Int.times(str: String) = str.repeat(this)
    println(2 times "Bye ")

    val pair = "ferrari" to "Katrina"
    println(pair)

    infix fun String.onto(other: String) = Pair(this, other)
    val myPair = "McLaren" onto "Lucas"
    println(myPair)

    val triplePair = "McLare" onto "Lucas" to "Katrina"
    println(triplePair)

    val sophia = Person("Sophia")
    val claudia = Person("Claudia")
    sophia likes claudia
    println(sophia)
}

class Person(val name: String) {
    val likedPeople = mutableListOf<Person>()
    infix fun likes(other: Person) {
        likedPeople.add(other)
    }

    override fun toString(): String {
        return "Person(name='$name', likedPeople=$likedPeople)"
    }
}
```
* Int 의 times를 다시 정의해서 수식 함수를 만든다.
* kotlin에는 Pair 클래스가 있다. (key, value) 로 값을 만든다.
* `onto` 함수는 바꿀 수 있다. 이름은 변경할 수 있다.
* `member` 클래스에 정의해서 사용할 수 있다. 
* 상관없이 알게 된 건데... function 안에 function을 정의할 수 있다.

## Operator Function

* `operator` 키워드를 사용하면 부호를 다시 정의해서 사용할 수 있다.
* `time()`는 `*` 로 대신해서 사용할 수 있다.
* range를 설정할 수 있다.

```kotlin
package session01

fun main(args: Array<String>) {
    operator fun Int.times(str: String) = str.repeat(this)
    println(2 * "Bye ")

    operator fun String.get(range: IntRange) = substring(range)
    val str = "너의 적을 용서해라. 괴롭히는 것은 문제가 있다."
    println(str[0..4])
}
```

































