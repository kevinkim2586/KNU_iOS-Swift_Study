<h1>Chapter 21. 익스텐션</h1>

<h2>21.1 익스텐션이란</h2>

익스텐션 - 구조체, 클래스, 열거형, 프로토콜 타입에 새로운 기능을 추가 가능

상속 - 수직 확장 ( 부모클래스에서 자식클래스로 상속)

익스텐션 - 수평 확장 ( 기존의 타입에 기능을 추가 , 재정의 불가)

<h2>21.2 익스텐션 문법</h2>


```swift
extension 확장할 타입 이름 {
    // 타입에 추가될 새로운 기능 구현
}
```

```swift
extension 확장할 타입 이름: 프로토콜1, 프로토콜2, 프로토콜3 {
    // 프로토콜 요구사항 구현
}
```

<h2>21.3 익스텐션으로 추가할 수 있는 기능</h2>

<h3>21.3.1 연산 프로퍼티</h3>

익스텐션으로 연산 프로퍼티를 추가할 수는 있지만, 저장 프로퍼티를 추가할 순 없다 또, 타입에 정의되어 있는 기존의 프로퍼티에 프로퍼티 옵저버를 추가할 수도 없다.

```swift
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    
    var isOdd: Bool {
        return self % w == 0
    }
}

print(1.isEven) // false
print(2.isEven) // true
print(1.isOdd) // true
print(2.isOdd) // false

var number: Int = 3
print(number.isEven) // false
print(number.isOdd) // true

number = 2
print(number.isEven) // true
print(number.isOdd) // false
```

<h3>21.3.2 메서드</h3>

```swift
extension Int {
    func repetitions(task: () -> Void) {
        for _ in 0..<self {
            task()
        }
    }
}

3.repetitions {
    print("Hello!")
}
// Hello!
// Hello!
// Hello!
```
<h3>21.3.3 이니셜라이저</h3>

```swift
struct Size {
    var width = 0.0, height = 0.0
}
struct Point {
    var x = 0.0, y = 0.0
}
struct Rect {
    var origin = Point()
    var size = Size()
}

let defaultRect = Rect()
let memberwiseRect = Rect(origin: Point(x: 2.0, y: 2.0),
   size: Size(width: 5.0, height: 5.0))
   
extension Rect {
    init(center: Point, size: Size) {
        let originX = center.x - (size.width / 2)
        let originY = center.y - (size.height / 2)
        self.init(origin: Point(x: originX, y: originY), size: size)
    }
}

let centerRect = Rect(center: Point(x: 4.0, y: 4.0),
                      size: Size(width: 3.0, height: 3.0))
// centerRect's origin is (2.5, 2.5) and its size is (3.0, 3.0)
```

<h3>21.3.4 서브 스크립트</h3>

```swift
extension String {
    subscript(appedValue: String) -> String {
        return self + appedValue
    }
    
    subscript(repeatCount: UInt) -> String {
        var str: String = " "
        
        for _ in 0..<repeatCount {
            str += self
        }
        
        return str
     }
}

print("abc"["def"]) // "abcdef"
print("abc"[3]) // abcabcabc
```
<h3>21.3.5 중첩 데이터 타입</h3>

```swift
extension Int {
    enum Kind {
        case negative, zero, positive
    }
    var kind: Kind {
        switch self {
        case 0:
            return .zero
        case let x where x > 0:
            return .positive
        default:
            return .negative
        }
    }
}

func printIntegerKinds(_ numbers: [Int]) {
    for number in numbers {
        switch number.kind {
        case .negative:
            print("- ", terminator: "")
        case .zero:
            print("0 ", terminator: "")
        case .positive:
            print("+ ", terminator: "")
        }
    }
    print("")
}
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])
// Prints "+ + - 0 - 0 + "

number.kind가 이미 switch에서 Int.Kind 타입이라는 것을 알고 있기 때문에 안의 case에서 kind의 축약형인 .negative, .zeo, .positive로 사용할 수 있습니다.
```
