# Chapter 25 : 패턴
- 대부분의 패턴은 switch, if, guard, for 등의 키워드로 이용하여 표현

스위프트에서는 크게 2가지 패턴:

1. 값을 추출하거나 무시하는 패턴

    → 와일드카드 패턴, 식별자 패턴, 값 바인딩 패턴, 튜플 패턴

2. 패턴 매칭을 위한 패턴

    → 열거형 케이스 패턴, 옵셔널 패턴, 표현 패턴, 타입캐스팅 패턴

### Wildcard Pattern

( _ ) 사용 → "이 자리에 올 것이 무엇이든간에 상관하지 마라"는 뜻. 즉, 와일드카드 식별자가 위치한 곳의 값은 무시한다.

```swift
// 코드 25-1 와일드카드 패턴의 사용
let string: String = "ABC"

switch string {
case _: print(string)   // ABC -> 어떤 값이 와도 상관없기에 항상 실행됩니다.
}

let optionalString: String? = "ABC"

switch optionalString {
case "ABC"?: print(optionalString)  // optionalString이 Optional("ABC")일 때만 실행됩니다.
case _?: print("Has value, but not ABC") // optionalString이 Optional("ABC") 외의 값이 있을 때만 실행됩니다.
case nil: print("nil")  // 값이 없을 때 실행됩니다.
}  // Optional(“ABC”)

let yagom = ("yagom", 99, "Male")

switch yagom {
case ("yagom", _, _): print("Hello yagom!!!")   // 첫 번째 요소가 "yagom"일 때만 실행됩니다.
case (_, _, _): print("Who cares~") // 그 외 언제든지 실행됩니다.
}  // Hello yagom!!!

for _ in 0..<2 {
    print("Hello")
}
// Hello
// Hello
```

### Value-Binding Pattern

변수 또는 상수의 이름에 매치된 값을 바인딩하는 것

```swift
let yagom = ("yagom", 99, "Male")

switch yagom {
// name, age, gender를 yagom의 각각의 요소와 바인딩합니다.
case let (name, age, gender) : print ("Name: \(name), Age: \(age), Gender: \(gender)")
} // Name: yagom, Age: 99, Gender: Male

switch yagom {
case (let name, let age, let gender) : print ("Name: \(name), Age: \(age), Gender: \(gender)")
} // Name: yagom, Age: 99, Gender: Male

switch yagom {
// 값 바인딩 패턴은 와일드카드 패턴과 결합하여 유용하게 사용될 수도 있습니다.
case (let name, _, let gender): print ("Name: \(name), Gender: \(gender)")
} // Name: yagom, Gender: Male
```

### Tuple Pattern

소괄호 내에 쉼표로 분리하는 리스트

→ 튜플 패턴은 그에 상응하는 튜플 타입과 값을 매치한다

```swift
let (a): Int = 2
print(a)    // 2

let (x, y): (Int, Int) = (1, 2)
print(x)    // 1
print(y)    // 2

let name: String = "Jung"
let age: Int = 99
let gender: String? = "Male"

switch (name, age, gender) {
case ("Jung", _, _): print("Hello Jung!!")
case (_, _, "Male"?): print("Who are you man?")
default: print("I don't know who you are")
}    // Hello Jung!!

let points: [(Int, Int)] = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]

for (x, _) in points {
    print(x)
}
// 0
// 1
// 1
// 2
// 2
```

### Enumeration Case Pattern

열거형 타입의 case 와 매치시킨다.

```swift
let someValue: Int = 30

if case 0...100 = someValue {
    print("0 <= \(someValue) <= 100")
}   // 0 <= 30 <= 100

let anotherValue: String = "ABC"

if case "ABC" = anotherValue {
    print(anotherValue)
}   // ABC

enum MainDish {
    case pasta(taste: String)
    case pizza(dough: String, topping: String)
    case chicken(withSauce: Bool)
    case rice
}

var dishes: [MainDish] = []

var dinner: MainDish = .pasta(taste: "크림")  // 크림 파스타
dishes.append(dinner)

if case .pasta(let taste) = dinner {
    print("\(taste) 파스타")
}   // 크림 파스타

dinner = .pizza(dough: "치즈크러스트", topping: "불고기")  // 치즈크러스트 불고기 피자 만들기
dishes.append(dinner)

func whatIsThis(dish: MainDish) {
    guard case .pizza(let dough, let topping) = dinner else {
        print("It's not a Pizza")
        return
    }
    
    print("\(dough) \(topping) 피자")
}
whatIsThis(dish: dinner) // 치즈크러스트 불고기 피자

dinner = .chicken(withSauce: true)  // 양념 통닭 만들기
dishes.append(dinner)

while case .chicken(let sauced) = dinner {
    print("\(sauced ? "양념" : "후라이드") 통닭")
    break
}   // 양념 통닭

dinner = .rice  // 밥
dishes.append(dinner)

if case .rice = dinner {
    print("오늘 저녁은 밥입니다.")
}   // 오늘 저녁은 밥입니다.

for dish in dishes {
    
    switch dish {
    case let .pasta(taste): print(taste)
    case let .pizza(dough, topping): print(dough, topping)
    case let .chicken(sauced): print(sauced ? "양념" : "후라이드")
    case .rice: print("Just 쌀")
    }
}
/*
 크림
 치즈크러스트 불고기
 양념
 Just 쌀
 */
```

### Optional Pattern

옵셔널 또는 암시적 추출 옵셔널 열거형에 감싸져 있는 값을 매치시킬 때 사용

- for-in 구문에서 nil 이 아닌 값을 찾는 데도 유용하게 사용

```swift
var optionalValue: Int? = 100

if case .some(let value) = optionalValue {
    print(value)
}   // 100

if case let value? = optionalValue {
    print(value)
}   // 100

func isItHasValue(_ optionalValue: Int?) {
    guard case .some(let value) = optionalValue else {
        print("none")
        return
    }
    
    print(value)
}
isItHasValue(optionalValue) // 100

while case .some(let value) = optionalValue {
    print(value)
    optionalValue = nil
}   // 100

print(optionalValue)    // nil

let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]

for case let number? in arrayOfOptionalInts {
    print("Found a \(number)")
}
// Found a 2
// Found a 3
// Found a 5
```

### Type-Casting Pattern

- is 패턴 & as 패턴
- 타입캐스팅을 하거나 타입을 매치시킨다

```swift
let someValue: Any = 100

switch someValue {
// 타입이 Int인지 확인하지만 캐스팅된 값을 사용할 수는 없습니다.
case is String: print ("It's String!")
    
// 타입 확인과 동시에 캐스팅까지 완료되어 value에 저장됩니다.
// 값 바인딩 패턴과 결합된 모습입니다.
case let value as Int: print(value + 1)
default: print("Int도 String도 아닙니다.")
}   // 101
```

### Expression Pattern

- 표현식의 값을 평가한 결과를 이용하는 것

```swift
switch 3 {
case 0...5: print("0과 5 사이")
default: print("0보다 작거나 5보다 큽니다.")
}   // 0과 5 사이

var point: (Int, Int) = (1, 2)

// 같은 타입 간의 비교이므로 == 연산자를 사용하여 비교할 것입니다.
switch point {
case (0, 0): print("원점")
case (-2...2, -2...2): print("(\(point.0), \(point.1))은 원점과 가깝습니다.")
default: print("point (\(point.0), \(point.1))")
}   // (1, 2)는 원점과 가깝습니다.

// String 타입과 Int 타입이 매치될 수 있도록 ~= 연산자를 정의합니다.
func ~= (pattern: String, value: Int) -> Bool {
    return pattern == "\(value)"
}

point = (0, 0)

// 새로 정의된 ~= 연산자를 사용하여 비교합니다.
switch point {
case ("0", "0"): print("원점")
default: print("point (\(point.0), \(point.1))")
}
// 원점

struct Person {
    var name: String
    var age: Int
}

let lingo: Person = Person(name: "Lingo", age: 99)

func ~= (pattern: String, value: Person) -> Bool {
    return pattern == value.name
}

func ~= (pattern: Person, value: Person) -> Bool {
    return pattern.name == value.name && pattern.age == value.age
}

switch lingo {
case Person(name: "Lingo", age: 99): print("Same Person!!")
case "Lingo": print("Hello Lingo!!")
default: print("I don't know who you are")
}   // Same Person!!
```
