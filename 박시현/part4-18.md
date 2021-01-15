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
  
