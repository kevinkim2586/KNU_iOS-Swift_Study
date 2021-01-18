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


<h2>21.3 익스텐션으로 추가할 수 있는 기능</h2>

<h3>21.3.1 연산 프로퍼티</h3>

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

```swfit
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
<h3>21.3.1 연산 프로퍼티</h3>


