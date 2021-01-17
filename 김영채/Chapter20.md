# Chapter 20 : 프로토콜

- 프로토콜(Protocol)은 특정 역할을 하기 위한 메서드, 프로퍼티, 기타 요구사항 등의 청사진을 정의한다.
- 구조체, 클래스 등에서 프로토콜을 '채택'해서 프로토콜의 요구사항 구현 가능

    → 프로토콜을 준수하는가? Conform

** 프로토콜을 준수하려면 제시하는 모든 기능을 구현해야 함

→ 프로토콜은 제시만 할 뿐이지 스스로 기능을 구현하지 않음 (override X)

```swift
protocol CarProto{
	var color: String { get set }
	//any class that conforms to this protocol must be ables=
	//to "get" and "set" the variable color.

	func drive()
	func isAllWheelDrive() -> Bool
	//Defining functions only. Doesn't implement them. Another
	//class will implement the specific functions
}

class Car{

}

//a class can conform to one or more protocols
class BMW: Car, CarProto{

	var color: String = ""
	
	func drive(){
	
	}
	
	func isAllWheelDrive()->Bool{
	
	}	

}
```

### 프로토콜 요구사항

- 프로토콜은 자신을 채택한 타입이 어떤 프로퍼티를 구현해야 하는지 요구 가능
- But 프로퍼티의 종류 (연산 프로퍼티, 저장 프로퍼티)는 신경 안 씀.
- 이름과 타입만 맞으면 됨
- 대신 읽기 전용 or 읽고 쓰기 모두 가능은 프로토콜이 정해야 함

```swift
//읽고 쓰기 모두 가능
protocol someProtocol{
	var settableProperty: String { get set }
	var notNeedToBeSettableProperty: String { get }
}
```

ex) 프로토콜을 준수하는 클래스 예시

```swift
protocol Sendable{
	var from: String { get }
	var to: String { get }
}

class Message: Sendable{
	var sender: String
	var from: String{
		return self.sender
	}
	var to: String
	
	init(sender: String, receiver: String){
		self.sender = sender
		self.to = receiver
	}
}
```

### 메서드 요구

- { } 내의 부분은 제외하고, 메서드의 이름, 매개변수, 반환 타입 등만 작성하며 가변 매개변수도 허용
- 매개변수 기본값을 프로토콜 내에서 지정 불가능

### 가변 메서드 요구

- 프로토콜이 어떤 타입이든 간에 인스턴스 내부의 값을 변경해야 하는 메서드를 요구하려면 프로토콜의 메서드 정의 앞에 mutating 키워드 명시 필요 → 구조체, 열거형 (값 타입 → Class X)

```swift
protocol Resettable{
	mutating func reset(){
}
```

→ 클래스 구현에서는 따로 신경 쓸 필요 X

### 이니셜라이저 요구

- 이니셜라이저를 프로토콜 내에 정의하는 것
- 매개변수만 딱 지정. 구현은 No.

```swift
protocol Named{
	var name: String { get }

	init(name: String)
}

struct Pet: Named{
	var name: String

	init(name: String){
		self.name = name
	}
}
```

→ 구조체는 이렇게 init ( ) 을 구현하고 끝이지만, 클래스는 상속 개념이 붙어있기 때문에 추가로 고려해야 할 점이 있음

```swift
class Person: Named{
	var name: String
	
	required init(name: String){
		self.name = name
	}
}
//required 키워드가 붙어있음
```

** Person 클래스를 상속 받는 모든 클래스는 똑같이 Named 프로토콜을 준수해야 한다. 그러려면 반드시 이니셜라이저를 위 형태로 구현해야 하는데, 이를 강제하기 위해서 required 키워드를 꼭 붙여줘야 한다!

→ 만약 클래스를 상속할 일이 없다 판단하면, final 키워드를 클래스 앞에 붙여주는 것도 방법임

```swift
final class Person: Named{
	var name: String
	
	init(name: String){
		self.name = name
	}
}
```

### 프로토콜의 상속과 클래스 전용 프로토콜

```swift
protocol Readable{
	func read()
}

protocol ReadSpeakable: Readable{
	func speak()
}

class SomeClass: ReadSpeakable{
	func speak(){
		print("speak")
	}
	func read(){
		print("read")
	}
}
```

→ ReadSpeakable 프로토콜을 준수하기 위해서는 결국 Readable 프로토콜까지 준수해야 함

- 프로토콜이 클래스 타입에만 채택될 수 있도록 제한하는 것도 가능

```swift
protocol ClassProtocol: class, Readable{
	..
}

struct SomeStruct: ClassProtocol{
	..
}
//오류! 클래스 타입에만 채택 가능
```

### 프로토콜 조합과 프로토콜 준수 확인

- 하나의 매개변수가 여러 프로토콜을 모두 준수하는 타입이어야 한다면 조합하여 사용 가능

→ SomeProtocol & AnotherProtocol 처럼 조합하여 표현한다

```swift
protocol Named{
	var name: String { get }
}

protocol Aged{
	var age: Int { get }
}

func celebrateBirthday(to celebrator: Named & Aged){
	print("Happy bday \(celebrator.name)!! You are \(celebrator.age)")
}
```

- is 연산자를 통해 해당 인스턴스가 특정 프로토콜 준수하는지 확인 가능
- as? as! 연산자를 통해 다른 프로토콜로 다운캐스팅 시도 가능

```swift
print(yagom is Named) //true
print(yagom is Aged)  //true

if let castedInstance: Named = yagom as? Named{
	//
}
//이런식으로 사용 가능
```

### 위임을 위한 프로토콜

위임: Delegation

→ 클래스나 구조체가 자신의 책임이나 임무를 다른 타입의 인스턴스에게 위임하는 디자인 패턴 

- 위임은 사용자의 특정 행동에 반응하기 위해 사용되기도 한다.

ex. UITableView 타입의 인스턴스가 해야 하는 일을 위임받아 처리하는 인스턴스는 UITableViewDelegate 프로토콜을 준수하면 됨
