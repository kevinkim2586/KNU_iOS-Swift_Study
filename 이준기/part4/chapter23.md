# Chapter23 프로토콜 지향 프로그래밍

- 스위프트는 프로토콜 지향 언어임
- 타입과 관련된 대부분이 구조체로 구현되어 있음
- 보통 클래스, 상속 등을 활용하지 않음
- 상속도 되지 않는 구조체로 어떻게 그렇게 다양한 공통 기능을 가질 수 있는걸까?

## 23.1 프로토콜 초기구현

- 익스텐션과 프로토콜의 결합으로 간단하게 프로토콜이 구현 가능하다

```swift
protocol Receiveable {
    func received(data: Any, from: Sendable)
}

extension Receiveable {
    
    // 메시지를 수신합니다.
    func received(data: Any, from: Sendable) {
        print("\(self) received \(data) from \(from)")
    }
}

// 무언가를 발신할 수 있는 기능
protocol Sendable {
    var from: Sendable { get }
    var to: Receiveable? { get }
    
    func send(data: Any)
    
    static func isSendableInstance(_ instance: Any) -> Bool
}

extension Sendable {
    
    // 발신은 발신 가능한 객체, 즉 Sendable 프로토콜을 준수하는 타입의 인스턴스여야 합니다.
    var from: Sendable {
        return self
    }
    
    // 메시지를 발신합니다.
    func send(data: Any) {
        guard let receiver: Receiveable = self.to else {
            print("Message has no receiver")
            return
        }
        // 수신 가능한 인스턴스의 received 메서드를 호출합니다.
        receiver.received(data: data, from: self.from)
    }
    
    static func isSendableInstance(_ instance: Any) -> Bool {
        if let sendableInstance: Sendable = instance as? Sendable {
            return sendableInstance.to != nil
        }
        return false
    }
}

// 수신, 발신이 가능한 Message 클래스
class Message: Sendable, Receiveable {
    var to: Receiveable?
}

// 수신, 발신이 가능한 Mail 클래스
class Mail: Sendable, Receiveable {
    var to: Receiveable?
}

// 두 Message 인스턴스를 생성합니다.
let myPhoneMessage: Message = Message()
let yourPhoneMesssage: Message = Message()

// 아직 수신받을 인스턴스가 없습니다.
myPhoneMessage.send(data: "Hello")    // Message has no receiver

// Message 인스턴스는 발신과 수신이 모두 가능하므로 메시지를 주고 받을 수 있습니다.
myPhoneMessage.to = yourPhoneMesssage
myPhoneMessage.send(data: "Hello")    // Message received Hello from Message

// Mail 인스턴스를 두 개 생성합니다.
let myMail: Mail = Mail()
let yourMail: Mail = Mail()

myMail.send(data: "Hi")   // Mail has no receiver

// Message와 Mail 모두 Sendable과 Receiveable 프로토콜을 준수하므로 서로 주고 받을 수 있습니다.
myMail.to = yourMail
myMail.send(data: "Hi")   // Mail received Hi from Mail

myMail.to = myPhoneMessage
myMail.send(data: "Bye")  // Message received Bye from Mail

// String은 Sendable 프로토콜을 준수하지 않습니다.
Message.isSendableInstance("Hello") // false

// Message와 Mail은 Sendable 프로토콜을 준수합니다.
Message.isSendableInstance(myPhoneMessage) // true
// yourPhoneMessage는 to 프로퍼티가 설정되지 않아서 보낼 수 없는 상태입니다.
Message.isSendableInstance(yourPhoneMesssage) // false
Mail.isSendableInstance(myPhoneMessage) // true
Mail.isSendableInstance(myMail) // true
```

## 23.2 맵,필터,리듀스 직접 구현해보기

## 23.3 기본 타입 확장

- 프로토콜 초기구현을 통해 스위프트의 기본 타입을 확장하여 내가 원하는 기능을 공통적으로 추가해볼 수도 있음

```swift
protocol SelfPrintable {
    func printSelf()
}

extension SelfPrintable {
    func printSelf() {
        print(self)
    }
}

extension Int: SelfPrintable { }
extension String: SelfPrintable { }
extension Double: SelfPrintable { }

1024.printSelf()    // 1024
3.14.printSelf()    // 3.14
"hana".printSelf()  // "hana"
```
