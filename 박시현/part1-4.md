<h1>Chapter 4. 데이터 타입 고급</h1>

<h3>4.1.2 타입추론</h3>

* 타입추론

```swift
let name = "kwanhee" 라고 타입을 지정을 안해줘도 컴파일러가 컴파일하면서 알아서 name을 String으로 지정해준다
타입별칭 -
typealias MyInt = Int // Int의 또다른 이름이 MyInt이다
let age: MyInt 이렇게 써줘도 이상없다
```
<h2>4.3 튜플</h2>

* 튜플 
- 지정된 데이터의 묶음

String, Int, Double 타입을 갖는 튜플
```swift
var person: (String, Int, Double) = ("yagom", 100, 182.5)
```

// 인덱스를  통해서 값을 빼 올 수 있다
```swift
print("이름: \(person.0), 나이: \(person.1), 신장: \(person.2)")
```

// 인덱스를 통해 값 할당도 가능

```swift
person.1 = 99
person.2 = 178.5
```
```swift
print("이름: \(person.0), 나이: \(person.1), 신장: \(person.2)")
```
<h2>4.4 컬렉션형</h2>

* 컬렉션형 
- 튜플 외에도 데이터를 묶어서 저장하고 관리할 수 있음

<h3>4.4.1 배열 (Array)</h3>

* 배열

```swift
var names: Array<String> = ["yagom", "chulsoo", "younghee", "yagom"]
```

[String]은 Array<String>의 축약 표현

위 표현과 정확히 같은 표현
```swift
var names: [String] = ["yagom", "chulsoo", "younghee", "yagom"]
```

```swift
var emptyArray: [Any] = [Any]()
```
위 표현과 정확히 같은 표현
```swift
var emptyArray: [Any] = Array<Any>()
```

배열의 타입을 정확히 명시해줬다면 []만으로도 빈 배열 생성 가능

```swift
var emptyArray: [Any] = []
print(emptyArray.isEmpty) //true
print(names.count) // 4
```

배열 사용법
```swift
print(names[2]) //younghee
name[2] = "jenny" // younghee -> jenny
print(names[2]) // jenny
print(names[4]) // 인덱스 범위를 벗어났기 때문에 오류발생

names.append("elsa") // 마지막에 elsa 추가
names.append(contentsOf: ["john", "max"]) //맨 마지막에 john과 max가 추가됌

print(names[4]) // yagom 인덱스 4에 해당하는 값
print(names.firstIndex(of: "yagom")) // 0 yagom의 인덱스
print(names.firstIndex(of: "christal")) // nil
print(names.first) // yagom
print(names.last) // max

let firstItem: String = names.removeFirst()
let lastItem: String = namas.removeLast()
let indexZeroItem: String = names.remove(at: 0)

print(firstItem) // yagom
print(lastItem) // max
print(indexZeroItem) // chulsoo
print(names[1...3]) // ["jenny", "yagom", "jinhee"]
```

<h3>4.4.2 딕셔너리</h3>

* 딕셔너리(Dictionary)

- 키와 값을 가짐

```swift
var numberForName: Dictionary<String, Int> = Dictionary<String, Int>()
```
위 표현과 정확히 같은 표현 // Dictionary<String, Int> = [String: Int]
```swift
var numberForName: [String: Int] =  [String: Int]()
```
위 표현과 정확히 같은 표현
```swift
var numberForName: StringIntDictionary = StringIntDictionary()
```

딕셔너리의 사용

```swift
var numberForName: [String: Int] = ["yagom": 100, "chulsoo": 200, "jenny": 300] // 초기값 주기
print(numberForName["chulsoo"]) // 200
print(numberForName["minji"]) // nil - 딕셔너리 내부에 없는 키로 접근해도 오류는 안나지만 nil을 반환한다
```

```swift
numberForName["max"] = 999 // max라는 키로 999라는 값을 추가
print(numberForName["max"]) // 999
print(numberForName.removeValue(forkey: "yagom")) // yagom에 해당하는 값을 제거해주기 때문에 nil이 반환됌
print(numberForName["yagom", default: 0]) // 0 yagom에 해당하는 값이 없기때문에 기본값 0이 반환됌
```
<h3>4.4.3 세트</h3>

* 세트(Set) - 데이터를 순서 없이 하나의 묶음으로 저장

-순서가 중요하지 않거나 각 요소가 유일한 값일때 사용

```swift
var names: Set<String> = Set<String>() // 빈 세트 생성
var names: Set<String> = [] // 빈 세트 생성

var names: Set<String> = ["yagom", "chulsoo", "younghee", "yagom"] // Array와 마찬가지로 대괄호 사용
```

세트의 사용
```swift
print(names.count) // 3 yagom이 중복되므로 한개만 세준다
names.insert("jenny") // jenny 삽입
print(names.count) // jenny가 삽입되었으므로 4
print(names.remove("john")) // nil john이라는 데이터가 존재하지 않음
```

<h2>4.5 열거형</h2>

* 열거형 

 - 연관된 항목들을 묶어서 표현할 수 있는 타입

(1)제한된 선택지를 주고 싶을때
(2)정해진 값 외에는 입력받고 싶지 않을때
(3)예상된 입력 값이 한정되어 있을때

열거형 선언
```swift
enum School {
    case primary // 유치원
    case elementary // 초등
    case middle // 중등
    case high // 고등
    case college // 대학
    case university // 대학교
    case graduate // 대학원
    //모두 한줄로 나열가능
}
```

열거형 변수 생성
```swift
var highestEducationLevel: School = School.university
```
위 표현과 정확히 같은 표현
```swift
var highestEducationLevel: School = .university
```

열거형의 응용
```swift
enum PastaTaste {
    case cream, tomato
}
enum PizzaDough {
    case cheeseCrust, thin, original
}
enum PizzaTopping {
    case pepperoni, cheese, bacon
}
enum MainDish {
    case pasta(taste: PastaTaste)
    case pizza(dough: PizzaDough, topping: PizzaTopping)
    case chicken(withSauce: Bool)
    case rice
}

var dinner: MainDish = MainDish.pasta(taste: PastaTaste.tomato)
dinner = MainDish.pizza(dough: PizzaDough.cheeseCrust, topping: PizzaTopping.bacon)
```

비교가능한 열거형
```swift
enum condition: Comparable {
    case terrible
    case bad
    case good
    case great
}

let myCondition: Condition = Condition.great
let yourCondition: Condition = Condition.bad

if myCondition >= yourCondition {
    print("제 상태가 더 좋군요")
}
else {
    print("당신의 상태가 더 좋아요")
}
```
