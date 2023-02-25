
- 자바와 다른 점 위주로 기술

## Program entry point
```kotlin
fun main() {
	println("Hello world!")
}
```

## Functions
- 두개의 `Int`파라미터와 `Int`타입 리턴 값
```kotlin
fun sum(a: Int, b: Int): Int {
	return a + b
}
```

- 표현식으로 가능 (리턴 타입 유추)
```kotlin
fun sum(a: Int, b: Int) = a + b
```

- 리턴 값 없을 때
```kotlin
fun printSum(a: Int, b: Int): Unit {
	println("sum of $a and $b is ${a + b}")
}
```

- `Unit`생략 가능
```kotlin
fun printSum(a: Int, b: Int) {
	println("sum of $a and $b is ${a + b}")
}
```

## Variables
- 읽기 전용 지역변수(Read-only local variable)은 `val` 키워드 사용
- 오직 한 번만 할당 가능
```kotlin
val a: Int = 1   // 즉시 할당
val b = 2        // `Int`타입 유추
val c: Int       // 초기화 없는 타입 선언
c = 3            // 지연 할당
```

- `var` 키워드는 재할당(reassigned) 가능
```kotlin
var x = 5
x += 1
```

- 최상위에 변수 선언 가능
```kotlin
val PI = 3.14
var x = 0

fun incrementX() {
	x += 1
}
```

## Creating classes and instances
- `class`키워드로 정의
```kotlin
class Shape
```
- 클래스의 프로퍼티들은 선언시 정의 하거나 본문에 나열할 수 있음
```kotlin
class Rectangle(var height: Double, var length: Double) {
	var perimeter = (height + length) * 2
}
```
- 클래스 파라미터로 나열하면 매개변수가 있는 생성자로 자동 사용 가능
```kotlin
val rectangle = Rectangle(5.0, 2.0)
println("The perimeter is ${rectangle.perimeter}")
```
- 상속은 클래스간의 `:`으로 정의, 기본적으로 `final`클래스 이며 `open`키워드로 상속가능한 클래스로 변경 가능
```kotlin
open class Shape

class Rectangle(var height: Double, var length: Double): Shape() {
	var perimeter = (height + length) * 2
}
```

## String templates

- `String`내 변수 맵핑
- 템플릿안에 `expression`가능
```kotlin
var a = 1
var s1 = "a is $a"

a = 2
var s2 = "${s1.replace("is", "was")}, but now is $a"
```

## Conditional expressions
- `=`로 리턴 처리 가능
```kotlin
fun maxOf(a: Int, b: Int) {
	if (a > b) {
		return a
	} else {
		return b
	}
}

fun maxOf(a: Int, b: Int) = if (a > b) a else b
```

## When expression
```kotlin
fun describe(obj: Any): String =
	when (obj) {
		1           -> "One"
		"Hello"     -> "Greeting"
		is Long     -> "Long"
		!is String  -> "Not a string" 
		else        -> "Unknown"
	}
```

## Ranges
- `in`을 사용하여 범위 안의 숫자인지 확인
```kotlin
val x = 10
val y = 9
if (x in 1..y+1) {
	println("fits in range")
}
```
- 범위 밖의 숫자인지 확인
```kotlin
val list = listOf("a", "b", "c")

if (-1 !in 0..list.lastIndex) {
	println("-1 is out of range")
}
if (list.size !in list.indices) {
	println("list size is out of valid list indices range, too")
}
```

## Collections
- `Collections`안의 특정 데이터 포함여부 확인 시 `in` 사용
```kotlin
when {
	"pineapple" in fruits -> println("green and yellow color")
	"watermelon" in fruits -> println("red and green color")
}
```
- 람다 표현식 가능
```kotlin

```