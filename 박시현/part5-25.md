<h1>Chapter 25. 패턴</h1>

패턴 - 단독 또는 복합 값의 구조를 나타내느 것, 패턴 매칭 - 코드에서 어떤 패턴의 형태를 찾아내는 행위

대부분의 패턴은 switch, if, guard, for 등의 키워드와 아주 친하며 두 개 이상의 키워드가 합을 이뤄 동작함

<h2>25.1 와일드카드 패턴</h2>

와일드카드 식별자( _ )를 사용하는 것은 이 자리에 올 것이 무엇이든간에 상관하지 마라 는 뜻임 . 즉, 와일드카드 식별자가 위치한 곳의 값은 무시함

```swift
let string: String = "ABC"

switch string {
// ABC -> 어떤 값이 와도 상관없기에 항상 실행됨
case _ : print(string)
}

let optionalString: String? = "ABC"

switch optionalString {
// optionalString이 Optional("ABC")일 때만 실행됨
case "ABC"?: print(optionalString)

// optionalString이 Optional("ABC") 외의 값이 있을 때만 실행됨
case _ ?: print("Has value, but not ABC")

// 값이 없을 때 실행됨
case nil: print("nil")
] // Optional("ABC")

let yagom = ("yagom", 99, "Male")

switch yagom {
// 첫 번째 요소가 "yagom"일 때만 실행됨
case ("yagom", _ , _ ): print("Hello yagom!!")

// 그 외 언제든지 실행됩니다.
case( _, _, _ ): print("Who cares~")
} // Hello yagom!!

for _ in 0..<2 {
    print("Hello")
}
// Hello
// Hello
```

<h2>25.2 식별자 패턴</h2>

```swift
someValue의 타입인 Int와 할당하려는 42의 타입이 매치된다면 someValue는 42라는 값의 식별자

le someValue: Int = 42
```

<h2>25.3 값 바인딩 패턴</h2>

값 바인딩 패턴 - 변수 또는 상수의 이름에 매치된 값을 바인딩 함, switch 구문에 자주사용

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

<h2>25.4 튜플 패턴</h2>

튜플 패턴 - 소괄호 () 내에 쉼표로 분리하는 리스트

```swift
let (a): Int = 2
print(a) // 2

let (x, y): (Int, Int) = (1, 2)
print(x) // 1
print(y) // 2

let name: String = "Jung"
let age: Int = 99
let gender: String? = "Male"

switch(name, age, gender) {
case("Jung", _, _): print("Hello Jung!!")
case(_, _, "Male"?): print("Who are you man?")
default: print("I don't know who you are")
} // Hello Jung!!

let points: [(Int, Int)] = [(0, 0), (1, 0), (1, 1), (2, 0), (2, 1)]

for(x, _) in points {
    print(x)
}
// 0
// 1
// 1
// 2
// 2
```

<h2>25.5 열거형 케이스 패턴</h2>

열거형 케이스 패턴 - 값을 열거형 타입의 case와 매치시킴

```swift
let someValue: Int = 30

if case0...100 = someValue {
   print("0 <= \(someValue) <= 100")
} // 0 <= 30 <= 100

enum MainDish {
    case pasta(taste: String)
    case pizza(dough: String, topping: String)
    case chicken(withSauce: Bool)
    case rice
}

var dishes: [MainDish] = []

var dinner: MainDish = .pasta(taste: "크림") // 크림 파스타
dishes.append(dinner)

dinner = .pizza(dough: "치즈크러스트", topping: "불고기") // 치즈크러스트 불고기 피자 만들기
dishes.append(dinner)

func whatIsThis(dish: MainDish) {
    guard case .pizza(let dough, let topping) = dinner else {
        print("It's not a Pizza")
        return
    }
    
    print("\(dough) \(topping) 피자")
}
whatIsThis(dish: dinner) // 치즈크러스트 불고기 피자

dinner = .chicken(withSauce: true) // 양념 통닭 만들기
dishes.append(dinner)

while case .chicken(let sauced) = dinner {
    print("\(sauced ? "양념" : "후라이드") 통닭")
    break
} // 양념 통닭

dinner = .rice // 밥
dishes.append(dinner)

if case .rice = dinner {
    print("오늘 저녁은 밥입니다.")
} // 오늘 저녁은 밥입니다.

for dish in dishes {
    switch dish {
    case let .pasta(taste): print(taste)
    case let .pizza(dough, topping): print(dough, topping)
    case let .chicken(sauced): print(sauced ? "양념" : "후라이드")
    case let .rice: print("Just 쌀")
    }
}
/*
크림
치즈크러스트 불고기
양념
Just 쌀
*/
```


<h2>25.6 옵셔널 패턴</h2>

옵셔널 패턴 - 식별자 패턴 뒤에 물음표를 넣어 표기하며 열거형 케이스 패턴과 동일한 위치에 자리함

```swift
var optionalValue: Int? = 100

if case .some(let value) = optionalvalue {
    print(value)
} // 100

if case let value? = optionalValue {
    print(value)
} // 100

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
} // 100

print(optionalValue) // nil

let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]

for case let number? in arrayOfOptionalInts {
    print("Found a \(number)")
}
//Found a 2
//Found a 3
//Found a 5
```

<h2>25.7 타입캐스팅 패턴</h2>

is 패턴은 Switch case 레이블에서만 사용가능, is (TYPE_NAME)과 같이 쓸 수 있음

as 패턴은 SomePattern as (TYPE_NAME)과 같이 쓸 수 있음

```swift
let someValue: Any = 100

switch someValue {
// 타입이 Int인지 확인하지만 캐스팅된 값을 사용할 수는 없습니다.
case is String: print("It's String!")

// 타입 확인과 동시에 캐스팅까지 완료되어 value에 저장됩니다.
// 값 바인딩 패턴과 결합된 모습입니다.
case let value as Int: print(value + 1)
default: print("Int도 String도 아닙니다.")
} // 101
```
<h2>25.8 표현 패턴</h2>

표현 패턴 - switch 구문의 case 레이블에서만 사용 가능 ~= 연산자의 연산 결과가 true를 반환하면 매치시킴

```swift
switch 3 {
case 0...5: print("0과 5
표현 패턴 - switch 구문의 case 레이블에서만 사용 가능 ~= 연산자의 연산 결과가 true를 반환하면 매치시킴 사이"
표현 패턴 - switch 구문의 case 레이블에서만 사용 가능 ~= 연산자의 연산 결과가 true를 반환하면 매치시킴)
