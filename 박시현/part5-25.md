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

<h2>25.6 옵셔널 패턴</h2>

<h2>25.7 타입캐스팅 패턴</h2>

<h2>25.8 표현 패턴</h2>

