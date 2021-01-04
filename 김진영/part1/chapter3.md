# Chapter3 데이터 타입 기본

> 스위프트의 기본 데이터 타입은 모두 구조체를 기반으로 구현되어 있다

## 3.1 Int와 UInt

- Int: +, - 부호를 포함한 정수  
- UInt: - 부호를 포함하지 않는 0을 포함한 양의 정수  
- *max와 min 프로퍼티로 각 타입의 최댓값과 최솟값을 알아볼 수 있다*
> Int.min < UInt.min (== 0) < Int.max < UInt.max
- 각각 8비트, 16비트, 32비트, 64비트의 형태가 있다  
    - 즉 Int8, Int16, Int32, Int64, UInt8, UInt16, UInt32, UInt64가 존재
> 32비트 아키텍처에서는 Int32가 Int타입으로, UInt32가 UInt타입으로 지정  
> 64비트 아키텍처에서는 Int64가 Int타입으로, UInt64가 UInt타입으로 지정

진수별 정수 표현

~~~ swift
let decialInteger: Int = 28
let binaryInteger: Int = 0b11100    // 2진수로 10진수 28을 표현
let octalInteger: Int = 0o34        // 8진수로 10진수 28을 표현
let hexadecimalInteger: Int = 0x1C  // 16진수로 10진수 28을 표현
~~~

## 3.2 Bool

불리언 타입은 참(true) 또는 거짓(false)만 값으로 가짐

~~~ swift
var var1: Bool = true
var1 = false
~~~

## 3.3 Float과 Double

(64비트에서)  
Float: 32비트 부동소수 표현, 6자리 십진수까지 표현 가능  
Double: 64비트 부동소수 표현, 최소 15자리 십진수 표현 가능  

~~~ swift
var floatValue: Float = 126456.1
var doubleValue: Double = 1234567890.1
~~~

## 3.4, 3.5 Character, String

~~~ swift
let alphabetA: Character = "A"
let name: String = "yagom"
print(name.hasPrefix("ya")) // true
print(name.count)           // 5
~~~

## 3.6 Any, AnyObject와 nil

Any는 모든 데이터 타입을 사용할 수 있다는 뜻이다  
AnyObject는 클래스의 인스턴스만 할당할 수 있다  
nil은 특정 타입이 아니라 '없음'을 나타내는 키워드이다. 타 언어의 null, NULL과 유사하다

~~~ swift
var someVar: Any = "yagom"
someVar = 50
someVar = 100.1
someVar = true
~~~

