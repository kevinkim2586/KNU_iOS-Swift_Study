<h1>Chapter 10. 프로퍼티와 메서드</h1>

<h2>10.1 프로퍼티</h2>

* 저장 프로퍼티
* 연산 프로퍼티
* 타입 프로퍼티

<h3>10.1.1 저장 프로터피</h3>

저장 프로퍼티는 '클래스'와 '구조체'에서 사용된다

```swift
//좌표
struct CoordinatePoint [ // 구조체
    var x: Int // 저장 프로퍼티
    var y: Int // 저장 프로퍼티
}

// 구조체에는 기본적으로 저장 프로퍼티를 매개변수로 갖는 이니셜라이저가 있다.
let yagomPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 5)
```

```swift
// 사람의 위치정보
class Position {
    var point: CoordinatePoint // 저장 프로퍼티 (변수) - 위치(point)는 변경될 수 있음을 의미
    let name: String // 저장 프로퍼티 (상수)
    
    init(name: String, currentPoint: CoordinatePoint) { // 저장 프로퍼티에 초기값이 없다면 클래스에서는 init을 꼭 따로 써줘야함
        self.name = name
        self.point = currentPoint
    }
}

//사용자 정의 이니셜라이저를 호출해야함 그렇지않으면 프로퍼티 초깃값을 할당할 수 없기 때문에 인스턴스 생성이 불가능
let yagomPosition: Position: Position(name: "yagom", currentPoint: yagomPoint)
```
저장 프로퍼티의 값이 있어도 그만, 없어도 그만인 옵셔널이라면 초기값을 굳이 안넣어도됨

```swift
// 좌표
struct CoordinatePoint {
    // 위치는 x, y값이 모두 있어야 하므로 옵셔널이면 안 됨
    var x: Int
    var y: Int
}

// 사람의 위치 정보
class Position {
    // 현재 사람의 위치를 모를 수도 있음
    var point: CoordinatePoint?
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

//이름은 필수지만 위치는 모를 수도 있음}
let yagomPosition: Position = Position(name: "yagom")

//위치를 알게되면 그 때 위치값 할당
yagomPosition.point = CoordinatePoint(x: 20, y: 10)
```

<h3>10.1.2 지연 저장 프로퍼티</h3>

지연 저장 프로퍼티는 호출이 있어야 값을 초기화하며 이 때 lazy 키워드를 사용 ,

주로 복잡한 클래스나 구조를 구현할 때 많이 사용하고 지연 저장 프로퍼티를 잘 사용하면 불필요한 성능저하나 공간 낭비를 줄일 수 있음

```swift
struct CoordinatePoint {
    var x: Int = 0
    var y: Int = 0
}

class position {
    lazy var point: CoordinatePoint = CoordinatePoint()
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let yagomPosition: Position = Position(name: "yagom")

//  이 코드를 통해 point 프로퍼티로 처음 접근할 때 point 프로퍼티의 CoordinatePoint가 생성됨
print(yagomPosition.point) // x:0, y: 0
```

<h3>10.1.3 연산 프로퍼티</h3>

연산 프로퍼티의 정의와 사용

연산 프로퍼티를 사용하면 하나의 프로퍼티에 접근자와 설정자가 모두 모여있고 해당 프로퍼티가 어떤 역할을 하는지 좀 더 명확하게 표현이 가능하다

관용적인 표현으로 set() 괄호 내부의 매개변수를 따로 표기하지 않고 newValue를 이용해 x = -newValue.x 이런식으로 표현이 가능하다

```swift
struct CoordinatePoint {
    var x: Int // 저장 프로퍼티
    var y: Int // 저장 프로퍼티
    
    // 대칭 좌표
    var oppositePoint: CoordinatePoint { // 연산 프로퍼티
        // 접근자
        get {
            return CoordinatePoint(x: -x, y: -y)
        }
        // 설정자
        set(opposite) {
            x = -opposite.x
            y = -opposite.y
        }
    }
}

var yagomPosition: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

//현재 좌표
print(yagomPosition) // 10, 20

//대칭 좌표
print(yagomPosition.oppositePoint) // -10, -20
```

<h3>10.1.4 프로퍼티 감시자</h3>

프로퍼티 값이 변경됨애 따라 적절한 작업을 취함 

프로퍼티 값이 변경되기 직전에 호출하는 willSet 메서드와 프로퍼티 값이 변경된 직후에 호출하는 didSet 메서드가 있음

```swift
class Account {
    var credit: Int = 0 {
        willSet {
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        
        didSet {
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
}

let myAccount: Account = Account() // 잔액이 0원에서 1000원으로 변경될 예정입니다.
my Account.credit = 1000 // 잔액이 0원에서 1000원으로 변경되었습니다.

```

<h3>10.1.5 전역변수와 지역변수</h3>

우리가 이제까지 변수라고 통칭했던 전역변수 또는 지역변수는 저장변수라고 할 수 있음

var wonInPocket: Int = 2000 {
    willSet {
        print("주머니의 돈이 \(wonInPocket)원에서 \(newValue)원으로 변경될 예정입니다.")
    }
    
    didSet {
        print("주머니의 돈이 \(oldValue)원에서 \(wonInPocket)원으로 변경되었습니다.")
    }
 }
 
 var dollarInPocket: Double {
    get {
        return Double(wonInPocket) / 1000.0
    }
    
    set {
        wonInPocket = Int(newValue * 1000.0)
        print("주머니의 달러를 \(newValue)달러로 변경 중입니다.")
    }
 }
 
 // 주머니의 돈이 2000원에서 3500원으로 변경될 예정입니다.
 // 주머니의 돈이 2000원에서 3500원으로 변경되었습니다.
 dollarInPocket = 3.5 // 주머니의 달러를 3.5달러로 변경 중입니다.

<h3>10.1.6 타입 프로퍼티</h3>

각각의 인스턴스가 아닌 타입 자체에 속하는 프로퍼티를 타입 프로퍼티라고 함

인스턴스의 생성 여부와 상관없이 타입프로퍼티의 값은 하나며, 그 타입의 모든 인스턴스가 공통으로 사용하는 값, 모든 인스턴스에서 공용으로 접근하고 값을 변경할 수 있는 변수 등을 정의 할 때 유용

```swift
class AClass {
    // 저장 타입 프로퍼티
    static var typeProperty: Int = 0
    
    // 저장 인스턴스 프로퍼티
    var instanceProperty: Int = 0 {
        didSet {
            // Self.typeProperty는 AClass.typeProperty와 같은 표현입니다.
            selft.typeProperty = instanceProperty + 100
        }
    }
    
    // 연산 타입 프로퍼티
    static var typeComputedProperty: Int {
        get {
            return typeProperty
        }
        set {
            typeProperty = newValue
        }
    }
}

AClass.typeProperty = 123

let classInstance: AClass = AClass()
classInstance.instanceProperty = 100

print(AClass.typeProperty) // 200
print(AClass.typeComputedProperty) // 200
```

<h3>10.1.7 키 경로</h3>

키 경로를 사용하여 간접적으로 특정 타입의 어떤 프로퍼티 값을 가리켜야 할지 미리 지정해두고 사용할 수 있다

WritablekeyPath<Root, Value>* '값' 타입에 키 경로 타입으로 읽고 쓸 수 있음

ReferenceWritableKeyPath<Root, Value>타입은 클래스 타입에 키 경로 타입으로 읽고 쓸 수 있음

```swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}

struct Stuff {
    var name: String
    var owner: Person
}

print(type(of: \Person.name)) // ReferenceWritableKeyPath<Person, String>
print(type(of: \Stuff.name)) // WritableKeyPath<Stuff, String>

```

<h2>10.2 메서드</h2>

메서드 - 특정 타입에 관련된 함수

구조체와 열거형이 메서드를 가질 수 있다는 것은 기존 프로그래밍 언어와 스위프트간의 큰 차이점이다


<h3>10.2.1 인스턴스 메서드</h3>

클래스의 인스턴스 메서드
```swift
Class LevelClass {
    // 현재 레벨을 저장하는 저장 프로퍼티
    var level: Int = 0 {
        // 프로퍼티 값이 변경되면 호출하는 프로퍼티 감시자
        didSet {
            print("Level \(level)")
        }
    }
    
    // 레벨이 올랐을 때 호출할 메서드
    func levelUp() {
        print("Level Up!")
        level += 1
    }
    
    // 레벨이 감소했을 때 호출할 메서드
    func levelDown() {
        print("Level Down")
        level -= 1
        if level < 0 {
            reset()
        }
    }
    
    // 특정 레벨로 이동할 때 호출할 메서드
    func jumpLevel(to: Int) {
        print("Jump to \(to)")
        level = to
    }
    
    // 레벨을 초기화할 때 호출할 메서드
    func reset() {
        print("Reset!")
        level = 0
    }
}

var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp() // Level Up! Level 1
levelClassInstance.levelDown() // Level Down Level 0
levelClassInstance.levelDown() // Level Down Level -1 Reset! Level 0
levelClassInstance.jumpLevel(to: 3) // Jump to 3 Level 3
```

mutaiting 키워드 구조체나 열거형 등 값 타입의 값을 수정할때 (인스턴스 내부의 값을 변경한다는 것 명시)

self 프로퍼티

인스턴스를 더 명확히 지칭하고 싶을 때 사용

```swift
class LevelClass {
    var level: Int = 0
    
    func jumpLevel(to level: Int) {
        print("Jump to \(Level)")
        self.level = level
    }
}
```

<h3>10.2.2 타입 메서드</h3>

타입 자체에 호출이 가능한 메서드

메서드 앞에 static 키워드를 사용하여 타입메서드임을 나타내준다

```swift

Class AClass {
    static func staticTypeMethod() {
        print("AClass staticTypeMethod")
    }
    
    class func classTypeMethod() {
        print("AClass classTypeMethod")
    }
 }
 
 class BClass: AClass {
    /*
    //  오류 발생!! 재정의 불가!
    override static func staticTypeMethod() {
    }
    */
    
    override class func classTypeMethod() {
        print("BClass classTypeMethod")
    }
 }
 
 AClass.staticTypeMethod() // AClass staticTypeMethod
 AClass.classTypeMethod() // AClass classTypeMethod
 BClass.classTypeMethod() // BClass classTypeMethod
 
 ```

