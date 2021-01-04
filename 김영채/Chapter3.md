# Chapter 3 : 데이터 타입 기본

- 스위프트의 기본 데이터 타입이 모두 ***구조체*** 를 기반으로 구현되어 있음.
- Bool, Int, UInt, Float, Double, Character, String
→ Swift 의 모든 데이터 타입 이름은 첫 글자가 대문자로 시작하는 ***대문자 카멜케이스*** 를 사용

```swift
var someBool : Bool = true
someBool = false
//변수니까 나중에 수정 가능

someBool = 1 -> 안 됨 (1은 true 라 될 것 같지만 안 됨)

var someInt: Int = -100
var someUInt: UInt = 100
//unsigned int (양의 정수만 가능)

var someFloat: Float = 3.14
var someDouble: Double = 3.14

var someCharacter: Character = "*"

var someString: String = "하하하"
someString = someString + "웃으면 행복해요" 

let 한글변수이름: Character = "ㄱ"
//한글도 유니코드 문자에 속하므로 가능 
```

- Any, AnyObject, nil

→ Any: Swift 의 모든 타입을 지칭하는 키워드

→ AnyObject: 모든 클래스 타입을 지칭하는 프로토콜 / Any 보다는 조금 한정된 의미로 클래스의 인스턴스만 할당 가능하다

→ nil: 없음을 의미하는 키워드

```swift
var someAny: Any = 100
//어떠한 타입의 데이터 타입도 들어올 수 있음을 의미
someAny = "안녕"
someAny = 123.213
someAny = nil //오류; Any 는 어떠한 데이터타입도 올 수 있음을 의미하기는 하지만, 
							//무조건 하나는 와야 함

let someDouble: Double = someAny //-> 이건 또 안 됨
```

```swift
class SomeClass{}

var someAnyObject:   AnyObject = SomeClass()
someAnyObject = 123.12 -> 오류
someAnyObject = nil //이것도 안 됨
```
