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

지연 저장 프로퍼티는 호출이 있어야 값을 초기화하며 이 때 lazy 키워드를 사용
주로 복잡한 클래스나 구조채를 구현할 때 많이 사용하고 지연 저장 프로퍼티를 잘 사용하면 불필요한 성능저하나 공간 낭비를 줄일 수 있음

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

<h3>10.1.4 프로퍼티 감시자</h3>

<h3>10.1.5 전역변수와 지역변수</h3>

<h3>10.1.6 타입 프로퍼티</h3>

<h3>10.1.7 키 경로</h3>

<h2>10.2 메서드</h2>

<h3>10.2.1 인스턴스 메서드</h3>

<h3>10.2.2 타입 메서드</h3>

