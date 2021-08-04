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

## Function with varargs Parameter


```kotlin
fun main(args: Array<String>) {
    fun printAll(vararg messages: String) {
        for (m in messages) println(m)
    }

    printAll("hello", "world ")

    fun printAllWithPrefix(vararg messages: String, prefix: String) {
        for (m in messages) println(prefix + m)
    }

    printAllWithPrefix("hello", "world", prefix = "messsage is ")

    fun log(vararg entries: String) {
        printAll(*entries)
    }

    log("hello", "world")
}
```

* 변수를 키워드로 list 형태를 사용할 수 있다.
* named parameters 덕분에, `prefix`를 선언해서 별도로 사용할 수 있다.
* 실행 시, varargs 는 배열이다. varargs 를 다른 함수에 전달하려면 `*`키워드를 사용해야 한다.

## Variables

* 코틀린은 강력한 타입추론이 가능하다.
* `var` 보다 `val` 을 추천한다.
* 반드시 초기화 해야한다.

## Null Safety

```kotlin
fun main(args: Array<String>) {
    var neverNull: String = "test"
    neverNull = null

    var nullable:String? = "test"
    nullable = null
}
```

* `null` 을 사용하려면, `?` 키워드를 사용해야 한다.

## Class

* Class 맴버 변수가 없다면, 이름만 선언할 수 있다. 
* Class 내에 메소드가 없다면, `{}`없이 맴버 변수만 선안할 수 있다.
* new 키워드를 사용하지 않는다.
* 클래스 맴버 변수에 var, val 을 선언하지 않으면 접근할 수 없다. 생각에는 private 으로 선언하는 것 같다.

```kotlin
class Contact

class Customer(val id:Int, var name:String)

fun main(args: Array<String>) {
    val contact = Contact()
    val customer = Customer(1, "test")

    customer.name = "Test"
    println("id = " + customer.id)
    println("name = " + customer.name)
}
```









































