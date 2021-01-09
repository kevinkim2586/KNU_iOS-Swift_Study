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

클래스를 정의한 후 인스턴스를 생성하고 초기화하고자 할 때는 기본적인 이니셜라이저를 사용한다 

클래스의 인스턴스는 참조 타입이므로 구조체와 다르기때문에 인스턴스를 상수 let으로 선언해도 내부 프로퍼티 값을 변경할 수 있다

```swift
var yagom: Person = Person()
yagom.height = 123.4
yagom.weight = 123.4

let jenny: person = Person()
jenny.height = 123.4
jenny.weight = 123.4
```

<h3>9.2.3 클래스 인스턴스의 소멸</h3>
클래스의 인스턴스는 참조 타입이므로 더는 참조할 필요가 없을 때 메모리에서 해제됨 -> 이 과정을 소멸이라고 하는데 소멸되기 직전 denit이라는 메서드가 호출된다

denit 메서드는 클래스당 하나만 구현할 수 있으며 , 매개변수와 반환 값을 가질 수 없다

```swift
class Person {
  var height: Float = 0.0
  var weight: Float = 0.0
  
  denit {
    print("Person 클래스의 인스턴스가 소멸됩니다.")
  }
}

var yagom: Person? = Person()
yagom = nil // Person 클래스의 인스턴스가 소멸됩니다.
```

<h2>9.3 구조체와 클래스의 차이</h3>

차이점 : 구조체는 상속불가, 타입캐스팅은 클래스의 인스턴스에서만 허용, 디이니셜라이저는 클래스의 인스턴스에만 활용가능, 참조 횟수 계산은 클래스의 인스턴스에만 적용

가장 큰 차이점은 구조체는 '값' 타입 이고 클래스는 '참조' 타입임

<h3>9.3.1 값 타입과 참조 타입</h3>

값 타임의 값을 어떤 함수의 전달인자로 넘기면 전달될 값이 복사되어 전달됨 

그러나 참조 타입이 전달인자로 전달될 때는 값을 복사하지 않고 참조(주소)가 전달됨

```swift
// 구조체
struct BasicInformation {
  let name: String
  var age: Int
}

var yagomInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
yagomInfo.age = 100

// yagomInfo의 값을 복사하여 할당!
var friendInfo: BasicInformation = yagomInfo

print("yagom's age: \(yagomInfo.age)") // 100
print("friend's age: \(yagomInfo.age)") // 100

friend.age = 999

print("yagom's age: \(yagomInfo.age)") // 100 - yagom의 값은 변동 없음
print("friend's age: \(friendInfo.age)")
// 999 - friendInfo는 yagomInfo의 값을 복사해왔기 때문에 별개의 값을 갖는다

// 클래스
class Person {
  var height: Float = 0.0
  var weight: Float = 0.0
}

var yagom: Person = Person()
var friend: Person = yagom

print("yagom's height: \(yagom.height)") // 0.0
print("friend's height: \(friend.height)") // 0.0

friend.height = 185.5
print("yagom's height: \(yagom.height)") // 185.5  - friend는 yagom을 참조하기 때문에 값이 변동됨

print("friend's height: \(friend.height)") // 185.5 - 이를 통해 yagom이 참조하는 곳과 friend가 참조하는 곳이 같음을 알 수 있다

func changeBasicInfo(_ info: BasicInformation) {
  var copiedInfo: BasicInformation = info
  copiedInfo.age = 1
}

func chagnePersonInfo(_ info: Person) {
  info.height = 155.3
} // changeBaiscInfo(_:)로 전달되는 전달인자는 값이 복사되어 전달되기 때문에 yagomInfo의 값만 전달됨
changeBasicInfo(yagomInfo)
print("yagom's age: \(yagomInfo.age)") // 100

// changePersonInfo(_:)의 전달인자로 yagom의 참조가 전달되었기 때문에 yagom이 참조하는 값들에 변화가 생김
changePersonInfo(yagom)
print("yagom's height : \(yagom.height)") // 155.3
```


<h3>9.3.2 스위프트의 기본 데이터 타입은 모두 구조체</h3>

스위프트 String 타입의 정의
```swift
public struct String {
  /// An empty 'String'.
  public init()
}
```

<h2>9.4 구조체와 클래스 선택해서 사용하기</h2>

몇가지 상황을 제외하고는 클래스로 정의하여 사용, 대다수 사용자 정의 데이터 타입은 클래스로 구현할 일이 더 많음
