<h1>Chapter 9. 구조체와 클래스</h1>

<h2>9.1 구조체</h2>

<h3>9.1.1 구조체 정의</h3>

구조체 정의

```swift
struct BasicInformation {
  var name: String
  var age: Int
}
```

<h3>9.1.2 구조체 인스턴스의 생성 및 초기화</h3>

구조체 정의를 마친 후 인스턴스를 생성하고 초기화하고자 할 때는 기본적으로 생성되는 멤버와이즈 이니셜라이저를 사용

```swift
// 프로퍼티 이름(name, age)로 자동 생성된 이니셜라이저를 사용하여 구조체를 생성
var yagomInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
yagomInfo.age = 100 // 변경 가능
yagomInfo.name = "seba" //변경 가능

// 프로퍼티 이름(name, age)로 자동 생성된 이니셜라이저를 사용하여 구조체를 생성
// 구조체를 상수 let으로 선언하면 인스턴스 내부의 프로퍼티 값을 변경할 수 없다
let sebaInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
sebaInfo.age = 100 // 변경불가
jennyInfo.age = 100 // 변경불가
```

<h2>9.2 클래스</h2>

<h3>9.2.1 클래스정의</h2>

```swift
class Person {
  var height: Float = 0.0
  var weight: Float = 0.0
}
```

<h3>9.2.2 클래스 인스턴스의 생성과 초기화</h3>


<h3>9.2.3 클래스 인스턴스의 소멸</h3>

<h2>9.3 구조체와 클래스의 차이</h3>

<h3>9.3.2 스위프트의 기본 데이터 타입은 모두 구조체</h3>

<h2>9.4 구조체와 클래스 선택해서 사용하기</h2>
