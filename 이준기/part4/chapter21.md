# Chapter21 익스텐션

## 21.1 익스텐션이란

- 구조체, 클랫, 열거형, 프로토콜 타입에 새로운 기능을 추가할수 있는 기능
- 익스텐션이 추가할수 있는 기능은 아래와 같다
	- 연산 타입 프로퍼티 / 연산 인스턴스 프로퍼티
	- 타입메서드 / 인스턴스 메서드
	- 서브스크립트
	- 중첩 타입
	- 특정 프로토콜을 준수할 수 있도록 기능 추가

- \*익스텐션은 타입에 새로운 기능을 추가할 수는 있지만, 기존에 존재하는 기능을 재정의 할수는 없다.\*

- 클래스의 상속은 클래스 타입에서만 가능하지만, 익스텐션은 구조체, 클래스, 프로토콜 등에 적용이 가능

## 21.2 인스텐션 문법

- extension이라는 키워드를 사용하여 선언함

```swift
extension 확장할 타입 이름 {
    // 타입에 추가될 새로운 기능 구현
}

extension 확장할 타입 이름: 프로토콜1, 프로토콜2, 프로토콜3 {
    // 프로토콜 요구사항 구현
}
```

## 21.3 익스텐션으로 추가할 수 있는 기능

### 21.3.1 연산 프로퍼티

- 익스텐션을 통해서 타입에 연산 프로퍼티를 추가할수 있습니다.

```swift
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    
    var isOdd: Bool {
        return self % 2 == 1
    }
}

print(1.isEven) // false
print(2.isEven) // true
print(1.isOdd)  // true
print(2.isOdd)  // false

var number: Int = 3
print(number.isEven)    // false
print(number.isOdd)     // true

number = 2
print(number.isEven)    // true
print(number.isOdd)     // false
```

### 21.3.2 메서드

- 익스텐션을 통해서 타입에 메서드를  추가할수 있습니다.

```swift
extension Int {
    func multiply(by n: Int) -> Int {
        return self * n
    }
    
    mutating func multiplySelf(by n: Int) {
        self = self.multiply(by: n)
    }
    
    static func isIntTypeInstance(_ instance: Any) -> Bool {
        return instance is Int
    }
}

print(3.multiply(by: 2))  // 6
print(4.multiply(by: 5))  // 20

var number: Int = 3

number.multiplySelf(by: 2)
print(number)   // 6

number.multiplySelf(by: 3)
print(number)   // 18

Int.isIntTypeInstance(number)   // true
Int.isIntTypeInstance(3)        // true
Int.isIntTypeInstance(3.0)      // false
Int.isIntTypeInstance("3")      // false

prefix operator ++

struct Position {
    var x: Int
    var y: Int
}

extension Position {
    // + 중위 연산 구현
    static func + (left: Position, right: Position) -> Position {
        return Position(x: left.x + right.x, y: left.y + right.y)
    }
    
    // - 전위 연산 구현
    static prefix func - (vector: Position) -> Position {
        return Position(x: -vector.x, y: -vector.y)
    }
    
    // += 복합할당 연산자 구현
    static func += (left: inout Position, right: Position) {
        left = left + right
    }
}

extension Position {
    // == 비교 연산자 구현
    static func == (left: Position, right: Position) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
    
    // != 비교 연산자 구현
    static func != (left: Position, right: Position) -> Bool {
        return !(left == right)
    }
}


extension Position {
    
    // ++ 사용자정의 연산자 구현
    static prefix func ++ (position: inout Position) -> Position {
        position.x += 1
        position.y += 1
        return position
    }
}

var myPosition: Position = Position(x: 10, y: 10)
var yourPosition: Position = Position(x: -5, y: -5)

print(myPosition + yourPosition)    // Position(x: 5, y: 5)
print(-myPosition)                  // Position(x: -10, y: -10)

myPosition += yourPosition
print(myPosition)       // Position(x: 5, y: 5)

print(myPosition == yourPosition)   // false
print(myPosition != yourPosition)   // true

print(++myPosition)     // Position(x: 6, y: 6)
```

### 21.3.3 이니설라이즈

- 인스턴스를 초기화할 때 인스턴스 초기화에 필요한 다양한 데이터를 전달받을 수 있도록 여러 종류의 이니셜라이저를 만들 수 있다.

- 타입의 정의부에 이니셜라이저를 추가하지 않더라도 익스텐션을 통해 이니셜라이저를 추가할 수 있다. 

- 익스텐션으로 클래스 타입에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가할 수 없다. 지정 이니셜라이저와 디이니셜라이저는 반드시 클래스 타입의 구현부에 위치해야 한다.

 ```swift
extension String {
    init(intTypeNumber: Int) {
        self = "\(intTypeNumber)"
    }
    
    init(doubleTypeNumber: Double) {
        self = "\(doubleTypeNumber)"
    }
}

let stringFromInt: String = String(100)         // "100"
let stringFromDouble: String = String(100.0)    // "100.0"

class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
}


extension Person {
    convenience init() {
        self.init(name: "Unknown")
    }
}

let someOne: Person = Person()
print(someOne.name) // "Unknown"
```

### 21.3.4 서브스크립트

- 익스텐션을 통해 타입에 서브스크립트를 추가할수 있습니다

```swift
extension String {
    subscript(appedValue: String) -> String {
        return self + appedValue
    }
    
    subscript(repeatCount: UInt) -> String {
        var str: String = ""
        
        for _ in 0..<repeatCount {
            str += self
        }
        
        return str
    }
}

print("abc"["def"]) // "abcdef"
print("abc"[3])     // "abcabcabc"
```

### 21.3.5 중첩 데이터 타입
