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
