# Chapter 4 : 데이터 타입 고급
### 타입 별칭

- 이미 존재하는 데이터 타입에 임의로 다른 이름(별칭) 부여 가능

```swift
typealias MyInt = Int
typealias YourInt = Int

let age : MyInt = 100
```

→ Int 와 MyInt 모두 같은 타입으로 취급

### 튜플 (Tuple)

- 지정된 데이터의 묶음

```swift
var person: (String, Int, Double) = ("yagom", 100, 190.2)

// 인덱스를 통해 값 접근 가능

person.1 = 99
person.2 = 1998.2

print("이름: \(person.0)")
```

- 튜플 요소 이름 지정 역시 가능

```swift
var person: (name: String, age: Int, height: Double) = ("yagom", 100, 190.2)

person.age = 99
person.height = 102.4
```

→ 이런 식으로 이름을 따로따로 모두 지정을 해주면 나중에 access 가능

### 컬렉션형  (배열, 딕셔너리, 집합)

Array → 순서가 있는 리스트 컬렉션

Dictionary → key 와 value 의 쌍으로 이루어진 컬렉션

Set → 순서 X, 멤버가 유일한 컬렉션, 중복 X

Array:

```swift
var integers: Array<Int> = Array<Int>()
var doubles: Array<Double> = [Double]()

var string: [String] = [String]()
var characters: [Character]= []

//위 2가지 모두 동일한 표현

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

- 키와 값의 쌍으로 구성
- 순서가 없다
- 키 중복 X, but 값은 중복 가능
- Key 값으로 접근

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

- 세트 내의 값은 모두 유일한 값 - 중복 값 X
- 순서가 중요하지 않거나 각 요소가 유일한 값이어야 하는 경우에 사용
- 배열과 달리 줄여서 표현할 수 있는 축약형 (ex. Array[Int] → [Int])이 없음

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

### 열거형

- 연관된 항목들을 묶어서 표현할 수 있는 타입
- 프로그래머가 정의해준 항목 값 외에는 추가/수정 불가

→ When to use?


1. 제한된 선택지를 주고 싶을 때
2. 정해진 값 외에는 입력받고 싶지 않을 때
3. 예상된 입력 값이 한정되어 있을 때

```swift
enum School{
	case primary
	case elementary
	case middle
	case high
	case university
	case graduate
   ....
}
```

- 각 항목은 그 자체가 **고유의 값**이다

```swift
var highestEducation : School = School.university

highestEducation = School.graduate
//graduate 과 university는 같은 School 타입이기 때문에 할당 가능 
```

→ 고유의 값을 가지게 할 수도 있고, 원시값을 가지게 할 수도 있음. 즉, 특정 타입으로 지정된 값 가지기 가능

```swift
//열거형 이름 우측에 타입 명시
enum School : String{
	case primary = "유치원"
	case elementary = "초등학교"
	case middle = "중학교"
}
```

### 연관값

- 열거형 내의 항목(case)이 자신과 연관된 값을 가질 수 있음
- 연관값은 각 항목 옆에 소괄호로 묶어 표현
- 꼭 다 가질 필요는 X

```swift
enum MainDish{
	case pasta(taste: String)
	case pizza(dough: String, topping: String)
	case chicken(withSauce: Bool)
	case rice
}

var dinner : Maindish = Maindish.pasta(taste: "크림")
dinner = .pizza(dough: "치즈크르스트", topping: "불고기")
dinner = .rice
```
