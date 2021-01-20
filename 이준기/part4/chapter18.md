# Chapter18 상속

- 클래스는 메서드나 프로퍼티 등을 다른 클래스로부터 상속 받을수 있음
- 상속 받은 클래스는 어떤 클래스의 자식 클래스라고 표현
- 자식클래스에게 자신의 특성을 물려준 클래스를 부모 클래스라고 표현
- 다른 클래스로부터 상속을 받지 않을 클래스를 기반 클래스라고 한다.

## 18.1 클래스 상속

```swift
class 클래스이름: 부모클래스 이름 {
	프로터피와 메서드들
}
```

## 18.2 재정의

- 부모 클래스에서 물러 받은 특성을 그대로 사용하지 않고 자신만의 기능으로 변경하는 것
- 재정의 하려면 override라는 키워드를 사용
- 해당 하는 특성이 없는데 override를 사용하면 컴파일 오류가 발생함
- 부모클래스의 특성을 자식 클래스에서 사용하고싶다면 super 프로퍼티를 사용하면됨

### 18.2.1 매서드 재정의

- 부모클래스로부터 상속받은 인스턴스 메서드나 타입 메서드를 자식 클래스에서 재정의 할수 있음

```swift
class Person {
    var name: String = ""
    var age: Int = 0
    
    var introduction: String {
        return "이름 : \(name). 나이 : \(age)"
    }
    
    func speak() {
        print("가나다라마바사")
    }
    
    class func introduceClass() -> String {
        return "인류의 소원은 평화입니다."
    }
}

class Student: Person {
    var grade: String = "F"
    
    func study() {
        print("Study hard...")
    }
    
    override func speak() {
        print("저는 학생입니다.")
    }
}

class UniversityStudent: Student {
    var major: String = ""

    class func introduceClass() {
        print(super.introduceClass())
    }
    
    override class func introduceClass() -> String {
        return "대학생의 소원은 A+입니다."
    }
    
    override func speak() {
        super.speak()
        print("대학생이죠.")
    }
}

let yagom: Person = Person()
yagom.speak()   // 가나다라마바사

let jay: Student = Student()
jay.speak()   // 저는 학생입니다.

let jenny: UniversityStudent = UniversityStudent()
jenny.speak()   // 저는 학생입니다. 대학생이죠.

print(Person.introduceClass())      // 인류의 소원은 평화입니다.
print(Student.introduceClass())     // 인류의 소원은 평화입니다.
print(UniversityStudent.introduceClass() as String)     // 대학생의 소원은 A+입니다.
UniversityStudent.introduceClass() as Void      // 인류의 소원은 평화입니다.
```

### 18.2.2 프로퍼티 재정의

- 매서드와 마찬가지로 재정의 가능
- 그러나 재정의시 저장 프로퍼티로의 재정의는 불가능
-> 프로퍼티의 재정의 뜻 : getter, setter, property observer 등을 재정의한다는 것

```swift
import Swift

// 코드 18-5 프로퍼티 재정의
class Person {
    var name: String = ""
    var age: Int = 0
    var koreanAge: Int {
        return self.age + 1
    }
    
    var introduction: String {
        return "이름 : \(name). 나이 : \(age)"
    }
}

class Student: Person {
    var grade: String = "F"
    
    override var introduction: String {
        return super.introduction + " " + "학점 : \(self.grade)"
    }
    
    override var koreanAge: Int {
        get {
            return super.koreanAge
        }
        
        set {
            self.age = newValue - 1
        }
    }
}

let yagom: Person = Person()
yagom.name = "yagom"
yagom.age = 55
// yagom.koreanAge = 56 // 오류 발생
print(yagom.introduction)   // 이름 : yagom. 나이 : 55
print(yagom.koreanAge)  // 56

let jay: Student = Student()
jay.name = "jay"
jay.age = 14
jay.koreanAge = 15
print(jay.introduction) // 이름 : jay. 나이 : 14 학점 : F
print(jay.koreanAge)    // 15
```

### 18.2.3 프로퍼티 감시자 재정의

- 상수 저장이나 읽기 전용 연산 프로퍼티는 프로퍼티 감시자 재정의 x
- 조상 클래스에 정의한 프로퍼티 감시자도 동작
- 만약 둘다 동작하길 원한다면 재정의하는 접근자에 프로퍼티 감시자의 역할을 구현해야함

```swift
class Person {
    var name: String = ""
    var age: Int = 0 {
        didSet {
            print("Person age : \(self.age)")
        }
    }
    var koreanAge: Int {
        return self.age + 1
    }
    
    var fullName: String {
        get {
            return self.name
        }
        
        set {
            self.name = newValue
        }
    }
}

class Student: Person {
    var grade: String = "F"
    
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
        
        // didSet {  }  // 오류 발생!!
    }
    
    override var fullName: String {
        didSet {
            print("Full Name : \(self.fullName)")
        }
    }
}

let yagom: Person = Person()
yagom.name = "yagom"
yagom.age = 55  // Person age : 55
yagom.fullName = "Jo yagom"
print(yagom.koreanAge)  // 56


let jay: Student = Student()
jay.name = "jay"
jay.age = 14
// Person age : 14
// Student age : 14
jay.koreanAge = 15
// Person age : 14
// Student age : 14
jay.fullName = "Kim jay"    // Full Name : Kim jay
print(jay.koreanAge)    // 15
```

### 18.2.4 서브스크립트 재정의

- 자식 클래스와 부모 클래스의 매개변수와 반환타입을 맞춰줘야함

```swift
class Person {
    var name: String = ""
    var age: Int = 0 {
        didSet {
            print("Person age : \(self.age)")
        }
    }
    var koreanAge: Int {
        return self.age + 1
    }
    
    var fullName: String {
        get {
            return self.name
        }
        
        set {
            self.name = newValue
        }
    }
}

class Student: Person {
    var grade: String = "F"
    
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
        
        // didSet {  }  // Error!!
    }
    
    override var fullName: String {
        didSet {
            print("Full Name : \(self.fullName)")
        }
    }
}

// 코드 18-7 서브스크립트 재정의
class School {
    var students: [Student] = [Student]()
    
    subscript(number: Int) -> Student {
        print("School subscript")
        return students[number]
    }
}

class MiddleSchool: School {
    var middleStudents: [Student] = [Student]()

    // 부모클래스(School)에게 상속받은 서브스크립트 재정의
    override subscript(index: Int) -> Student {
        print("MiddleSchool subscript")
        return middleStudents[index]
    }
}

let university: School = School()
university.students.append(Student())
university[0]   // School subscript

let middle: MiddleSchool = MiddleSchool()
middle.middleStudents.append(Student())
middle[0]   // MiddleSchool subscript
```

### 18.2.5 재정의 방지

- 재정의를 방지할려면 특성앞에 final 이라는 키워드를 붙임

```swift
class Person {
    final var name: String = ""
    
    final func speak() {
        print("가나다라마바사")
    }
}

final class Student: Person {
    // 오류!
    // Person의 name은 final을 사용하여
    // 재정의를 할 수 없도록 했기 때문입니다.
    override var name: String {
        set {
            super.name = newValue
        }
        get {
            return "학생"
        }
    }
    
    // 오류! 
    // Person의 speak()는 final을 사용하여
    // 재정의를 할 수 없도록 했기 때문입니다.
    override func speak() {
        print("저는 학생입니다.")
    }
}

// 오류!
// Student 클래스는 final을 사용하여
// 상속할 수 없도록 했기 때문입니다.
class UniversityStudent: Student { }
```

## 18.3 클래스의 이니설라이저 - 상속과 재정의


### 18.3.1 지정 이니셜라이저와 편의 이니설라이저

- 모든 클래스는 하나 이상의 지정 이니설라이즈가 있음

```swift
init(매개변수들){
	초기화 구문
}
```

- 편의 이니설라이저는 초기화를 좀더 쉽게 도와주는 역할

```swift
convenience init(매개변수들){
	초기화 구문
}
```

### 18.3.2 클래스의 초기화 위임
1. 자식클래스의 지정 이니설라이저는 부모클래스의 지정 이니설라이저를 반드시 호출
2. 편의이니설라이저는 자신을 정의한 클래스의 다른 이니설라이저를 반드시 호출해야함
3. 편의 이니설라이저는 궁극적으로는 지정 이니설라이저를 반드시 호출해애함


### 18.3.3 2단계 초기화

- 스위프트의 클래스 초기화는 2단계를 거침
- 1단계는 클래스에 정의한 각각의 저장 프로퍼티에 초깃값이 할당됨
- 2단계는 저장 프로퍼티들을 사용자 정의할 기회를 얻음
- 2단계 초기화때 프로퍼티 값에 접근하는 것을 막아 초기화를 안전하게 할수 있도록 해줌

### 18.3.4 이니설라이저 상속 및 재정의

- 기본적으로 이니설라이저는 부모클래스의 이니설라이즈를 상속 받지 않는다
- 부모 클래스와 동일한 지정 이니설라이저를 자식클래스의 구현해주려면 재정의를 해야함
- 부모 클래스의 편의 이니설라이즈와 동알한 이니설라이즈를 자식 클래스에 구현할떄는 재정의 하지 않음

```swift
class Person {
    var name: String
    var age: Int
    
    init(name: String, age: Int) {
        self.name = name
        self.age = age
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 0)
    }
}

class Student: Person {
    var major: String

    override init(name: String, age: Int) {
        self.major = "Swift"
        super.init(name: name, age: age)
    }
    
    convenience init(name: String) {
        self.init(name: name, age: 7)
    }
}
```

### 18.3.5 이니설라이저 자동 상속

- 대부분의 경우 자식 클래스에서 이니설라이저를 재정의 할 필요 없음
- 자식 클래스에서 지정하지 않는 다면 부모클래스에서 지정 이니설라이즈가 자동 상속됨

```swift
class Person {
    var name: String
    
    init(name: String) {
        self.name = name
    }
    
    convenience init() {
        self.init(name: "Unknown")
    }
}

class Student: Person {
    var major: String = "Swift"
}

// 부모클래스의 지정 이니셜라이저 자동 상속
let yagom: Person = Person(name: "yagom")
let hana: Student = Student(name: "hana")
print(yagom.name)   // yagom
print(hana.name)    // hana

// 부모클래스의 편의 이니셜라이저 자동 상속
let wizplan: Person = Person()
let jinSung: Student = Student()
print(wizplan.name)     // Unknown
print(jinSung.name)     // Unknown
```

### 18.3.6 요구 이니설라이저

- required 수식어를 사용하여 정의
- 클래스를 상속받은 자식 클래스에서 반드시 해당 이니설라이저 구현해야

```swift
class Person {
    var name: String
    
    // 요구 이니셜라이저 정의
    required init() {
        self.name = "Unknown"
    }
}

class Student: Person {
    var major: String = "Unknown"

    // 자신의 지정 이니셜라이저 구현
    init(major: String) {
        self.major = major
        super.init()
    }
    
    required init() {
        self.major = "Unknwon"
        super.init()
    }
}

class UniversityStudent: Student {
    var grade: String
    
    // 자신의 지정 이니셜라이저 구현
    init(grade: String) {
        self.grade = grade
        super.init()
    }

    required init() {
        self.grade = "F"
        super.init()
    }
}

let jiSoo: Student = Student()
print(jiSoo.major)      // Unknwon

let yagom: Student = Student(major: "Swift")
print(yagom.major)      // Swift

let juHyun: UniversityStudent = UniversityStudent(grade: "A+")
print(juHyun.grade)     // A+
```
