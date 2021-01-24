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

<h3>27.2.1 강한참조 순환 문제</h3>
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

var yagom: Person? = Person(name: "yagom") // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "505") // Room 인스턴스의 참조 횟수 : 1

room?.host = yagom // Person 인스턴스의 참조 횟수 : 2
yagom?.room = room // Room 인스턴스의 참조 횟수 : 2

yagom = nil // Person 인스턴스의 참조 횟수 : 1
room = nil // Room 인스턴스의 참조 횟수 : 1

// Person 인스턴스를 참조할 방법 상실 - 메모리에 잔존
// Room 인스턴스를 참조할 방법 상실 - 메모리에 잔존
```

디이니셜라이저가 호출되지 않은 것으로 보아 메모리에서 해제되지 않고 계속 남아있음 - 메모리 누수

<h2>27.3 약한참조</h2>

강한 참조 문제를 해결하기 위한 약한참조

```swift
class Room {
    let number: String
    
    init(number: String) {
        self.number = number
    }
    
    weak var host: Person?
    
    deinit {
        print("Room \(number) is being deinitialized")
    }
}

var yagom: Person? = Person(name: "yagom") // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "505") // Room 인스턴스의 참조 횟수 : 1

room?.host = yagom // Person 인스턴스의 참조 횟수 : 1
yagom?.room = room // Room 인스턴스의 참조 횟수 : 2

yagom = nil // Person 인스턴스의 참조 횟수 : 0, Room 인스턴스의 참조 횟수 : 1
// yagom is being deinitialized
print(room?.host) // nil

room = nil // Room 인스턴스의 참조 횟수 : 0
// Room 505 is being deinitialized
```

<h2>27.4 미소유 참조</h2>

```swift
class Person {
    let name: String
    
    // 카드를 소지할 수도, 소지하지 않을 수도 있기 때문에 옵셔널로 정의함
    // 또, 카드를 한 번 가진 후 잃어버리면 안 되기 때문에 강한참조를 해야함
    var card: CreditCard?
    
    init(name: String) {
        self.name = name
    }
    
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    let numberL UInt
    unowned let owner: Person // 카드는 소유자가 분명히 존재해야 함
    
    init(number: UInt, owner: Person) {
        self.number = number
        self.owner = owner
    }
    
    deinit {
        print("Card #\(number) is being deinitialized")
    }
}

var jisoo: Person? = Person(name: "jisoo") // Person 인스턴스의 참조 횟수 : 1

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
<h2>27.5 미소유 옵셔널 참조</h2>

약한참조와 같은 상황에 사용할 수 있으나 차이가 있다면 미소유 옵셔널 참조는 항상 유효한 객체를 가리키거나 그렇지 않으면 nil을 할당해주어야 함

```swift
class Department {
    var name: String
    var subjects: [Subject] = []
    init(name: String) {
        self.name = name
    }
}

class Subject {
    var name: String
    unowned var department: Department
    unowned var nextSubject: Subject?
    init(name: String, in department: Department) {
        self.name = name
        self.department = department
        self.nextSubject = nil
    }
}

let department = Department(name: "Computer Science")

let intro = Subject(name: "Computer Architecture", in: department)
let intermediate = Subject(name: "Swift Language", in: department)
let advanced = Subject(name: "ios App Programming", in: department)

intro.nextSubject = intermediate
intermediate.nextSubject = advanced
department.subjects = [intro, intermediate, advanced]
```

<h2>27.6 미소유참조와 암시적 추출 옵셔널 프로퍼티</h2>

서로 참조해야 하는 프로퍼티에 값이 꼭 있어야 하면서도 한번 초기화되면 그 이후에는 nil을 할당할 수 없는 조건을 갖추어야 할때 사용
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
        print{"\(name)의 CEO는 \(CEO.name)입니다.")
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
company.introduce() // 무한상사의 CEO는 김태호입니다.
company.ceo.introdue() // 김태호는 무한상사의 CEO입니다.
```


<h2>27.7 클로저의 강한참조 순환</h2>

