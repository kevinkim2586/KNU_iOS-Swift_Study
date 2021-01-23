<h1>Chapter 23. 프로토콜 지향 프로그래밍</h1>

객체 지향과 프로토콜 지향의 차이

(ex) 콜라(캔)와 오렌지주스(플라스틱병) 객체가 각각 있다고 할 때

- Polymorphism 적용 X:
   if 콜라캔 { 캔을 손으로 따서 열기() }
   else if 오렌지주스 { 뚜껑을 오른쪽 방향으로 돌려서 열기() }

- Polymorphism 적용 O:
  콜라캔과 오렌지주스병을 둘 다 음료수객체로 정의,
  Open 동작 구현을 각각 해줌.
  if문 대신 음료수객체.Open() 실행

- 이때 와인병(코르크마개)이 추가된다면? 우유팩(손으로 잡아 뜯기)이 추가된다면? 전자의 경우에는 Open 동작이 필요한 부분마다 if 문을 줄줄이 추가해야 할 것이다. 반면 후자의 경우에는 추가할 모델을 똑같이 음료수객체로 정의한 후, 각각의 특징에 따라 Open 함수의 구현만 달리해주면 된다.

- 상호작용이 Open 만이 아니라 Drink, Recycle 등 더 많이 늘어난다면? 전자로 작성된 코드는 수많은 if문의 향연이 될 것이다.


출처: https://wlaxhrl.tistory.com/77 [찜토끼의 Swift 블로그]

<h2>23.1 프로토콜 초기구현</h2>

```swift
protocol Person {
    var name: String { get }
    var age: Int { get }
    
    func getAge() -> Int
}

struct FireFighter: Person {
    var name: String
    var age: Int
    
    // 중복 구현
    func getAge() -> Int {
        return age
    }
    
    func extinguish() {
    }
}

struct PolieMan: Person {
    var name: String
    var age: Int
    var jail: [Criminal] = []
    
    // 중복 구현
    func getAge() -> Int {
        return age
    }
    
    mutating func arrest(criminal: Criminal) {
        jail.append(criminal)
    }
}
```


<h2>23.2 맵, 필터, 리듀스 직접 구현해보기</h2>

```swift
array 타입의 맵 사용
let items: Array<Int> = [1, 2, 3]

let mappedItems: Array<Int> = items.map { (item: Int) -> Int in
    return item * 10
}

print(mappedItems) // [10, 20, 30]
```

```swift
array 타입의 필터사용

let filteredItmes: Array<Int> = items.filter { (item: Int) -> Bool in
    return item % 2 == 0
}

print(filteredItems) // [2]
```

```swift
array 타입의 리듀스 사용

let combinedItems: Int = items.reduce(0) { (result: Int, next: Int) -> Int in
    return result + next
}

print(combinedItmes) // 6

let combinedItemsDoubled: Double = items.reduce(0.0) { (result: Double, next: Int) -> Double in
    return result + Double(next)
}

print(combinedItemsDoubled) // 6.0

let combinedItemsString: String = items.reduce(" ") { (result: String, item: Int) -> String in
    return result + "\(next)"
}

print(combinedItemsString) // "1 2 3"
```

<h2>23.3 기본 타입 확장</h2>

프로토콜 초기구현을 통해 스위프트의 기본 타입을 확장하여 내가 원하는 기능을 추가해볼 수 있음
