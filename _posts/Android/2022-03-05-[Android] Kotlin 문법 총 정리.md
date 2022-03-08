---
layout: single
title: "[Android] Kotlin 문법 총 정리" 
categories: ['Android', 'Language', 'Kotlin']
tag: ['Android', 'Intro']
toc: true
toc_sticky: true
---

<br>

이 카테고리는 안드로이드에 관한 프로젝트를 구현해 보는 카테고리로써 여러 프로젝트를 해 보기 전에 안드로이드 문법인 코틀린 언어에 대해서 먼저 알아보도록 할 것이다.

안드로이트 스튜디오 사용방법에 대해 익히는 것보다 프로젝트를 하나씩 해 보면서 여러가지를 배우는 것이 재미적인 측면에서나 효율적인 측면에서 더 좋을 것이다.

그럼 이 프로젝트를 해 보기에 앞서 사용할 언어인 코틀린 언어에 대해서 알아보도록 하겠다. 참고로 코틀린 문법을 최대한 많이 정리를 해 놓으려고 하고 있고 계속 해서 추가할 예정이므로 이 포스팅의 길이는 매우 길 것이므로 유의하면서 읽기 바란다. 

![image](https://user-images.githubusercontent.com/79521972/156873100-79d876aa-91ca-4d3e-a43d-2251ec53edfb.png)

Kotlin이라는 언어를 들어본 사람도 있을 것이고 들어보지 못한 사람도 있을 것이다.

통상적으로 안드로이드 개발을 하기 위해서는 Kotlin 혹은 Java언어를 사용해서 구현을 한다. 만약 안드로이드 개발을 해봤다면 자바로 안드로이드 개발을 했던 사람도 있을 것이고 아니면 안드로이드 개발은 처음하지만 바로 코틀린 언어로 개발을 하기 위한 사람도 있을 것이다.

이 Android 프로젝트(개발?) 챕터에서는 코틀린 문법으로 30여개의 프로젝트 및 실습을 진행할 것인데 이에 앞서 이 포스팅에서는 Kotlin 문법에 대한 정리를 먼저 해 볼것이다.

<br>

## 1. 코틀린 이란?

<img src="https://user-images.githubusercontent.com/79521972/156873252-36b7b1f9-3b02-41d1-94c2-436cfd7d5668.png" alt="image" style="zoom: 67%;" /><img src="https://user-images.githubusercontent.com/79521972/156873266-bbc5e978-e666-4a01-9ae9-55bb99bb44d1.png" alt="image" style="zoom: 67%;" />



코틀린은 JETBRAIN에서 만든 프로그래밍 언어이다. 이 JETBRAIN은 자바 통합 개발 환경 tool인 Intellij를 만든 기업이다.

이 Intellij를 기반으로 만든 프로그램인 우리가 계속 사용 하게 될 Android Studio이고 앞으로 4.1.2버전을 기반으로 사용 할 것이다.

<br>

코틀린은 자바 언어의 단점을 보완하기 위해 나온 언어이기 때문에 자바가 동작하는 환경에서는 100% 호환성을 보여주고 있다. 그렇기 때문에 자바로 개발할 수 있는 것들은 모두 코틀린으로 개발할 수 있는 것이다.

그래서 보통 앱개발을 한다고 하면 자바로 개발을 하는 경우가 거의 대다수였지만 코틀린이 나온 이후로 인기가 많아지면서 요즘은 거의 코틀린 언어를 사용하는 경우가 더 많다.

![image](https://user-images.githubusercontent.com/79521972/156873432-8d3cc0ea-ddb7-4b3b-9ce5-0b9499d4ce3a.png)

또한 Zetbrain과 긴밀한 관계를 갖고 있는 회사인 `Google`에서도 2017년도에 코틀린을 자바에 이어 안드로이드 공식 언어로 선언한 바가 있다.

<br>

## 2. Why Kotlin?

2017년도에는 코틀린이 공식언어로 지정되기도 하였고 2019년도에는 안드로이드 퍼스트 언어로 선정된 코틀린을 왜 사용해야 할까?

 자바를 보완하여 나온 언어이기 때문에 코틀린이 자바보다 우세한 점이 있기 때문에 더 사용하기 좋고 비교적 최신에 나온 언어이기 때문에 현대 언어적인 부분이 가미가 되어있다.

<br>

다음과 같은 코틀린의 특성을 보면서 왜 코틀린을 사용하면 좋은지 보도록 하겠다.

- 호환성(Compatibility)
  - 코틀린은 JDK6 (Java Development Kit 6)와 완벽하게 호환가능하다.
  - 구형 안드로이드 기기 지원이 된다.
  - 안드로이드 스튜디오에서 지원이 되는 언어이기 때문에 안드로이드의 빌드 시스템과 완벽 호환이 가능하다.



<br>

## 3. Kotlin 문법 훑어보기

**0. Hello World!**

우리가 컴퓨터 언어를 처음 배울 때 처음 보고 작성해 보는 코드는 `'Hello World'` 일 것이다. 코틀린 언어로 이 코드를 작성하면 다음과 같다.

```kotlin
fun main(args: Array<String>) {
    print("Hello World")
}
```

<br>

> **1. Function**

코틀린에서 함수는 다음과 같이 작성한다.

```kotlin
fun sum(a: Int, b:Int): Int {
    return a + b
}
fun sum(a: Int, b:Int) = a + b
```

- fun: 함수(function)의 약자인 fun을 사용하여 함수를 정의한다.
- sum: 함수 이름을 적는다.

- 괄호 안에 Input value를 적어준다.
  - `인자명: dataType`식으로 작성한다.
- Return하는 데이터 타입은 괄호 뒤에 콜론(:)과 함께 작성한다.
  - return 타입이 void인 경우 생략 가능하다.

<br>

- 두 번째로 작성한 함수는 `표현식 함수`로 동작은 동일하다.
  - 첫 번쨰로 작성된 함수는 `구문식 함수`이다.

- 표현식만 작성할 수 있는 것이 아니라 아래와 같은 문장도 작성 가능하다.

```kotlin
fun max(a: Int, b: Int) = if(a > b) a else b
```

여기서 쓰이는 if문에 대해서는 뒤에서 더 다루어 보도록 할 것이니 이런 식으로 작성이 가능하다 정도만 알아가도록 하자.

<br>

> **2. Value, Variable**

코틀린에서 변수와 상수 선언 방법에 대해 알아보도록 하자.

- var
  - var는 변수를 선언할 때 사용되는 keyword이다.
  - var로 선언한 변수는 값을 이후에 변경할 수 있다.
- val
  - val은 상수를 선언할 때 사용되는 keyword이다.
  - val로 선언한 상수는 값을 이후에 변경하는 것이 불가능하다.

```kotlin
val a: Int = 1
val b = 2    // Int 형으로 추론
val c = 3.14 // Double 형으로 추론
val d: String
d = "필수로 있어야 하는 구문"
//d = "d의 초기값이 없으면 null 될 수 있는데, d는 null이 될 수 없기 때문에."
val e: String?

var d: String = "첫 번째 초기화"
e = "두 번째 초기화"
```

상수는 value의 약자인 val을 사용하여 변수를 선언하며 이때 자바가 데이터 타입을 표현했던 것처럼 코틀린도 콜론(:)과 함께 표현할 수도 있지만 코틀린에서는 데이터 타입 추론이 가능하기 때문에 생략하여도 문제없다.

정수를 입력하면 Int형으로 추론하고 실수를 입력하면 Double형으로 추론한다.

String 형 변수 d를 선언하고 초기화를 하지 않았는데 코틀린에서는 null safe라는 개념이 존재하여 반드시 선언을 하고 초기화를 해 주어야 한다. 그래서 그 다음줄에 문자열로 초기화를 해 준 모습이다.

<br>

val은 다른 언어의 상수와 마찬가지로 한 번 초기화가 되면 다시 초기화가 불가능하며 var은 변수와 마찬가지로 한 번 초기화 후에 다시 초기화 하는 것이 가능하다.

<br>

> **3. Data Type**

코틀린은 자바와 100% 호환이 되기 때문에 자바의 데이터 타입과 동일한 데이터 타입들이 존재한다.

- Numbers(숫자)

  - 정수형
    - Byte
    - Short
    - Int :    123
    - Long: 123L
    - 위 처럼 정수를 Long형으로 추론하도록 하기 위해선 숫자 뒤에 "L"을 붙여준다.
  - 실수형
    - Float :    123.4f
    - Double: 123.4
    - 위 처럼 실수를 Float형으로 추론하도록 하기 위해선 숫자 뒤에 "f"을 붙여준다.

- 그 외

  - Char

  - String

    - ```kotlin
      var str String = "abcd"
      str = "abcd" + 1     //abcd1
      str = "abcd" + "efg" //abcdefg
      ```

    - String 타입에서의 덧셈(+) 연산은 문자열이 붙여지는 개념이며 위 코드의 두 번째 줄과 같이 뒤에 number가 오더라도 string으로 변환되어 뒤에 더해지는 것을 확인할 수 있다.

    - ```kotlin
      ar a:Int = 10
      ar longStr = """문자열을
      	여러줄로 쓰고 싶을 땐 이렇게
      	합시다.
      """
      println(longStr)
      //문자열을
      //여러줄로 쓰고 싶을 땐 이렇게
      //합시다.
      ```

    - 

  - Boolean

    - ```kotlin
      val myTrue: Boolean = true
      val myFalse: Boolean = false
      val boolNull: Boolean? = null
      ```

    - 위 코드의 세 번째 줄처럼 Boolean 뒤에 "?"가 붙게 되면 이는 `Nullable`하다는 의미로 붙지 않은 것과 다르게 `true, false`와 더불어 `null`도 들어갈 수 있다는 것을 의미한다.



<br>

> **4. For 반복문**

```kotlin
for (i in 1..5) {
    println(i)
}
// 1 2 3 4 5

for (i in 6 downTo 0 step 2) {
    println(i)
}
//6 4 2 0

for (i in 1..5 step 3) {
    println(i)
}
//1 4

for (i in 0 until 5) {
    println(i)
}
// 0 1 2 3 4

val numberList = listOf(100, 200 ,300)
for (number in numberList) {
    println(number)
}
//100  200  300
```

코틀린에서 반복문은 자바보다 파이썬 문법과 더 비슷한 것을 볼 수 있다.

- 자바와 다르게 i를 선언하지 않아도 된다.
- for i in iterable객체 으로 표현된다.
- 1..5 : 1부터 5까지 (python으로 따지면 range(1,6))
- 6 downTo 0 step 2 : 6부터 0까지 내려가면서 숫자를 counting하는데 간격(step)이 2이다. 
- 정리하자면 올라가는 것은 `..`으로 내려가는 것은 `downTo`를 사용한다.
- 리스트를 만들 때는 listOf() 함수를 사용한다.
- until의 의미는 크기 - 1을 의미한다.(0 until 5 : 0부터 5까지 즉, 0 1 2 3 4)

위 코드를 보면 그래도 다른 언어와 비슷하여 이해할 수 있을 것이다. 하지만 코틀린 언어가 조금 더 사람 언어와 비슷하게 작성한다는 점을 알 수 있을 것이다.

<br>

> **5. While 반복문**

```kotlin
var x = 5
while (x > 0) {
    println(x)
    x--
}
//5 4 3 2 1

x = 0
while (x > 0) {
    println(x)
    x--
}
//출력 없음

var y = 0
do {
    print(y)
    y--
} while (y > 0)
// 0
```

코틀린에서 While문은 자바와 비슷하게 사용된다.

while, do~while문 존재



<br>

> **6. If 문**

```kotlin
var max: Int
if (a > b) {
    max = a
} else {
    max = b
}

//As expression
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

if 문도 자바언어와 크게 다를 것이 없다.



<br>

> **7. When 문**

 ```kotlin
 when (x) {
     1 -> print("x == 1")
     2 -> print("x == 2")
     else -> {
         print("x is neither 1 nor 2")
     }
 }
 
 when (x) {
     0, 1 -> print("x == 0 or x == 1")
     else -> print("otherwise")
 }
 
 when (x) {
     in 1..10 -> print("x는 1부터 10 범위 안에 있음")
     !in 10..20 -> print("x는 10부터 20 범위 안에 없음")
     else -> print("otherwise")
 }
 
 when (x) {
     is Int -> print("x는 int type")
     else -> print("x is not a int type object")
 }
 ```

코틀린은 사람이 말하는 것과 비슷하게 만들어졌기 때문에 코드만 봐도 무슨 말을 하는지 알 수 있을 것이다.

자바에는 switch문 대신 when문이 있다.

평소 자바나 c언어를 하면 switch문이 왜 switch라는 이름을 가지고 있는지 의문이 든 적이 많았는데 코틀린은 직관적으로 when이라는 문자를 사용하여 **변수 x가 특정 상태일 때** 화살표(->)뒤의 문장을 실행하는 구조를 가지고 있다.

<br>

> **8. 배열**

배열의 타입을 코틀린에서 Array이다. Array라는 타입을 코틀린 컴파일러에서 충분히 유추할 수 있다면 생략할 수 있다.

배열의 생성은 arrayOf()라는 함수를 이용하며 배열의 생성과 초기화를 한 번에 할 수도 있다.

```kotlin
val numbers1 : Array<Int> = arrayOf(1,2,3,4,5)
val numbers2 = arrayOf(1,2,3,4,5)
```

<br>

지금까지는 코틀린의 기초 문법으로 이것만 알아도 어플제작이 가능하다. 이보다 심화적인 내용은 계속 이어나가겠다.

# 심화 문법

## 1. 클래스

### 0) Data Class

코틀린에서는 data class라는 데이터만을 초기화하는 클래스를 별도로 구현이 가능하다. 이는 자바에서 굉장히 긴 코드를 한 줄만에 구현할 수 있기 때문에 강력한 키워드 이다.

```kotlin
data class JavaObject(val s: String)
```

```java
//In Java
public class JavaObject {
     
    private String s;
    
    JavaObject(String s) {
        this.s = s;
    }
    
    public String getS() {
        return s;
    }
    
    public void setS(String s) {
        this.s = s;
    }
    
    //copy
    //toStrin
    //hashCode 등등 생략
}
```



### 1) 클래스 선언

자바에서 클래스를 만들고 이를 객체로 생성하는 과정에서 사용했던 new 키워드를 코틀린에서는 사용하지 않는다. 

```kotlin
class Fruit {

}

val fruit = Fruit()
```

### 2) 생성자 

**간단한 구조의 생성자**

```kotlin
class Fruit (var weight : Double){

}
```

<br>

constructor 구문에서 초기화 구문을 작성할 수 있다. 

```kotlin
class Fruit() {
    private var weight: Double = 0.0

    constructor(weight : Double){
        this.weight = weight
    }
}
```

constructor 구문이 하나일 경우 이것을 primary constructor로 변환하라는 경고문구가 나타난다. primary constructor라는 것은 클래스 이름 옆에 () 안에 멤버변수들을 나열하는 것을 의미한다.
primary constructor를 사용하는 구문으로 변경한 것은 아래와 같다.

```kotlin
class Fruit(private var weight: Double) {
    init {
        println("weight :: $weight")
    }
}
```

### 3) 프로퍼티



자바에서 클래스의 멤버필드라고 하던 것을 코틀린에서는 프로퍼티라고 한다. 멤버필드에 getter/setter로 접근할 수도 있지만 직접적으로 프로퍼티에 접근한다.

코틀린 클래스 파일에서 작성했지만 자바 코드에서 사용하려면 바로 프로퍼티에 접근이 불가능하기 때문에 코틀린 클래스 생성 시 getter/setter가 자동으로 생성된다.

```kotlin
// 생성자와 함께 클래스를 선언
class Fruit (var weight: Double){
}

// 생성자를 이용한 객체생성 (1)
val banana : Fruit = Fruit(0.1)
println("weight of an banana is ${banana.weight} kg")

// 생성자를 이용한 객체생성 (2) : weight 프로퍼티(=필드)를 명시적으로 지정해 초기화했다.
val apple : Fruit = Fruit(weight = 0.2)
println("weight of an apple is ${apple.weight} kg")

// 프로퍼티(=멤버 필드) 접근시 getter/setter 없이 접근 가능하다.
apple.weight = 0.15
println("weight of an apple is ${apple.weight} kg")
```

생성자에 인자가 많을 때는 Fruit(weight = 0.2) 와 같은 식을 이용해 인자를 지정해주는 것 역시 다른 언어와 마찬가지로 가능하다. (자바는 불가능)

<br>

출력을 해 본 결과 필드에 직접 접근해 apple의 weight을 변경한 결과가 제대로 반영된 것을 확인 가능하다.

```null
weight of an banana is 0.1 kg
weight of an apple is 0.2 kg
weight of an apple is 0.15 kg
```

### 4) 접근제한자

변수, 함수의 접근 범위를 지정할 때 사용하는 키워드이다. 이도 자바와 비슷하나 조금 다른 점(internal)이 있다. 좀 더 키워드의 뜻과 그 의미가 일맥상통 하도록 수정한 것 같다.

- public
  - 전체공개
- private
  - 현재 파일 내에서만 사용가능
- internal
  - 같은 모듈 내에서만 사용가능
  - 예를 들어 app 모듈 내에 tv, smartphone, watch 모듈이 있고, app 모듈 내에 Fruit 클래스에 internal로 선언한 필드 weight 이 있다면, tv, smartphone, watch 모듈에서 모두 weight 필드에 접근 가능한 것이다.
- protected
  - 상속받은 클래스에서 사용할 수 있다.

앞에 어떠한 접근 제한자도 적지 않으면 public으로 간주한다.

```kotlin
open class Fruit{
    val name = "전체공개"   // 아무 키워드도 주지 않으면 기본적으로 public 으로 세팅
    private val vendor:String = "private"
    protected val vender1:String = "protected"
    internal val vender2:String = "internal"

    fun getVender1():String {
        return vender1
    }
}

class Apple : Fruit() {
    fun get_vender1() : String {
        return vender1
    }
}

val aFruit : Fruit = Fruit()
val anApple : Apple = Apple()

// public 멤버 접근
println(aFruit.name)

// private 멤버 접근
//println(aFruit.vender) // 컴파일 에러
println(aFruit.getVender1())

// protected 멤버 접근
// 확장클래스(상속받은 클래스)에서 protected 멤버인 vender1 에 접근
println(anApple.get_vender1())

// internal 멤버 접근 
// 확장클래스(상속받은 클래스)에서 internal 멤버인 vender2 에 접근
println(anApple.vender2)
```

### 5) 클래스 상속

코틀린에서 클래스는 기본적으로 상속이 금지되는 것이 원칙이다. 상속이 가능하도록 허용하기 위해서는 open 키워드를 클래스 선언 앞에 추가하면 된다. 

어쩌면 이런 방식이 조금 더 명확해서 다른 클래스에서 상속하고 있다는 것을 보여줄 수 있는 장점이 된다.

하지만 어떤 면에서는 라이브러리(예를 들면 resilience4j) 등을 만드는 설계자들의 입장에서는 기존에 만들었던 클래스를 상속하고자 할 때 open 키워드를 새로 추가하게 되어 구버전과 신버전사이의 호환이 어려워지도록 할 수 있다는 단점 또한 있는 듯 하다.

```kotlin
open class Fruit{}
class Apple : Fruit(){}

open class Person(var name : String)
class Student(var grade : Int) : Person("Jordan")
```

### 6) 내부 클래스

다른 언어들에서와 같이 클래스 내부에 클래스를 선언하는 것을 내부ㅜ클래스라고 한다. 내부 클래스 선언시에는 inner 키워드를 사용한다.

```kotlin
class Student {
    var name : String = "Jordan"

    init{
        println("My name is $name")
    }

    inner class Personality {
        fun doSomething() : Unit {
            name += ", He is Chicago Bulls No.23 Basketball Start"
            println(name)
        }
    }
}

val student : Student = Student()
student.Personality().doSomething()
```

**출력결과**

```null
My name is Jordan
Jordan, He is Chicago Bulls No.23 Basketball Start
```

### 7) 추상 클래스

추상 클래스는 미구현 메소드를 포함하는 클래스다. 이 역시도 자바와 마찬가지의 개념이며java 에서 지원하는 abstract 키워드를 사용한다. 

클래스와 미구현 메서드 앞에는 abstract 키워드를 붙인다. 추상 클래스는 직접 인스턴스화 할 수 없고 다른 클래스에서 직접 상속(확장)해서 미구현 메서드를 구현해야 한다. 

```kotlin
abstract class Printer {
    abstract fun doPrint(): Printer
    fun sayCategory() : Unit {
        println("This Product is a type of Printer")
    }
}

class LaserPrinter : Printer(){
    override fun doPrint() : Printer {
        println("치직치직~ ... 프린트가 완료되었습니당~")
        return this
    }
}

class InkJetPrinter : Printer(){
    override fun doPrint() : Printer {
        println("위잉위잉~ ... 프린트가 완료되었습니다~")
        return this
    }
}

val laserPrinter : LaserPrinter = LaserPrinter()
val inkJetPrinter : InkJetPrinter = InkJetPrinter()
laserPrinter.doPrint().sayCategory()
inkJetPrinter.doPrint().sayCategory()

val printer : Printer = Printer()           // 에러
                                            // abstract 클래스를 인스턴스로 생성하려 했으므로 에러
var lPrinter : Printer = LaserPrinter()
```

**출력결과**

```null
치직치직~ ... 프린트가 완료되었습니당~
This Product is a type of Printer
위잉위잉~ ... 프린트가 완료되었습니다~
This Product is a type of Printer
```

### 8) 인터페이스

 추상클래스가 인터페이스와 다른점은 하나의 클래스에서 여러개의 인터페이스를 구현할 수 있다는 점이다. 

추상 클래스의 경우 클래스 개념이기 때문에 하나의 클래스에서 하나의 추상클래스만을 상속받아 구현할 수 밖에 없다.

java 8 부터 인터페이스는 default 메서드라고 불리는 구현된 메서드를 포함할 수 있게 되었는데 이 개념을 코틀린에서도 역시 사용 가능하다.

```kotlin
interface Eat {
    fun eat() : Eat

    // java 8 부터 인터페이스는 default 메서드라고 불리는 구현된 메서드를 포함할 수 있다.
    fun pay() : Unit {
        println("계산")
    }
}

interface Workout {
    fun workout() : Unit
}

open class Person {
    fun walk() : Unit {
        println("걷는다")
    }
}

class Student : Person() ,Eat, Workout {
    override fun eat() : Eat {
        println("점심을 먹는다")
        return this
    }

    override fun workout() {
        println("헬스장에 간다")
    }
}

val student : Student = Student()
student.walk()
student.eat().pay()
student.workout()
```

출력결과

```null
걷는다. 
점심을 먹는다
계산
헬스장에 간다.
```



## 2. Null 처리, null 가능성, nullable

코틀린에서는 기본적으로 객체를 불변으로 취급하고 null 값을 허용하지 않는다. 

null 값을 허용하기 위해서는 별도의 연산자를 사용해 초기화한다. null 값이 허용된 자료형을 사용할 때에서 별도의 연산자를 통해 안전하게 호출해야 한다.

null 이 허용되는 변수를 선언할 때는 `?` 와 같이 표현식을 사용하면 null 이 허용되는 대입연산을 수행하게 된다. 그래서 String? 와 String 타입은 엄연히 다른 것이다.

### 1) ? 연산자 - null 허용

```kotlin
val sample : String          // 초기화를 진행하지 않았기 때문에 에러 발생
val a : String  = null       // 일반적으로는 null로 초기화할 수 없기 때문에 에러 발생
var a : String? = null       // 정상
```

### 2) lateinit 키워드 (var 변수의 늦은 초기화)

초기값이 없는 변수를 초기화 하고 싶을 때 사용하는 키워드이다.(코틀린에서는 초기값없이 변수 선언이 불가능하기 때문에)

```kotlin
var nullableNumber: Int? = null

lateinit var lateinitNumber: Int

//추후 초기화하는 코드
lateinitNumber = 10

//사용할 때
nullableNumber?.add()

lateinitNumber.add()
```



var 로 선언한 변수를 일단 선언만 해놓고 초기화는 나중에 진행하는 경우 lateinit 키워드를 사용한다.

```kotlin
lateinit var a : String
a = "안녕 세상~"
println(a)

lateinit var b : String
println(b)
```

- var 키워드를 사용하여 선언한 경우에만 사용 가능
- null타입이 아닌 경우에만 사용가능
- 초기화 전에는 변수를 사용할 수 없다.
- int, Long, Double, Float 에는 사용할 수 없다.



### 3) lazy 키워드 (val 변수의 늦은 초기화)

NullSate한 코드를 사용하기 위해서 non-null Type으로 변수를 선언함. 변수는 미리 선언해 놓고 사용할 때 할당해 주기 위함.

```kotlin
val lazyNumber :Int by lazy {
    100
}

//사용하기 전까지는 lazyNumer라는 변수에 100이 할당되지 않음

lazyNumber.add()
//사용할 때 100이 할당 됨
```

<br>

val로 선언한 변수를 일단 선언만 해놓고 초기화는 나중에 진행할 때 사용한다. 따라서 val을 사용하는 경우에만 사용이 가능하다.

val 선언뒤에 by lazy 블록에 초기화에 필요한 코드를 작성한다. 마지막줄에는 초기화 할 값을 작성해준다.

아래 예제에서는 a 라는 상수를 초기화 할 때 println() 으로 "안녕세상~"이라는 문구를 출력하고 있다. 그런데 상수 a는 두번 참조되어 진다. 여기서 처음 참조될 때에만 by lazy {…} 구문내의 초기화 구문이 실행되고 그 이후부터는 초기화구문 없이 값의 반환만을 수행하게 된다.

```kotlin
val a : String by lazy {
    println("안녕세상~")
    "헬로~"
}

println(a)
println(a)
```

참고)

- lateinit
  - var 로 선언한 변수를 늦은 초기화 할 때 사용
- by lazy { … }
  - val 로 선언한 변수를 늦은 초기화 할 때 사용

### 4) null 값이 아님을 보증 (!!)

변수 뒤에 `!!`을 추가하면 null 값이 아님을 보증한다는 의미이다.

아래의 예제를 보면 name1 의 타입은 String? 이다. String? 타입은 String 타입과 명백히 다르다.

String? 타입의 변수를 다른 변수에 저장할 때는

- String? 타입의 변수는 String? 타입에 저장하거나
- String? 타입의 변수의 뒤에 !!을 붙여서 String 타입으로 변환시켜주어야 한다.

참고)

연산자 `!!`가 실제로 String? 을 String 타입으로 변환하는 것과 같은 동작을 하는지는 확실히 모른다. 일단 예제가 이런 예제가 있어서 정리해봤다.

```kotlin
val name1: String? = "과일"
val name2: String  = name1            // 에러
val name3: String? = name1            // 정상
val name4: String  = name1!!          // 정상
```

### 5) 안전한 호출 (?.)

메서드 호출시 . 대신 ?. 연산자를 사용하면 **null 값이 아닌 경우에만 호출하게 된다.**

```Kotlin
Integer a = 100;

val b: Int? 100
val c: Int = 100 
```

```kotlin
a = null;
///중략///
a.sum(); // NullPointerException 발생 가능

//null safe code
if(a != null) {
    a.sum();
}

b?.sum() // null일 경우 실행하지 않음.
c.sum()  // 애초에 nullsafe함(c 변수 정의에 의해)
```



### 6) 엘비스 연산자 '?:'

안전한 호출 키워드인 ?. 을 사용시 null 아닌 기본 값을 반환하고 싶을경우 엘비스 연산자를 함께 사용한다.

```kotlin
val str: String? = null
var upperCase = if (str != null) str else null
upperCase = str?.toUpperCase() ?: "str 변수는 초기화를 해야만 upperCase() 가 가능합니다."
println(upperCase)
```

**출력결과**

```null
str 변수는 초기화를 해야만 upperCase()가 가능합니다.
```

<br>

## 3. 컬렉션

**1) 리스트**

리스트는 배열 또는 java의 리스트와 같은 자료구조다.

- 중복된 아이템을 가질 수 있다.
- 추가/삭제/교체 메서드 등을 제공한다.
- listOf(), mutableListOf() 메서드로 리스트를 생성가능하다.
- listOf는 요소를 변경할 수 없는 읽기 전용 리스트를 생성할 때 사용한다.
- mutableListOof는 요소를 변경할 수 있는 리스트를 작성할 때 사용한다.

자바에서 리스트의 특정 요소에 접근할 때 .get(i)를 사용했는데 코틀린에서는 단순하게 [i]를 통해 접근가능하다.

**listOf**

```kotlin
//요소를 변경할 수 없는 읽기 전용 리스트 생성은 listOf() 메서드로 생성
val fwPlayers1: List<String> = listOf("손흥민", "황희찬", "황의조")
//타입 추론으로 자료형을 생략 가능하다.
val fwPlayers2 = listOf("손흥민", "황희찬", "황의조")

println("left wing : ${fwPlayers1[0]}")
```

출력결과

```null
left wing : 손흥민
```

**mutableListOf**

```kotlin
val fwPlayers2 = mutableListOf("손흥민", "황희찬", "황의조")
fwPlayers2.add("윤종신")

val removed = fwPlayers2.removeAt(2)
println("${removed} was removed.")

fwPlayers2[2] = "황의조"
println("${fwPlayers2[2]} was added")
```

**출력결과**

```null
황의조 was removed.
황의조 was added
```

### 2) 맵

mapOf() 메서드로 읽기 전용 맵 자료를 만들수 있다. mutableMapOf() 메서드로 수정이 가능한 맵을 만들 수 잇다. 대괄호 ([]) 안에 key 를 요소명으로 지정해서 접근가능하다. (마치 javascript와 유사한 쓰임새이다.)

```kotlin
// 읽기전용 맵
val map1 = mapOf("left" to "손흥민", "center" to "헤리케인", "right" to "루카스 모우라")

// 변경가능한 맵
val map2 = mutableMapOf("left" to "손흥민", "center" to "헤리케인", "right" to "루카스 모우라")
println(map2.toString())

map2["center"] = "가레스 베일"
println(map2.toString())

//맵 전체의 키,값을 순회하기
for ((k,v) in map2){
    println("$k : $v")
}
```

**출력결과**

```null
{left=손흥민, center=헤리케인, right=루카스 모우라}
{left=손흥민, center=가레스 베일, right=루카스 모우라}
left : 손흥민
center : 가레스 베일
right : 루카스 모우라
```

### 3) 셋(Set), 집합

Set 은 중복되지 않는 요소들로 구성된 자료구조이다. setOf() 메서드로 읽기 전용 집합을 생성할 수 있고, mutableSetOf() 메서드로 수정가능한 집합을 생성한다.

```kotlin
val citySet = setOf("손흥민", "헤리 케인", "루카스 모우라")
val citySet2 = mutableSetOf("손흥민", "헤리 케인", "루카스 모우라")

citySet2.add("가레스 베일")
println(citySet2.toString())

citySet2.remove("루카스 모우라")
println(citySet2.toString())

println("size of 'citySet2' is ${citySet2.size}")
println("'citySet2' contains '루카스 모우라' :: ${citySet2.contains("루카스 모우라")}")

citySet2.add("루카스 모우라")
println("'citySet2' contains '루카스 모우라' :: ${citySet2.contains("루카스 모우라")}")
```

출력결과

```null
[손흥민, 헤리 케인, 루카스 모우라, 가레스 베일]
[손흥민, 헤리 케인, 가레스 베일]

size of 'citySet2' is 3
'citySet2' contains '루카스 모우라' :: false
'citySet2' contains '루카스 모우라' :: true
```

<br>

## 3. 람다식

 java 8 에서는 람다식을 만들때 interface를 선언해서 사용해왔다. 코틀린에서는 기본으로 함수 자체를 람다식으로 생성하는 것을 지원한다.

### 함수 자체를 람다식으로 선언하기

```kotlin
fun addNormal (x: Int, y: Int): Int {
    return x + y
}

fun addByLambda(x:Int, y: Int) = x + y
```

### 1) SAM 변환

코틀린에서는 추상 메서드 하나를 인수로 사용할 때 함수를 인수로 전달할수 있어서 편하다.

자바 기반의 인터페이스 들 중 메서드가 하나인 인터페이스를 구현할 때는 이 것을 람다식으로 변경가능하다.

ex)

```kotlin
button.setOnClickListener( object : View.OnClickListener {
    override fun onClick(v: View?){
        // 클릭 이벤트에 대한 처리
    }
})
```

위의 코드는 아래와 같이 람다식으로 줄여서 표현 가능하다.

```kotlin
button.setOnClickListener({ v:View ? -> 
    println("hello") // 클릭시 처리: 쓸게 없어서 println 을 입력했다.
})
```

<br>

메서드 호출 시 맨뒤(마지막)에 전달되는 인자가 람다식일 경우 람다식을 괄호 바깥으로 빼는것 역시 가능하다.

```kotlin
button.setOnClickListener { v:View? -> 
    println("hello") // 클릭시 처리
}
```

클릭 Listener 와 같은 리스너 작성시 v 인수를 사용하지 않을 경우 v 라는 이름의 인자는 _ 기호로 대치가능하다. 이건 무슨뜻인지 모르지만 일단 정리 (TODO)

```kotlin
button.setOnClickListener { _ -> 
    // 클릭 시 처리
}
```

람다식에서 인자가 하나인 경우 이를 아예 생략하고 람다 블록 내에서 인수를 it로 접근할 수 있다. 아래 코드에서 it는 View? 타입의 v 인수를 가리킨다.

```kotlin
button.setOnClickListener{
    it.visibility = View.GONE
}
```

<br>

## 5. 기타 기능(Scope Function)

코틀린 기본 라이브러리는 유용한 함수들을 제공한다. 이에 대해 알아보자.

- 확장함수

  - 원래 있던 클래스에 기능을 추가하는 함수

- 형변환

  - 숫자형 자료형끼리 쉽게 형변환 가능

- 타입 체크

  - 변수의 형이 무엇인지 검사하는 기능

- 고차 함수

  - 인자로 함수를 전달하는 기능

- 동반 객체

  - 클래스의 인스턴스 생성 없이 사용할 수 있는 객체

- let() 함수

  - null이 아닌 객체에서 lambda를 실행(?.으로 null이 아님을 보장 받음)

  - 블록에 자기 자신을 전달하고 수행된 결과를 반환하는 함수

  - ```kotlin
    val numver: Int?
    
    val sumNumberStr = number?.let {
        "${sum(10, it)}"
    }.orEmpty()
    //.orEmpty를 추가하게 되면 null인 경우는 빈 값으로 치환하는 과정이 이루어 진다.
    ```

    ```java
    //In Java
    Integer number = null;
    String sumnumberStr = null;
    
    if (number != null) {
        sumNumberStr = "" + sum(10, number);
    }
    ```

    

- with() 함수

  - 인자로 객체를 받고 블록에서 수행된 결과를 반환하는 함수

  - ```kotlin
    val person = person()
    
    with(person) {
        work()
        sleep()
        println(age)
    }
    ```

    ```java
    //In Java
    Person person = new Person();
    
    person.work();
    person.sleep();
    System.out.println(person.age);
    ```

- apply() 함수

  - 블록에 자기 자신을 전달하고 이 객체를 반환하는 함수

  - 주로 객체를 초기화 할 때

  - ```kotlin
    //Kotlin
    val person = Person().apply {
        firstName = "ChangRyong"
        lastName = "Kang"
    }
    
    //Java에서는 이렇게 사용하던 것
    Person person = new Person();
    person.firstName = "ChangRyong"
    person.lastName = "Kang"
    ```

- run() 함수

  - 익명함수 처럼 사용하거나, 블록에 자기 자신을 전달하고 수행된 결과를 반환하는 함수

  - 어떤 값을 계산할 필요가 있거나 객체 구성과 결과 계산이 한 번에 있을 때 유용

  - ```kotlin
    val result = service.run {
        port = 8080
        query()
    }
    ```

    ```java
    //In java
    service.port = 8080;
    Result result = service.query()
    ```

- also 함수

  - ```kotlin
    Random.nextInt(100).also {
        print("getRandomInt() generated value $it")
    }
    
    Random.nextInt(100).also { value ->
             print("getRandomInt() generated value $ value")
    }
    ```

  - ```java
    //자바에서는 이렇게 해야함
    int value = Random().nextInt(100);
    System.out.print(value);
    ```

<br>

### 1) 확장함수

기존 클래스에 함수를 추가할 수 있다. [클래스명].[추가하려는 메서드명] 으로 원하는 클래스에 함수를 추가할 수 있다. 확장함수 내에서는 자기 자신이 속한 클래스에 대한 객체를 this로 접근할 수 있고, 이 객체를 리시버 객체라고 부른다.

```kotlin
class Fish(var age:Int, var weight:Double){}
// 확장함수 선언 및 정의
fun Fish.printAge() = println("이 물고기의 나이는 ... ${age}살입니다.")
fun Fish.printWeight() = println("이 물고기의 무게는 ... ${weight}kg 입니다.")

val 고등어:Fish = Fish(100, 10.0)

고등어.printAge()
고등어.printWeight()
```

예제에서는 '고등어’라는 변수를 추가했는데, 한글 변수명이 지원되나 봤는데 지원된다는 점이 신기하기는 하다.

객체 '고등어’에 printAge(), printWeight() 메서드를 추가했는데, 이렇게 printAge(), printWeight()을 확장함수라고 한다. 컴파일 타임이 아닌 런타임에 어떤 동작을 추가할때 사용하지 않을ㄲ ㅏ싶다.

또 다른 예로 Int 자료형에 대한 예를 살펴보자

```kotlin
fun Int.product1000() = this*1000

val one: Int = 1
val two: Int = 2

println("일천원 : ${one.product1000()}")
println("이천원 : ${two.product1000()}")

//## 고차함수
fun add(x: Int, y: Int, callback: (sum: Int) -> Unit){
    callback(x + y)
}
```

자바에서는 기본 자료형에 기능을 추가하려면 상속을 받고 추가 메서드를 작성해야 했다. String 클래스의 경우 final로 상속이 막혀있어 이 마저도 불가능했다.

### 2) 형변환

- 숫자형 데이터간 형변환
  - 숫자형 자료형 끼리 서로 다른 타입으로 형변환이 가능하다.
  - to[숫자타입종류](https://velog.io/@gosgjung/코틀린언어-기본개념-총정리-스압주의) 메서드를 사용한다.
- 문자열을 숫자로 변환시
  - java에서와 같이 Integer.parseInt() 를 사용한다.
  - Long.parseLong(), Double.parseDouble() 이 없다는것은 조금 아쉽긴 하다.
- 일반 클래스 간에 형변환을 할 때는 as 키워드를 사용한다.
  - ex) 고등어 as Fish

```kotlin
//### 숫자형 데이터간 형변환
val a = 10L
val b = 20

val c = a.toInt()
val d = b.toDouble()
val e = a.toString()

//### 문자열을 숫자로 변환시
val strSeven = "7"
val str = Integer.parseInt(strSeven)

//### 일반 클래스 간에 형변환을 할 때는 as 키워드를 사용한다.
open class SeaFood{}
class Abalone : SeaFood(){} // Abalone 은 '전복'이다.

val abalone = Abalone()
val seafood = abalone as SeaFood
```

### 3) 타입 체크

애플리케이션을 개발하다 보면 타입을 체크해야 하는 경우가 자주 있다. 타입 체크는 is 키워드를 사용한다. java의 instanceOf 키워드와 동일한 역할을 수행한다.

```kotlin
val strMsg: String = "안녕하세용~"
if (strMsg is String){
    println(strMsg)
}
```

### 4) 고차 함수

코틀린에서는 함수의 인자로 함수를 전달할수 있고, 함수를 반환하는 것 역시 가능하다. 이렇게 함수를 인자로 받거나 반환할 수 있는 매개체 역할의 함수를 고차함수(High Order Fucntion)이라고 부른다. react 등을 접해본 사람이라면 고계함수, 고차함수 라는 단어에 익숙할 수 있을 것 같다.

```kotlin
fun add(x: Int, y: Int, callback: (sum: Int) -> Unit){
    callback(x + y)
}

add(5, 3, { println(it) })
add(5, 3) { println(it) }
```

add 함수는 x, y, callback 을 인수로 전달받는다. callback 이라는 인자는 함수타입을 파라미터로 전달받는다. (callback 이라는 이름의 파라미터는 Int를 인자로 받고 Unit을 반환형으로 하는 함수 파라미터이다.) 자바에서는 인터페이스를 이용해 람다식을 반들어 전달하는 편인데, 코틀린에서는 그렇게까지 하지 않아도 된다는 점이 편하다. 거기에 성능까지 빠르니 좋다고 볼수 있다.

함수는 {} 으로 감싸서 내부에서는 반환값을 it로 접근할 수 있다. 그런데 이렇게 {} 로 감싸는 것은 함수 호출 바깥에서 {}으로 감싸서 함수의 body(몸체)를 구현할 수도 있는 것 같다. 어차피 함수를 리턴하는 함수이기 때문인듯 하다. (IDE에서 권장되는 문법을 추천해준다.)

### 5) 동반 객체

애플리케이션을 개발하다보면 팩토리 메서드를 사용하는 경우가 있다. Fragment 를 사용하게 되는 경우을 예로 들 수 있다. Java에서는 static과 같은 정적인 메서드로 팩토리 메서드를 구현이 가능하다. 하지만 코틀린에서는 static과 같은 정적인 타이핑이 가능한 키워드를 제공하지 않는다.

코틀린에서는 동반객체(companion object)로 static 키워드와 같은 역할을 수행가능하다. (TODO 이부분 다시 공부가 필요하다.)

```kotlin
class Fragment {
    companion object {
        fun newInstance(): Fragment {
            println("생성됨")
        }
    }
}

val fragment = Fragment.newInstance()
```

### 6) let() 함수

let() 함수는 블록에 자기 자신을 인수로 전달하고 수행된 결과를 반환한다. 인수로 전달되는 자기 자신은 it로 참조하면 된다. let() 함수는 안전한 호출연산자인 ?. 와 함께 사용하면 null 값이 아닐 때만 실행하는 코드를 아래와 같이 표현 가능하다.

```kotlin
// fun<T,R> T.let (block: (T) -> R): R
var strNum1: String = "1"
val result:Int = strNum1?.let {
    Integer.parseInt(it)
}
println("let 을 통해 parseInt 한 결과는 ${result} 입니다.\n")
```

출력결과

```null
let 을 통해 parseInt 한 결과는 1 입니다.
```

### 7) with() 함수

with() 함수는 인자로 객체를 전달 받는데, 이 객체는 블록 내에 리시버 객체로 전달된다. 그리고 수행된 결과를 리턴(반환)한다. 리시버 객체로 전달된 객체는 this로 접근할 수 있는데, 이 this는 생략이 가능하다. 아래 예제처럼 this를 통한 연산은 모두 this. 을 제거하고 작성해도 된다.

with는 ?. 을 이용한 안전한 호출이 불가능하므로 strWorld가 null 이 아닐 경우에만 사용해야 한다.

```kotlin
// fun<T,R> with (receiver: T, block T.() -> R): R
var strWorld = "world"
with(strWorld){
    println(this.toUpperCase())
    println(toUpperCase())          // this 는 생략이 가능하다.
}
```

출력결과

```null
WORLD
WORLD
```

### 8) apply() 함수

apply() 함수는 블록내에 객체 자신이 리시버 객체로 전달된다. 그리고 이 객체가 반환된다. 객체의 상태를 변화시키고 그 객체를 다시 반환할 때 주로 사용된다.

```kotlin
// fun <T> T.apply(block: T.() -> Unit): T
class Fruit (var name : String, var price : Int){}
val f: Fruit = Fruit("APPLE", 1000)
var test = f?.apply{
    f.name = "apple"
    f.price = 1500
}
println("f      = ${f.name}, ${f.price}")
println("result = ${test.name}, ${test.price}")
```

출력결과

```null
f      = apple, 1500
result = apple, 1500
```

### 9) run() 함수

run() 함수는 마치 javascript에서 사용하던 즉시실행함수, 익명함수 등의 형식과 비슷해보인다.

- 익명함수 형식으로 사용되는 방식
- 객체 내에서 호출하는 방식

run() 함수는 익명함수처럼 사용하는 방법, 객체에서 호출하는 방법 모두를 제공한다.

<br>

**익명함수처럼 사용하는 벙법**

익명함수처럼 사용할 때는 블록의 결과를 반환한다. 블록 안에 선언된 변수는 모두 임시로 사용되는 변수다.

```kotlin
// fun <R> run(block: () -> R): R
var avg = run {
    val kor = 100
    val english = 80
    val math = 50

    (kor + english + math)/3.0
}

println(avg)
```

**출력결과**

```null
76.66666666666667
```

<br>

**객체에서 호출하는 방법**

```kotlin
// fun <R> run(block: () -> R): R
var str1: String = "hello"

str1.run{
    println(toUpperCase())
}
```

**출력결과**

```null
HELLO
```

<br>



## 6. 로그(log)

- 안드로이드 스튜디오로 개발을 할때는 이 로그를 많이 보게 될 것이다. Log 클래스를 코드 중간에 적재적소에 사용할 수 있다면 내가 만드는 어플이 어떤 flow로 실행되는지와 디버깅을 할 수 있게 된다.

> 자주 사용하는 로그 사용법
>
> > log.v()
>
> https://velog.io/@soyoung-dev/%EC%95%88%EB%93%9C%EB%A1%9C%EC%9D%B4%EB%93%9C-%EB%A1%9C%EA%B7%B8-%ED%99%9C%EC%9A%A9







코틀린 문법에 대해서 더 자세히 알고 싶으면 다음 링크를 참고하면 좋을 것이다.

- [코틀린 공식 사이트](https://kotlinlang.org)



























