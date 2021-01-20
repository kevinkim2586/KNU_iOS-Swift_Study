# Chapter20 프로토콜

## 20.1 프로토콜이란

> 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진  
> **정의를 하고 제시를 할 뿐 스스로 기능을 구현하지는 않는다**

## 20.2 프로토콜 채택

~~~ swift
protocol 프로토콜_이름 {
    프로토콜_정의
}

struct SomeStruct: Aprotocol, AnotherProtocol {
    // 구조체 정의
}

class SomeClass: SuperClass, Aprotocol, AnotherProtocol {
    // 클래스 정의
}
~~~

## 20.3 프로토콜 요구사항

### 20.3.1 프로퍼티 요구

> 프로토콜은 자신을 채택한 타입이 어떤 프로퍼티를 구현해야 하는지 요구할 수 있다  
> 프로토콜을 채택한 타입은 프로토콜이 요구하는 프로퍼티의 이름과 타입만 맞도록 구현해주면 된다  

> { get }: 읽기만 가능하다면 어떻게 구현되어도 상관없음(상수 저장 프로퍼티, 읽기 전용 프로퍼티, 읽기 쓰기가 가능한 프로퍼티 등) 구현 가능  
> { get set }: 읽고 쓰기가 모두 가능해야함  

~~~ swift
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
    /*
    let to: String,
    var to: String {
        return xxx
    },
    var to: String {
        get {
            retrun xxx
        }
        set {
            self.xxx = newValue
        }
    }
    모두 가능
    */
    
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
~~~

### 20.3.2 메서드 요구

> 메서드의 실제 구현부인 {} 부분은 제외하고 작성  

~~~ swift
// 무언가를 수신받을 수 있는 기능
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

// 수신, 발신이 가능한 Message 클래스
class Message: Sendable, Receiveable {
    
    // 발신은 발신 가능한 객체, 즉 Sendable 프로토콜을 준수하는 타입의 인스턴스여야 합니다.
    var from: Sendable {
        return self
    }
    
    // 상대방은 수신 가능한 객체, 즉 Receiveable 프로토콜을 준수하는 타입의 인스턴스여야 합니다.
    var to: Receiveable?
    
    // 메시지를 발신합니다.
    func send(data: Any) {
        guard let receiver: Receiveable = self.to else {
            print("Message has no receiver")
            return
        }
        // 수신 가능한 인스턴스의 received 메서드를 호출합니다.
        receiver.received(data: data, from: self.from)
    }
    
    // 메시지를 수신합니다.
    func received(data: Any, from: Sendable) {
        print("Message received \(data) from \(from)")
    }
    
    // class 메서드이므로 상속이 가능합니다.
    class func isSendableInstance(_ instance: Any) -> Bool {
        if let sendableInstance: Sendable = instance as? Sendable {
            return sendableInstance.to != nil
        }
        return false
    }
}

// 수신, 발신이 가능한 Mail 클래스
class Mail: Sendable, Receiveable {
    
    var from: Sendable {
        return self
    }
    
    var to: Receiveable?
    
    func send(data: Any) {
        guard let receiver: Receiveable = self.to else {
            print("Mail has no receiver")
            return
        }
        
        receiver.received(data: data, from: self.from)
    }
    
    func received(data: Any, from: Sendable) {
        print("Mail received \(data) from \(from)")
    }
    
    // static 메서드이므로 상속이 불가능합니다.
    static func isSendableInstance(_ instance: Any) -> Bool {
        if let sendableInstance: Sendable = instance as? Sendable {
            return sendableInstance.to != nil
        }
        return false
    }
}

// 두 Message 인스턴스를 생성합니다.
let myPhoneMessage: Message = Message()
let yourPhoneMesssage: Message = Message()

// 아직 수신받을 인스턴스가 없습니다.
myPhoneMessage.send(data: "Hello")    // Message has no receiver

// Message 인스턴스는 발신과 수신이 모두 가능하므로 메시지를 주고 받을 수 있습니다.
myPhoneMessage.to = yourPhoneMesssage
myPhoneMessage.send(data: "Hello")    // Message received Hello from Message

// 두 Mail 인스턴스를 생성합니다.
let myMail: Mail = Mail()
let yourMail: Mail = Mail()

myMail.send(data: "Hi")   // Mail has no receiver

// Mail과 Message 모두 Sendable과 Receiveable 프로토콜을 준수하므로 서로 주고 받을 수 있습니다.
myMail.to = yourMail
myMail.send(data: "Hi")   // Mail received Hi from Mail

myMail.to = myPhoneMessage
myMail.send(data: "Bye")  // Message received Bye from Mail

// String은 Sendable 프로토콜을 준수하지 않습니다.
Message.isSendableInstance("Hello") // false

// Mail과 Message는 Sendable 프로토콜을 준수합니다.
Message.isSendableInstance(myPhoneMessage) // true

// yourPhoneMessage는 to 프로퍼티가 설정되지 않아서 보낼 수 없는 상태입니다.
Message.isSendableInstance(yourPhoneMesssage) // false
Mail.isSendableInstance(myPhoneMessage) // true
Mail.isSendableInstance(myMail) // true
~~~

### 20.3.3 가변 메서드 요구

> 프로토콜이 어떤 타입이든 간에 인스턴스 내부의 값을 변경해야 하는 메서드를 요구하려면 프로토콜의 메서드 정의 앞에 mutating 키워드를 명시해야 한다  
> 프로토콜에 mutating 키워드를 사용한 메서드 요구가 있다고 해도 클래스 구현에서는 mutating 키워드를 써주지 않아도 된다  

### 20.3.4 이니셜라이저 요구

> 구조체는 상속할 수 없기 때문에 이니셜라이저 요구에 대해 크게 신경쓸 필요가 없다  
> 클래스에서는 요구에 부합하는 이니셜라이저를 구현할 때는 required 식별자를 붙인 요구 이니셜라이저로 구현해야 한다

~~~ swift
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

class Person: Named {
    var name: String
    
    required init(name: String) {
        self.name = name
    }
}
~~~

### 20.4 프로토콜의 상속과 클래스 전용 프로토콜

> 클래스 상속 문법과 유사  
> 프로토콜의 상속 리스트에 class 키워드를 추가하면 프로토콜이 클래스 타입에만 채택될 수 있도록 제한할 수 있다  

~~~ swift
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


// 코드 20-13 클래스 전용 프로토콜의 정의
protocol ClassOnlyProtocol: class, Readable, Writeable {
    // 추가 요구사항
}

class SomeClass: ClassOnlyProtocol {
    func read() { }
    func write() { }
}

// 오류!! ClassOnlyProtocol 프로토콜은 클래스 타입에만 채택 가능합니다.
struct SomeStruct: ClassOnlyProtocol {
    func read() { }
    func write() { }
}
~~~

## 20.5 프로토콜 조합과 프로토콜 준수 확인

> & 기호를 사용해서 프로토콜 조합 요구 가능  
> 조합 요구할 때 클래스는 한개만 가능  

~~~ swift
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

func celebrateBirthday(to celebrator: Named & Aged) {
    print("Happy birthday \(celebrator.name)!! Now you are \(celebrator.age)")
}

let yagom: Person = Person(name: "yagom", age: 99)
celebrateBirthday(to: yagom)    // Happy birthday yagom!! Now you are 99

let myCar: Car = Car(name: "Boong Boong")
//celebrateBirthday(to: myCar)    // 오류 발생!! Aged를 충족시키지 못합니다!


// 클래스 & 프로토콜 조합에서 클래스 타입은 한 타입만 조합할 수 있습니다. 오류 생!
//var someVariable: Car & Truck & Aged

// Car 클래스의 인스턴스 역할도 수행할 수 있고,
// Aged 프로토콜을 준수하는 인스턴스만 할당할 수 있습니다.
var someVariable: Car & Aged

// Truck 인스턴스는 Car 클래스 역할도 할 수 있고 Aged 프로토콜도 준수하므로 할당할 수 있습니다.
someVariable = Truck(name: "Truck", age: 5)

// Car의 인스턴스인 myCar는 Aged 프로토콜을 준수하지 않으므로 할당할 수 없습니다.
// 오류 발생!
//someVariable = myCar


// 코드 20-15 프로토콜 확인 및 캐스팅
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
~~~

## 20.6 프로토콜의 선택적 요구

> 선택적 요구를 하면 프롵노콜을 준수하는 타입에 해당 요구사항을 필수로 구현할 필요가 없다  
> ojbc 속성이 부여되는 프로토콜은 Objective-C 클래스를 상속받은 클래스에서만 채택할 수 있다  

~~~ swift
import Foundation

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
~~~

## 20.7 프로토콜 변수와 상수

> 프로토콜 변수나 상수를 생성하여 특정 프로토콜을 준수하는 타입의 인스턴스를 할당할 수 있다  

~~~ swift
var someNamed: Named = Animal(name: "Animal")
someNamed = Pet(name: "Pet")
someNamed = Person(name: "Person")
someNamed = School(name: "School")!
~~~

## 20.8 위임을 위한 프로토콜

> 위임(Delegation): 클래스나 구조체가 자신의 책임이나 임무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴. 책무를 위임하기 위해 정의한 프로토콜을 준수하는 타입은 자신에게 위임될 일정 책무를 할 수 있다는 것을 보장  

> 예를 들어 UITableView 타입의 인스턴스가 해야 하는 일을 위임받아 처리하는 인스턴스는 UITableViewDelegate 프로토콜을 준수하면 된다. 이 인스턴스는 UITableView의 인스턴스가 해야 하는 일을 대신 처리해줄 수 있다.

