- String Interpolation
- \() 사용

    → C언어에서는 %d, %f 등 사용

ex)

```swift
let age: Int = 25

print("안녕하세요! 저는 \(age)살입니다")
print("안녕하세요! 저는 \(age+5)살입니다") 
```

상수 선언 키워드: let      

```swift
let 이름: 타입 = 값
//이름: 타입 -> 이 중간에 space가 있어야 함
```

변수 선언 키워드: var

```swift
var 이름: 타입 = 값
```

- 값의 타입이 명확하다면 타입은 생략 가능

```swift
let 이름 = 값
var 이름 = 값

ex)
let constantExample: String = "상수입니다"
var variableExample: String = "변수입니다"
```

상수 선언 후에 나중에 최초 값 할당 1번 가능

```swift
let sum: Inn
let inputA: Int = 100
let inputB: Int = 200

sum = inputA + inputB

sum = sum + 1 //다시 수정 불가 오류 발생
```

기본 데이터 타입

- Bool, Int, UInt, Float, Double, Character, String

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

```

- Any, AnyObject, nil

→ Any: Swift 의 모든 타입을 지칭하는 키워드

→ AnyObject: 모든 클래스 타입을 지칭하는 프로토콜

→ nil: 없음을 의미하는 키워드

```swift
var someAny: Any = 100
//어떠한 타입의 데이터 타입도 들어올 수 있음을 의미
someAny = "안녕"
someAny = 123.213
someAny = nil //오류; Any 는 어떠한 데이터타입도 올 수 있음을 의미하기는 하지만, 
무조건 하나는 와야 함

let someDouble: Double = someAny -> 이건 또 안 됨
```

```swift
class SomeClass{}

var someAnyObject:   AnyObject = SomeClass()
someAnyObject = 123.12 -> 오류
someAnyObject = nil //이것도 안 됨
```


컬렉션 타입 (배열, 딕셔너리, 집합)

Array → 순서가 있는 리스트 컬렉션

Dictionary → key 와 value 의 쌍으로 이루어진 컬렉션

Set → 순서 X, 멤버가 유일한 컬렉션, 중복 X

Array:

```swift
var integers: Array<Int> = Array<Int>()
var doubles: Array<Double> = [Double]()
//위 2가지 모두 동일한 표현
var string: [String] = [String]()
var characters: [Character]= []

//위도 동일
integers.append(1) //배열 값 추가

integers.contains(100)
//false
integers.remove(at: 0)
//0번 인덱스에 있는 값 없애기
integers.removeLast()
//마지막에 있는 값 삭제
integers.removeAll()

integers[0]

let immutableArray = [1,2,3]
//let 을 사용하여 배열을 선언하면 불변 array 가 된다
immutableArray.append(2) //불가능
```

Dictionary:

```swift
//key가 string 타입이고 value가 any 인 빈 딕셔너리 생성
var anyDictionary: Dictionary<String,Any> = [String:Any]()
//any 가 value라는 것은 어떤 타입이라도 들어와도 된다는 것

anyDictionary["someKey"] = "value"
anyDictionary["anotherKey"] = 100

anyDictionary.removeValue(forKey: "anotherKey")
anyDictionary["somekey"] = nil
//위 2줄은 거의 동일한 의미

let emptyDictionary: [String: String] = [:]
emptyDictionary["key"] = "value" //수정 불가

let initializedDictionary: [String: String] = ["name":"yagom"]
//위 딕셔너리도 수정 불가
```

Set:

```swift
var integerSet: Set<Int> = Set<Int>()
//Set 는 축약 문법이 없음. 이대로 길게 써야함

integerSet.insert(1)
integerSet.insert(100)
integerSet.insert(99)
integerSet.insert(99)   //중복 X

integerSet
-> {100,99,1} 이렇게 결과가 나옴

integerSet.contains(1)
//true

integerSet.remove(100)
-> remove 된 값을 return

integerSet.count
//1

let setA: Set<Int> = [1,2,3,4,5]
let setB: Set<Int> = [3,4,5,6,7]

let union: Set<Int> = setA.union(setB)
//{2,4,5,6,7,3,1} -> 순서 없음 및 중복 없음

let sortedUnion: [Int] = union.sorted()
//{1,2,3,4,5,6,7}

let intersection: Set<Int> = setA.intersection(setB)
//{5,3,4}

let subtracting: Set<Int> = setA.subtracting(setB)
//{2,1}

```
