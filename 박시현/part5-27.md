<h1>Chapter 27. ARC</h1>

ARC는 클래스의 인스턴스에서만 적용됨 왜냐하면 참조 타입이기 때문 즉, 열거형이나 구조체는 값 타입이므로 ARC로 메모리를 관리할 필요 없음

<h2>27.1 ARC란</h2>

ARC - 더이상 필요하지 않은 클래스의 인스턴스를 메모리에서 해제하는 방식으로 동작함

<h2>27.2 강한참조</h2>

강한참조 - 참조의 기본

```swift
class Person {
    let name: String
    
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "yagom")
// yagom is being initialized
// 인스턴스의 참조 횟수 : 1

reference2 = reference1 // 인스턴스의 참조 횟수 : 2
reference3 = reference1 // 인스턴스의 참조 횟수 : 3

reference3 = nil // 인스턴스의 참조 횟수 : 2
reference2 = nil // 인스턴스의 참조 횟수 : 1
reference1 = nil // 인스턴스의 참조 횟수 : 0
// yagom is being deinitialized
```
<h2>27.3 약한참조</h2>

<h2>27.4 미소유 참조</h2>

<h2>27.5 미소유 옵셔널 참조</h2>

<h2>27.6 미소유참조와 암시적 추출 옵셔널 프로퍼티</h2>

<h2>27.7 클로저의 강한참조 순환</h2>

