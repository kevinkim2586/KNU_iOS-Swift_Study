<h1>Chapter 17. 서브스크립트</h1>

서브스크립트 - 클래스, 구조체, 열거형에서 컬렉션, 리스트, 시퀀스 등 타입의 요소에 접근하는 단축 문법

<h2>17.1 서브스크립트 문법</h2>

```swift
subscript(index: Int) -> Int {
  get {
    // 적절한 서브스크립트 결과값 반환
  }
  
  set(newvalue) {
    // 적절한 설정자 역할 수행
  }
}
```

```swift
// 읽기전용
subscript(index: Int) -> Int {
  get {
    // 적절한 서브스크립트 값 반환
  }
}
subscript(index: Int) -> Int {
  // 적절한 서브스크립트 결괏값 반환
}

get 메서드 없이 단순히 값만 반환하도록 구현하면 읽기 전용이 된다
```


<h2>17.2 서브스크립트 구현</h2>

```swift
struct Student {
  var name: String
  var number: Int
}

class School {
  var number: Int = 0
  var students: [Student] = [Student]()
  
  func addStudent(name: String) {
    let student: Student = Student(name: name, number: self.number)
    self.students.append(student)
    self.number += 1
  }
  
  func addStudents(names: String...) {
    for name in names {
      self.addStudent(name: name)
    }
  }
  
  subscript(index: Int = 0) -> Student? {
    if index < self.number {
      return self.students[index]
    }
    return nil
  }
}

let highSchool: School = School()
highSchool.addStudents(names: "Mijeong", "JuHyun", "JiYoung", "SeongUk", "MoonDuk")

let aStudent: Student? = highSchool[1]
print("\(aStudent?.number) \(aStudent?.name)") // Optional(1)
Optional("JuHyun")
print(highSchool[]?.name) // 매개변수 기본값 사용 : Optional("Mijeong")
```

<h2>17.3 복수 서브스크립트</h2>

<h2>17.4 타입 서브스크립트</h2>

타입 자체에서 사용할 수 있는 서브스크립트

```swift
enum School: Int {
  case elementary = 1, middle, high, university
  
  static subscript(level: Int) -> School? {
    return Self(rawValue: level)
    // return School(rawValue: level)과 같른 표현이다
  }
}

let school: School? = School[2]
print(school) // School.middle
```
