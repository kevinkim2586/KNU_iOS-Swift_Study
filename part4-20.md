<h1>Chapter 20. 프로토콜</h1>

<h2>20.1 프로토콜이란</h2>

프로토콜 - 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진 즉, 프로토콜은 정의를 하고 제시를 할 뿐이지 스스로 기능을 구현하지는 않음.

<h2>20.2 프로토콜 채택</h2>

protocol 키워드를 사용함

```swift
protocol SomeProtocol {
    // 프로토콜 정의
}
```

<h2>20.3 프로토콜 요구사항</h2>

특정기능을 실행하기 위해 필요한 기능을 요구함

<h3>20.3.1 프로퍼티 요구</h3>

```swift
protocol SomeProtocol {
    var settableProperty: String { get set } // 읽기와 쓰기 모두 요구
    var notNeedToBeSettableproperty: String { get } // 읽기만 가능하다면 어떻게 구현되어도 상관x
}

protocol AnotherProtocol {
    static var someTypeProperty: Int { get set }
    static var anotherTypeProperty: Int { get }
}
```

<h3>20.3.2 메서드 요구</h3>

```swift
protocol SomeProtocol {
    static func someTypeMethod() // static키워드는 타입 메서드를 요구할때 사용
}

protocol RandomNumberGenerator { // 필수 메서드 지정시 함수명과 반환값을 지정할 수 있고, 구현에 사용하는 괄호는 적지 않아도 됨
    func random() -> Double
}

// 프로토콜 필수 메서드 random()을 구현한 클래스

class LinearCongruentialGenerator: RandomNumberGenerator {
    var lastRandom = 42.0
    let m = 139968.0
    let a = 3877.0
    let c = 29573.0
    func random() -> Double {
        lastRandom = ((lastRandom a + c).truncatingRemainder(dividingBy:m))
        return lastRandom / m
    }
}

let generator = LinearCongruentialGenerator()
print("Here's a random number: \(generator.random())")
// Prints "Here's a random number: 0.3746499199817101"
print("And another one: \(generator.random())")
// Prints "And another one: 0.729023776863283"
```

<h3>20.3.3 가변 메서드 요구</h3>

 mutating 키워드 사용
 
 ```swift
 protocol Togglable {
    mutating func toggle()
}
```

```swift
enum OnOffSwitch: Togglable {
    case off, on
    mutating func toggle() {
        switch self {
        case .off:
            self = .on
        case .on:
            self = .off
        }
    }
}
var lightSwitch = OnOffSwitch.off
lightSwitch.toggle()
// lightSwitch is now equal to .on
```

<h3>20.3.4 이니셜라이저 요구</h3>

```swift
protocol SomeProtocol {
    init(someParameter: Int)
}
```
클래스에서의 이니셜라이저 요구 구현
```swift
class SomeClass: SomeProtocol {
    required init(someParameter: Int) {
        // initializer implementation goes here
    }
}
```
상속받은 클래스의 이니셜라이저 요구 구현 및 재정의

```swift
protocol SomeProtocol {
    init()
}

class SomeSuperClass {
    init() {
        // initializer implementation goes here
    }
}

class SomeSubClass: SomeSuperClass, SomeProtocol {
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass
    required override init() {
        // initializer implementation goes here
    }
}
```

<h2>20.4 프로토콜의 상속과 클래스 전용 프로토콜</h2>

클래스의 상속문법과 유사하게 프로토콜도 상속할 수 있음

```swift
protocol Readable {
    func read()
}

protocol Writable {
    func write()
}

protocol ReadSpeakable: Readable {
    func speak()
}

class SomeClass: ReadWriteSpeakable {
    func read() {
        print("Read")
    }
    
    func write() {
        print("Write")
    }
    
    func speak() {
        print("Speak")
    }
}
```

<h2>20.5 프로토콜 조합과 프로토콜 준수 확인</h2>

프로토콜 조합 및 프로토콜, 클래스 조합

```swift
protocol Named {
    var name: String { get }
}

protocol Aged {
    var age: Int { get }
}

struct Person: Named, Aged {
    var name: String
    var age: Int
}

class Car: Named {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

class Truck: Car, Aged {
    var age: Int
    
    init(name: String, age: Int) {
        self.age = age
        super.init(name: name)
    }
}


var someVariable: Car & Aged
```

프로토콜을 준수하는지 확인하는 방법

* is 연산자를 통해 해당 인스턴스가 특정 프로토콜을 준수하는지 확인할 수 있습니다. 

* as? 다운캐스팅 연산자를 통해 다른 프로토콜로 다운캐스팅을 시도해 볼 수 있습니다.

* as! 다운캐스팅 연산자를 통해 다른 프로토콜로 강제 다운캐스팅을 할 수 있습니다.


<h2>20.6 프로토콜의 선택적 요구</h2>

@objc키워드를 프로토콜 앞에 붙이고, 개별 함수 혹은 프로퍼티에는 @objc와 optional 키워드를 붙인다. @objc 프로토콜은 클래스 타입에서만 채용될 수 있고 구조체나 열거형에서는 사용할 수 없다. 

```swift
@objc protocol CounterDataSource {
    @objc optional func increment(forCount count: Int) -> Int
    @objc optional var fixedIncrement: Int { get }
}
```

<h2>20.7 프로토콜 변수와 상수</h2>

프로토콜 이름을 타입으로 갖는 변수 또는 상수에는 그 프로토콜을 준수하는 타입의 어떤 인스터스라도 할당할 수 있다

```swift
var someNamed: Named = Animal(name: "Animal")
someNamed = Pet(name: "Pet")
someNamed = Person(name: "Person")
someNamed = School(name: "School")!
```

<h2>20.8 위임을 위한 프로토콜</h2>

위임 - 클래스나 구조체가 자신의 책임이나 임무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴

