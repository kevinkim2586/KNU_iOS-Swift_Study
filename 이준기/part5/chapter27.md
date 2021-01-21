# Chapter27 ARC

- 매번 전달할 때 마다 값을 복사해 전달하는 값 타입과는 달리 참조 타입은 하나의 인스턴스가 참조를 통해 여러곳에서 접근하기 때문에 언제 메모리에서 해제되는지가 중요한 문제
- 스위프트는 이를 위해서 메모리 관리 기법 인 ARC를 사용함.

## 27.1 ARC란

- 메모리를 자동으로 관리해주는 방식
- arc는 더이상 필요하지 않은 클래스의 인스턴스를 메모리에서 해제하는 방식으로 동작

## 27.2 강한참조

- 인스턴스가 메모리에 남아있어야 하는 명분을 만들어 주는 것이 바로 강한 참조임
	- 인스턴스는 참조 횟수가 0이 되는 순간 메모리에서 해제되는데, 인스턴스를 다른 인스턴스의 프로퍼티나 변수 상수등에 할당 할때 강한참조를 사용하면 참조횟수가 1증가함
	- 강한참조를 사용하는 프로퍼티 변수 상수 등에 nil 을 할당해주면 원래 자신에게 할당 되어 있던 인스턴스의 참조횟수가 1감소함

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

reference3 = nil    // 인스턴스의 참조 횟수 : 2
reference2 = nil    // 인스턴스의 참조 횟수 : 1
reference1 = nil    // 인스턴스의 참조 횟수 : 0
// yagom is being deinitialized
```

### 27.2.1 강한참조 순환 문제

- 인스턴스 끼리 서로가 서로를 강한참조할 때
- 운영체제에서 데드락

```swift
class Person {
    
    let name: String
    
    init(name: String) {
        self.name = name
    }
    
    var room: Room?
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

class Room {
    
    let number: String
    
    init(number: String) {
        self.number = number
    }
    
    var host: Person?
    
    deinit {
        print("Room \(number) is being deinitialized")
    }
}


var yagom: Person? = Person(name: "yagom")  // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "505")   // Room 인스턴스의 참조 횟수 : 1

room?.host = yagom  // Person 인스턴스의 참조 횟수 : 2
yagom?.room = room  // Room 인스턴스의 참조 횟수 : 2

yagom = nil // Person 인스턴스의 참조 횟수 : 1
room = nil  // Room 인스턴스의 참조 횟수 : 1

// Person 인스턴스를 참조할 방법 상실 - 메모리에 잔존
// Room 인스턴스를 참조할 방법 상실 - 메모리에 잔존
```

## 27.3 약한 참조

- 약한 참조는 강한 참조와 달리 자신이 참조하는 인스턴스의 참조 횟수를 증가시키지 않음
- 참조 타입의 프로퍼티나 변수의 선언 앞에 weak 키워드를 써주면 그 프로퍼티나 변수는 자신이 참조하는 인스턴스를 약한 참조함

## 27.4 미소유참조

- 약한 참조와 마찬가지로 인스턴스의 참조 횟수를 증가시키지 않음
- 미소유 참조는 약한 참조와 다르게 자신이 참조하는 인스턴스가 항상 메모리에 존재할 것이라는 전제를 기반으로 동작

```swift
class Person {
    
    let name: String
    
    var card: CreditCard?	// 카드를 소지할 수도, 소지하지 않을 수도 있기 때문에 옵셔널로 정의합니다.
    // 또, 카드를 한 번 가진 후 잃어버리면 안 되기 때문에 강한참조를 해야 합니다.
    init(name: String) {
        self.name = name
    }
    
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    
    let number: UInt
    
    unowned let owner: Person	// 카드는 소유자가 분명히 존재해야 합니다.
    
    init(number: UInt, owner: Person) {
        self.number = number
        self.owner = owner
    }
    
    deinit {
        print("Card #\(number) is being deinitialized")
    }
}


var jisoo: Person? = Person(name: "jisoo")  // Person 인스턴스의 참조 횟수 : 1

if let person: Person = jisoo {
    // CreditCard 인스턴스의 참조 횟수 : 1
    person.card = CreditCard(number: 1004, owner: person)
    // Person 인스턴스의 참조 횟수 : 1
}

jisoo = nil // Person 인스턴스의 참조 횟수 : 0
// CreditCard 인스턴스의 참조 횟수 : 0
// jisoo is being deinitialized
// Card #1004 is being deinitialized
```

## 27.5 미소유참조와 임시적 추출 옵셔널 프로퍼티

- 약한 참조와 미소유 참조의 예제에서 강한 참조 순환 문제 2가지를 해결

```swift
class Company {
    let name: String
    // 암시적 추출 옵셔널 프로퍼티 (강한참조)
    var ceo: CEO!
    
    init(name: String, ceoName: String) {
        self.name = name
        self.ceo = CEO(name: ceoName, company: self)
    }
    
    func introduce() {
        print("\(name)의 CEO는 \(ceo.name)입니다.")
    }
}

class CEO {
    let name: String
    // 미소유참조 상수 프로퍼티 (미소유참조)
    unowned let company: Company
    
    init(name: String, company: Company) {
        self.name = name
        self.company = company
    }
    
    func introduce() {
        print("\(name)는 \(company.name)의 CEO입니다.")
    }
}

let company: Company = Company(name: "무한상사", ceoName: "김태호")
company.introduce()     // 무한상사의 CEO는 김태호입니다.
company.ceo.introduce() // 김태호는 무한상사의 CEO입니다.
```

## 27.6 클로저의 강한참조 순환

- 강한 참조의 순환 문제는 클로저가 인스턴스의 프로퍼티 일때나 클로저의 값 획득 특성 떄문에 발생

```swift
class Person {
    let name: String
    let hobby: String?
    
    lazy var introduce: () -> String = {
        
        var introduction: String = "My name is \(self.name)."
        
        guard let hobby = self.hobby else {
            return introduction
        }
        
        introduction += " "
        introduction += "My hobby is \(hobby)."
        
        return introduction
    }
    
    init(name: String, hobby: String? = nil) {
        self.name = name
        self.hobby = hobby
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

var yagom: Person? = Person(name: "yagom", hobby: "eating")

print(yagom?.introduce())   // My name is yagom. My hobby is eating.

yagom = nil
```

### 27.6.1 획득목록

- 획득 목록은 클로저 내부에서 참조 타입을 획득하는 규칙을 제시해줄수 있는 기능

```swift
class Person {
    let name: String
    let hobby: String?
    
    lazy var introduce: () -> String = { [unowned self] in
        
        var introduction: String = "My name is \(self.name)."
        
        guard let hobby = self.hobby else {
            return introduction
        }
        
        introduction += " "
        
        introduction += "My hobby is \(hobby)."
        
        return introduction
    }
    
    init(name: String, hobby: String? = nil) {
        self.name = name
        self.hobby = hobby
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

var yagom: Person? = Person(name: "yagom", hobby: "eating")

print(yagom?.introduce())   // My name is yagom. My hobby is eating.

yagom = nil // yagom is being deinitialized
```

