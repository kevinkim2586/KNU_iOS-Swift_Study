# Chapter20 프로토콜

## 20.1 프로토콜이란

- \*프로토콜은 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항등의 청사진\*을 정의한다.
- \*프로토콜은 정의를 하고 제시를 할 뿐이지 스스로 기능을 구현하지는 않음\*

## 20.2 프로토콜 채택

- protocol 키워드를 사용

```swift
protocol 프로토콜 이름{
	프로토콜 정의
}
```
- 타입 프로토콜의 채택

```swift
struct Somestruct: Aprotocol, AnotherProtocol{
	//구조체 정의
}

class SomeClass: AProtocol, AnotherProtocol{
	//클래스 정의
}

enum SomeEnum: Aprotocol,AnotherProtocol{
	//열거형 정의
}
```

- 만약 클래스가 다른 클래스를 상속받는다면 클래스 이름 다음에 채택할 프로토콜을 나열

```swift
class SomeClass: SuperClass, AProtocol, AnotherProtocol{
	//클래스 정의
}
```

## 20.3 프로토콜 요구사항

### 20.3.1 프로퍼티 요구

- 프로토콜은 자신을 채택한 타입이 어떤 프로퍼티를 구현해야하는지 요구 가능
- 프로퍼티의 종류는 신경을 안쓰고 이름과 타입만 신경을씀
- 프로토콜의 프로퍼티 요구사항은 항상 var 키워드를 사용한 변수 프로퍼티로 정의

```swift
protocol Sendable {
    var from: String { get }
    var to: String { get }
}

class Message: Sendable {
    
    var sender: String
    var from: String {
        return self.sender
    }
    
    var to: String
    
    init(sender: String, receiver: String) {
        self.sender = sender
        self.to = receiver
    }
}

class Mail: Sendable {
    
    var from: String
    var to: String
    
    init(sender: String, receiver: String) {
        self.from = sender
        self.to = receiver
    }
}
```

### 20.3.2 메서드 요구

- 메서드의 실제 구현부인 중괄호를 제외하고 메서드의 이름, 매개변수, 반환 타입 등만 작성하면 가변 매개변수도 허용
- 프로토콜의 메서드 요구에서는 매개변수 기본값을 지정할 수 없음
- 타입 메서드를 요구 할때는 static 키워드를 명시

```swift
protocol Receiveable {
    func received(data: Any, from: Sendable)
}

// 무언가를 발신할 수 있는 기능
protocol Sendable {
    var from: Sendable { get }
    var to: Receiveable? { get }
    
    func send(data: Any)
    
    static func isSendableInstance(_ instance: Any) -> Bool
}
```

### 20.3.3 가변 메서드 요구

- 프로토콜이 어떤 타입이든 간에 인스턴스의 내부의 값을 변경해야하는 메서드를 요구하려면 프로토콜의 메서드 정의 앞에 mutating이라는 키워드를 명시

### 20.3.4 이니설라이즈 요구

- 정의는 하지만 구현은 x

```swift
protocol Named {
    var name: String { get }
    
    init(name: String)
}

struct Pet: Named {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}
```

## 20.4 프로토콜의 상속과 클래스 전용 프로토콜

- 프로토콜은 하나 이상의 프로토콜을 상속받아 기존 프로토콜의 요구사항보다 더 많은 요구사항을 추가가능

```swift
protocol Readable {
    func read()
}

protocol Writeable {
    func write()
}

protocol ReadSpeakable: Readable {
    func speak()
}

protocol ReadWriteSpeakable: Readable, Writeable {
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

## 20.5 프로토콜 조합과 프로토콜 준수 확인

- 하나의 매개변수가 여러 프로토콜을 모두 준수하는 타입이어야 한다면 하나의 매개변수에 여러 프로토콜을 한 번에 조합하여 요구 가능.

```swift
print(yagom is Named)   // true
print(yagom is Aged)    // true

print(myCar is Named)   // true
print(myCar is Aged)    // false


if let castedInstance: Named = yagom as? Named {
    print("\(castedInstance) is Named")
}   // Person is Named

if let castedInstance: Aged = yagom as? Aged {
    print("\(castedInstance) is Aged")
}   // Person is Aged

if let castedInstance: Named = myCar as? Named {
    print("\(castedInstance) is Named")
}   // Car is Named

if let castedInstance: Aged = myCar as? Aged {
    print("\(castedInstance) is Aged")
}   // 출력 없음... 캐스팅 실패

```

## 20.6 프로토콜의 선택적 요구

- 프로토콜의 요구사항 중 일ㄹ부를 선택적 요구사항으로 지정할수 있음
- 선택적 요구사항을 정의하고 싶은 프로토콜은 objc 속성이 부여된 프로토콜이여야함

## 20.7 프로토콜 변수와 상수

- 프로토콜 이름을 타입으로 갖는 변수 또는 상수에는 그 프로토콜을 준수하는 타입의 어떤 인스턴스라도 할당할수 있다.

```swift
@objc protocol Moveable {
    func walk()
    @objc optional func fly()
}

// 걷기만 할 수 있는 호랑이
class Tiger: NSObject, Moveable {
    func walk() {
        print("Tiger walks")
    }
}

// 걷고 날 수 있는 새
class Bird: NSObject, Moveable {
    func walk() {
        print("Bird walks")
    }
    func fly() {
        print("Bird flys")
    }
}

let tiger: Tiger = Tiger()
let bird: Bird = Bird()

tiger.walk()    // Tiger walks

bird.walk()     // Bird walks
bird.fly()      // Bird flys

var movableInstance: Moveable = tiger
movableInstance.fly?()  // 응답 없음

movableInstance = bird
movableInstance.fly?()  // Bird flys
```

## 20.8 위임을 위한 프로토콜

\*위임\*

	- 클래스나 구조체가 자신의 책임이나 임무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴 

	- 위임은 사용자의 특정 행동에 반응하기 위해 사용되기도 한다.
