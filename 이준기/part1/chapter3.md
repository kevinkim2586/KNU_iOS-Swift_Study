# Chapter3 데이터 타입 기본

>스위프트는 기본 데이터 타입은 모두 구조체를 기반으로 구현<br>
>대문자 카멜 케이스를 이용

## 3.1 Int와 Unit

- Int: +, - 부호를 포함한 정수, 0을 포함하지 않는 양의 정수는 UInt로 표현 
- 각각의 아키텍쳐에 따라서 표현되는 타입이 다르다.  
    - Int8, Int16, Int32, Int64, UInt8, UInt16, UInt32, UInt64
	- 32Bbit 아키텍처에서는 Int32가 Int타입이며, UInt32가 UInt타입으로 지정  
	- 64bit 아키텍처에서는 Int64가 Int타입이며, UInt64가 UInt타입으로 지정

진수별 정수 표현

```
let decialInteger: Int = 28
let binaryInteger: Int = 0b11100    // 2진수로 10진수 28을 표현
let octalInteger: Int = 0o34        // 8진수로 10진수 28을 표현
let hexadecimalInteger: Int = 0x1C  // 16진수로 10진수 28을 표현
```

## 3.2 Bool

불리언 타입은 참(true) 또는 거짓(false)만 값으로 가짐


## 3.3 Float과 Double

- 소수점을 표현하는 수
Float : 6자리 십진수까지 표현 가능  
Double : 최소 15자리 십진수 표현 가능  

## 3.4 Character

- 문자를 의미, 한단어

## 3.5 String

- 문자열을 의미 ,여러 단어

### 3.5.1 특수문자

```
\n : 문자열 바꾸기
\\ : 백슬래쉬
\" : 큰따옴표
\t : 탭
\o : null문자열
```

## 3.6 Any, AnyObject와 nil

- Any는 모든 데이터 타입을 사용할 수 있다  
- AnyObject는 클래스의 인스턴스만 할당할 수 있다  
- nil은 특정 타입이 아니라 '없음'을 나타내는 키워드, 다른 언어의 null 과 유사 