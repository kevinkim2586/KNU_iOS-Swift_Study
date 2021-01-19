# Chapter 23 : 프로토콜 지향 프로그래밍 (POP)
- 스위프트 표준라이브러리에서 타입과 관련된 것을 보면 대부분 구조체로 구현되어 있음.
- 보통 클래스, 상속 등을 활용하지 않음
- 상속도 되지 않는 구조체로 어떻게 공통 기능, 다양한 기능을 구현 가능한가?

     → 프로토콜, 익스텐션 등을 활용했기 때문

출처 :( [https://jinshine.github.io/2018/09/11/Swift/18.프로토콜 지향 프로그래밍(Protocol-Oriented Programming)/](https://jinshine.github.io/2018/09/11/Swift/18.%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C%20%EC%A7%80%ED%96%A5%20%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D(Protocol-Oriented%20Programming)/))

흔히 알고 있는 객체 지향 프로그래밍(Object-Oriented Programming)이라고 하면, 사물을 객체로 형성하여 공통점을 갖는 모든 곳에서 상속받는 객체 내부의 모든 로직을 캡슐화합니다. 의도 하지 않아도 상속했다는 이유로 모든 속성과 행위를 공유해야하며, 복잡한 상속 구조를 지닌 클래스를 상속했다면 원하는 클래스를 참조해야 할때 다운캐스팅을 해야 합니다. 또한 가장 큰 단점은 단 하나의 SuperClass만 상속할 수 있다는 점입니다. 시간이 흐르면 기능도 확장하기 마련이므로 이에 따라 복잡도가 높아지고 관리도 어려워지게 됩니다.

여기서 프로토콜 지향 프로그래밍이 등장하게 된 계기를 찾을 수 있습니다.

POP는 필요한 부분만 프로토콜로 분리해서 만들 수 있고 다중프로토콜을 구현할 수 있습니다.게다가 프로토콜 규칙을 class, struct, enum에 적용할 수 있기 때문에 확장 부분에서도 OOP보다 유연합니다.

스위프트는 다른 언어와 달리 클래스로 구현된 타입보다는 대부분 구조체로 기본 타입이 구현되어 있습니다. 상속도 되지 않는 구조체로 공통 기능을 가질 수 있는 방법에는 프로토콜과 익스텐션에 있습니다

```swift
protocol Walkable {
    var isBareFoot: Bool { get set }
    
    func walk()
}

struct Person: Walkable {
    var isBareFoot: Bool
    var name: String
    
    func walk() {
        print("\(name)은 걷습니다")
    }
}

struct Animal: Walkable {
    var isBareFoot: Bool
    
    func walk() {
        print("\(name)은 걷습니다")
    }
}
```

프로토콜을 채택시 아시다시피 프로토콜이 요구하는 사항을 모두 구현해주어야합니다.

예를 들어 프로토콜에 많은 요구 사항들이 들어있고, 많은 구조체에서 채택을 했다면 많은 중복된 코드를 사용해야 할 겁니다.

이를 방지하기 위해 프로토콜의 요구사항을 구현하지 않더라도 프로토콜의 익스텐션에 미리 프로토콜의 요구사항을 구현해 둘 수 있습니다.

```swift
protocol Walkable {
    var isBareFoot: Bool { get set }
    var speed: Double { get set }
    
    func walk(name: String)
}

extension Walkable {
    func walk(name: String) {
        if isBareFoot == true {
            print("\(name)은 맨발인 상태에 \(speed)속도로 걷습니다.")
        } else {
            print("\(name)은 신발인 상태에 \(speed)속도로 걷습니다.")
        }
    }
}

struct Person: Walkable {
    var isBareFoot: Bool
    var speed: Double
}

struct Animal: Walkable {
    var isBareFoot: Bool
    var speed: Double
}

let seungjin = Person(isBareFoot: false, speed: 5.0)
seungjin.walk(name: "승진")
let dog = Animal(isBareFoot: true, speed: 10.0)
dog.walk(name: "몽실")
```

→ 위의 코드에서 Person과 Animal은 Walkable의 요구사항인 walk(name:)을 구현하지 않았음에도 불구하고 오류가 발생하지 않습니다.

프로토콜이 요구하는 사항을 미리 구현해 둘 수 있다면 중복된 코드를 피할 수 있습니다. 구현을 잘 해둔다면 프로토콜 채택만으로도 그 타입의 기능이 추가되어 사용할 수가 있죠.
