# Chapter10 프로퍼티와 메서드

## 10.1 프로퍼티

> 저장 프로퍼티(Stored Properties)  
> 연산 프로퍼티(Computed Properties)  
> 타입 프로퍼티(Type Properties)

### 10.1.1 저장 프로퍼티

> 클래스 또는 구조체의 인스턴스와 연관된 값을 저장하는 프로퍼티

> Note  
> *구조체는 저장 프로퍼티를 모두 포함하는 이니셜라이저를 자동으로 생성  
> *클래스의 저장 프로퍼티는 옵셔널이 아니라면 프로퍼티 기본값을 지정해주거나 사용자 정의 이니셜라이저를 통해 반드시 초기화해주어야 한다  
> *클래스 인스턴스의 상수 프로퍼티는 인스턴스가 초기화될 때 한번만 값을 할당할 수 있으며, 자식클래스에서 이 초기화를 변경(재정의)할 수 없다

~~~ swift
struct CoordinatePoint {
    var x: Int      // 저장 프로퍼티
    var y: Int      // 저장 프로퍼티
}

// 구조체에는 기본적으로 저장 프로퍼티를 매개변수로 갖는 이니셜라이저가 있다
let myPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 5)

class Position {
    // 현재 사람의 위치를 모를 수도 있다. - 옵셔널
    var point: CoordinatePoint?
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

// 이름은 필수지만 위치는 모를 수 있다
let myPosition: Position = Position(name: "jin")

// 위치를 알게 되면 그 때 위치 값을 할당
myPosition.point = CoordinatePoint(x: 20, y: 10)
~~~

### 10.1.2 지연 저장 프로퍼티

> 필요할 때 값이 할당: 호출이 있어야 값을 초기화함  
> lazy 키워드 사용  
> 상수는 인스턴스가 완전히 생성되기 전에 초기화해야 하므로 지연 저장 프로퍼티는 상수로 정의 X

~~~ swift
struct CoordinatePoint {
    var x: Int = 0
    var y: Int = 0
}

class Position {
    lazy var point: CoordinatePoint = CoordinatePoint()
    let name: String
    
    init(name: String) {
        self.name = name
    }
}

let myPosition: Position = Position(name: "jin")

// 이 코드를 통해 point 프로퍼티로 처음 접근할 때
// point 프로퍼티의 CoordinatePoint가 생성됨
print(myPosition.point)
~~~

Note 다중 스레드와 지연 저장 프로퍼티
> *다중 스레드 환경에서 지연 저장 프로퍼티에 동시다발적으로 접근할 때는 한 번만 초기화 된다는 보장이 없다.  
> *생성되지 않은 지연 저장 프로퍼티에 많은 스레드가 비슷한 시점에 접근하면, 여러 번 초기화될 수 있다.

### 10.1.3 연산 프로퍼티

> 특정 상태에 따른 값을 연산하는 프로퍼티  
> 접근자(Getter), 설정자(Setter) 역할

~~~ swift
struct CoordinatePoint {
    var x: Int
    var y: Int
    
    // 대칭 좌표
    var oppositePoint: CoordinatePoint {    // 연산 프로퍼티
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

var myPoint: CoordinatePoint = CoordinatePoint(x: 10, y: 20)

// 현재 좌표
print(myPoint)                  // 10, 20

// 대칭 좌표
print(myPoint.oppositePoint)    // -10, -20

// 대칭 좌표를 (15, 10)으로 설정하면
myPoint.oppositePoint = CoordinatePoint(x: 15, y: 10)

// 현재 좌표는 -15, -10으로 설정된다
print(myPoint)                  // -15, -10
~~~

### 10.1.4 프로퍼티 감시자

> 일반 저장 프로퍼티에만 적용  
> 프로퍼티의 값이 새로 할당될 때마다 호출(변경되는 값이 현재의 값과 같더라도 호출)  

~~~ swift
class Account {
    var credit: Int {
        willSet {
            print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
        }
        didSet {
            print("잔액이 \(oldValue)원에서 \(credit)원으로 변경되었습니다.")
        }
    }
    init(credit: Int) {
        self.credit = credit
    }
}

let myAccount: Account = Account(credit: 0)

// 잔액이 0원에서 1000원으로 변경될 예정입니다.
myAccount.credit = 1000
// 잔액이 0원에서 1000원으로 변경되었습니다.
~~~

### 10.1.5 전역변수와 지역변수

> 연산 프로퍼티와 프로퍼티 감시자는 전역번수와 지역변수 모두에 사용가능  
> \**전역변수 또는 전역상수는 지연 저장 프로퍼티처럼 처음 접근할 때 최초로 연산이 이루어짐*

~~~ swift
var wonInPocket: Int = 2000 {
    willSet {
        print("주머니의 돈이 \(wonInPocket)원에서 \(newValue)원으로 변경될 예정")
    }
    
    didSet {
        print("주머니의 돈이 \(oldValue)원에서 \(wonInPocket)원으로 변경됨")
    }
}

var dollarInPocket: Double {
    get {
        return Double(wonInPocket) / 1000.0
    }
    
    set {
        wonInPocket = Int(newValue * 1000.0)
        print("주머니의 달러를 \(newValue)달러로 변경 중입니다")
    }
}

// 주머니의 돈이 2000원에서 3500원으로 변경될 예정
// 주머니의 돈이 2000원에서 3500원으로 변경됨
dollarInPocket = 3.5    // 주머니의 달러를 3.5달러로 변경 중입니다
~~~


### 10.1.6 타입 프로퍼티

> 각각의 인스턴스가 아닌 타입 자체에 속하는 프로퍼티  

> 저장 타입 프로퍼티: 변수 또는 상수 (반드시 초깃값 설정해야 하고 지연 연산됨, **다중 스레드 환경이라고 하더라도 단 한번만 초기화된다는 보장을 받음**)  
> 연산 타입 프로퍼티: 상수

~~~ swift
class AClass {
    // 저장 타입 프로퍼티
    static var typeProperty: Int = 0
    
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

print(AClass.typeProperty)
AClass.typeComputedProperty = 100
print(AClass.typeProperty)
~~~

### 10.1.7 키 경로

> 프로퍼티의 위치만 참조하도록 하는것

~~~ swift
class Person {
    let name: String
    init(name: String) {
        self.name = name
    }
}

struct Stuff {
    var name: String
    var owner: Person
}

let jin = Person(name: "jin")
let yuju = Person(name: "yuju")
let macBook = Stuff(name: "MacBook Pro", owner: jin)
let iphone = Stuff(name: "iPhone 12 Pro", owner: yuju)

let stuffNameKeyPath = \Stuff.name
let ownerKeyPath = \Stuff.owner
let ownerNameKeyPath = ownerKeyPath.appending(path: \.name)

print(macBook[keyPath: stuffNameKeyPath])           // MacBook Pro
print(iphone[keyPath: stuffNameKeyPath])            // iPhone 12 Pro

print(macBook[keyPath: ownerNameKeyPath])           // jin
print(iphone[keyPath: ownerNameKeyPath])            // yuju
~~~

## 10.2 메서드

> 특정 타입에 관련된 함수  
> *구조체와 열거형이 메서드를 가질 수 있음

### 10.2.1 인스턴스 메서드

> 특정 타입의 인스턴스에 속한 함수: 인스턴스가 존재할 때만 사용 가능  
> *구조체나 열거형 등의 값 타입에서는 메서드가 인스턴스 내부의 값을 변경할 때 mutating을 붙여줘야함

~~~ swift
class AClass {
    var level: Int = 0

    // 인스턴스 메서드
    func levelUp() {
        print("Level Up!")
        self.level += 1
    }
}
~~~

#### **self 프로퍼티**

> 인스턴스 자기 자신을 가리키는 프로퍼티  
> **Self와 비교**: Self는 인스턴스가 아닌 타입 자체를 가리킴

### 10.2.2 타입 메서드

> static 또는 class 키워드 사용  
> 상속 시에 static이면 override 불가, class이면 override 가능  
> **타입 메서드 내에서 self는 Self와 같아짐**

~~~ swift
class BClass: AClass {
    // 오류 발생!!
    override static func staticTypeMethod() {
        
    }
    
    override class func classTypeMethod() {
        print("BClass classTypeMethod")
    }
}
~~~
