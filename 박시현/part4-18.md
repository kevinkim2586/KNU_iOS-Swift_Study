<h1>Chapter 18. 상속</h1>

<h2>18.1 클래스 상속</h2>

상속은 기반클래스를 다른 클래스에서 물려받는 것임 부모클래스의 메서드, 프로퍼티 등을 재정의하거나,

기반클래스의 기능이나 프로퍼티를 물려받고 자신의 기능을 추가할 수 있음

```swift
기반클래스 Person
class Person {
  var name: String = " "
  var age: Int = 0
  
  var introduction: String {
    return "이름 : \(name). 나이 : \(age)"
  }
  
  func speak() {
    print("가나다라마바사")
  }
}

let yagom: Person = Person()
yagom.name = "yagom"
yagom.age = 99
print(yagom.introduction) // 이름 : yagom. 나이 : 99
yagom.speak() // 가나다라마바사
```
```swift
Person 클래스를 상속받은 Student 클래스
class student: Person {
  var grade: String = "f"
  
  func study() {
    print("Study hard...")
  }
}

let yagom: Person = Person()
yagom.name = "yagom"
yagom.age = 99
print(yagom.introduction) // 이름 : yagom. 나이 : 99
yagom.speak() // 가나다라마바사

let jay : Student = Student()
jay.name = "jay"
jay.age = 10
jay.grade = "A"
print(jay.introduction) // 이름 : Jay. 나이 : 10
jay.speak() // 가나다라마바사
jay.study() // Study hard...
```

<h2>18.2 재정의</h2>
재정의(override) - 자식클래스가 부모클래스로부터 물려받은 특성(인스턴스 메서드, 타입 메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 서브스크립트 등)

을 그대로 사용하지 않고, 자신만의 기능으로 변경하여 사용할 수 있는것

<h3>18.2.1 메서드 재정의</h3>

```swift
메서드 재정의
class Person {
  var name: String = " "
  var age: Int = 0
  
  var introduction: String {
    return "이름 : \(name). 나이 : \(age)"
  }
  
  func speak() {
    print("가나다라마바사")
  }
  
  class func introduceClass() -> String {
    return "인류의 소원은 평화입니다"
  }

class student: Person {
  var grade: String = "F"
  
  func study() {
    print("Study hard...")
  }
  
  override func speak() {
    print("저는 학생입니다.")
  }
}

class UniversityStudent: Student {
  var major: String = " "
  
  class func introduceClass() {
    print(super.introduceClass())
  }
  
  override class func introduceClass() -> String {
    return "대학생의 소원은 A+ 입니다."
  }
  
  override func speak() {
    super.speak()
    print("대학생이죠.")
  }
}

let yagom: Person = Person()
yagom.speak() // 가나다라마바사

let jay: Student = Student()
jay.speak() // 저는 학생입니다.

let jenny: UniversityStudent = UniversityStudent()
jenny.speak() // 저는 학생입니다. 대학생이죠.

print(Person.introduceClass()) // 인류의 소원은 평화입니다.
print(Student.introduceClass()) // 인류의 소원은 평화입니다.
print(UniversityStudent.introduceClass() as String) // 대학생의 소원은 A+ 입니다.
UniversityStudent.introduceClass() as Void // 인류의 소원은 평화입니다.
```

<h3>18.2.2 프로퍼티 재정의</h3>

메서드 재정의와 같다 

읽기 쓰기가 모두 가능한 프로퍼티를 재정의 할 때  설정자만 따로 재정의 할수는 없고

접근자와 설정자를 모두 재정의해야한다. 접근자에 따로 기능 변경이 필요 없다면 부모클래스의 접근자를 사용하여 값을 받아와

반환해야한다 (super사용)

```swift
override var koreanAge: Int {
  get {
    return super.koreanAge
  }
  
  set {
    self.age = newValue - 1
  }
```

<h3>18.2.3 프로퍼티 감시자 재정의</h3>

상수 저장 프로퍼티나 읽기 전용 연산 프로퍼티는 프로퍼티 감시자를 재정의 할 수 없다

```swift
override var age: Int {
  didSet {
    print("Student age : \(self.age)")
  }
}

override var koreanAge: Int {
  get {
    return super.koreanAge
  }
  
  set {
    self.age = newValue - 1
  }
  
  didSet { } // 오류 발생!
}

override var fullName: String {
  didSet {
    print("Full Name : \(self.fullName)")
  }
}
```

<h3>18.2.4 서브스크립트 재정의</h3>

자식클래스에서 재정의 하려는 서브스크립트라면 부모클래스 서브스크립트의 매개변수와 반환 타입이 같아야 함

```swift
override subscript(index: Int) -> Student {
  print("MiddleSchool subscript")
  return middleStudents[index]
}
```

<h3>18.2.5 재정의 방지</h3>

재정의를 제한하고 싶다면 재정의를 방지하고 싶은 특성 앞에 final 키워드를 명시하면 됨

```swift
class Person {
  final var name: String = " "
  
  final func speak() {
    print("가나다라마바사")
  }
}

final class Student: Person {
  override var name: String { // 오류 , Person의 name은 final을 사용하여 재정의를 할 수 없도록 했기 때문
 }
 
 class universityStudent: Student {} // 오류 , Student 클래스는 final을 사용하여 상속할 수 없도록 했기 때문
 ```
 
 <h2>18.3 클래스의 이니셜라이저 - 상속과 재정의</h2>
 
 <h3>18.3.1 지정 이니셜라이저와 편의 이니셜라이저</h3>
 
 지정 이니셜라이저 - 필요에 따라 부모클래스의 이니셜랄이저를 호출할 수 있으며, 이니셜라이저가 정의된 클래스의 모든 프로퍼티를 초기화해야 하는 임무를 갖고 있음
 
 편의 이니셜라이저 - 초기화를 좀 더 손쉽게 도와주는 역할. 지정 이니셜라이저를 자신 내부에서 호출함 (앞에 convenience 지정자를 init 키워드 앞에 명시해주면됨)
 
 ```swift
 init(매개변수들) {
  초기화 구문
 }
 ```
 
 ```swift
 convenience init(매개변수들) {
  초기화 구문
 }
 ```
 
 <h3>18.3.2 클래스의 초기화 위임</h3>
 
 지정 이니셜라이저와 편의 이니셜라이저의 관계
 
 * 자식클래스의 지정 이니셜라이저는 부모클래스의 지정 이니셜라이저를 반드시 호출해야함
 
 * 편의 이니셜라이저는 자신을 정의한 클래스의 다른 이니셜라이저를 반드시 호출해야함
 
 * 편의 이니셜라이저는 궁극적으로는 지정 이니셜라이저를 반드시 호출해야함
 
 <h3>18.3.3 2단계 초기화</h3>
 
 클래스 초기화는 2단계를 거침 
 
 2단계 초기화를 오류 없이 처리하기 위해 다음과 같은 네 가지 안전확인을 실행
 
 1. 자식클래스의 지정 이니셜라이저가 부모클래스의 이니셜라이저를 호출하기 전에 자신의 프로퍼티를 모두 초기화했는지 확인
 
 2. 자식클래스의 지정 이니셜라이저는 상속받은 프로퍼티에 값을 하랑하기 전에 반드시 부모클래스의 이니셜라이저를 호출
 
 3. 편의 이니셜라이저는 자신의 클래스에 정의한 프로퍼티를 포함하여 그 어떤 프로퍼티라도 값을 할당하기 전에 다른 이니셜라이저를 호출해야 함
 
 4. 초기화 1단계를 마치기 전까지는 이니셜라이저는 인스턴스 메서드를 호출할 수 없음. 또, 인스턴스 프로퍼티의 값을 읽어들일 수도 없음. self 프로퍼티를 자신의 인스턴스를 나타내는 값으로 활용할 수도 없음.
 
 * 1단계 
 1. 클래스가 지정 또는 편의 이니셜라이저를 호출함
 
 2. 클래스의 새로운 인스턴스를 위한 메모리가 할당됨, 메모리는 아직 초기화되지 않은 상태
 
 3. 지정 이니셜라이저는 클래스에 정의된 모든 저장 프로퍼티에 값이 있는지 확인. 현재 클래스 부분까지의 저장 프로퍼티를 위한 메모리는 이제 초기화됨
 
 4. 지정 이니셜라이저는 부모클래스의 이니셜라이저가 같은 동작을 행할 수 있도록 초기화를 양도
 
 5. 부모클래스는 상속 체인을 따라 최상위 클래스에 도달할 때까지 이 작업을 반복
 
 * 2단계
 
 1. 최상위 클래스로부터 최하위 클래스까지 상속 체인을 따라 내려오면서 지정 이니셜라이저들이 인스턴스를 제각각 사용자 정의하게 된다. 이 단계에서는 self를 통해 프로퍼티 값을 수정할 수 있고, 인스턴스 메서드를 호출하는 등의 작업을 진행할 수 있다
 
 2. 마지막으로 각각의 편의 이니셜라이저를 통해 self를 통한 사용자 정의 작업을 진행할 수 있다.
 
```swift
class Person {
  var name: String
  var age: Int
  
  init(name: String, age: Int) {
    self.name = name
    self.age = age
  }
}

class Student: Person {
  var major: String
  
  init(name: String, age: Int, major: String) {
    self.major = "Swift" // 안전확인 중 1번 만족
    super.init(name: name, age: age) // 안전확인 중 2번 만족
  }

  convenience init(name: String) { // 안전확인 중 3번 만족
    self.init(name: name, age: 7, major: " ")
  }
} // 이니셜라이저 어디에서도 인스턴스 메서드를 호출하거나 프로퍼티 값을 읽어오지 않았으므로 안전확인 중 4번 만족
```
 
 <h3>18.3.4 이니셜라이저 상속 및 재정의</h3>
 
 ```swift
 override init(name: String, age: Int) {
  self.major = "Swift"
  super.init(name: name, age: age)
 }
 
 convenience init(name: String) {
  self.init(name: name, age: 7)
 } // 편의 이니셜라이저 재정의에서는 override 수식어를 붙이지 않음
 ```
 
 ```swift
 // 실패 가능한 이니셜라이저의 재정의
 override init?(name: String, age: Int) {
  self.major = "Swift"
  super.init(name: name, age: age)
 }
 
 override init(age: Int) {
  self.major = "Swift"
  super.init()
 }
 ```
 
 <h3>18.3.5 이니셜라이저 자동 상속</h3>
 
 자식클래스에서 프로퍼티 기본값을 모두 제공한다고 가정할 때, 두 가지 규칙에 따라 이니셜라이저가 자동으로 상속됨
 
 규칙1. 자식클래스에서 별도의 지정 이니셜라이저를 구현하지 않는다면, 부모클래스의 지정 이니셜라이저가 자동으로 상속됨
 
 규칙2. 만약 규칙1에 따라 자식클래스에서 부모클래스의 지정 이니셜라이저를 자동으로 상속받은 경우 또는 부모클래스의 지정 이니셜라이저를 모두 재정의하여 부모클래스와 동일한 지정 이니셜라이저를 모두 사용 할 수 있는 상황이라면 부모클래스의 편의 이니셜라이저가 모두 자동으로 상속됨
 
 
 <h3>18.3.6 요구 이니셜라이저</h3>
 
 required 수식어를 클래스의 이니셜라이저 앞에 명시해주면 이 클래스를 상속받은 자식클래스에서 반드시 해당 이니셜라이저를 구현해주어야 함
