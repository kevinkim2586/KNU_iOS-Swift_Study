# Chapter18 상속

> 클래스는 메서드나 프로퍼티 등을 다른 클래스로부터 **상속**받을 수 있다  
> **자식클래스**: 어떤 클래스로부터 상속을 받은 클래스  
> **부모클래스**: 자식클래스에게 자신의 특성을 물려준 클래스  

## 18.1 상속

> 클래스 이름 뒤에 콜론을 붙이고 다른 클래스 이름을 써주면 뒤에 오는 클래스로부터 상속받는 클래스가 된다  

~~~ swift
class Person {
    var name: String = ""
    var age: Int = 0
    
    var introduction: String {
        return "이름 : \(name). 나이 : \(age)"
    }
    
    func speak() {
        print("가나다라마바사")
    }
}

class Student: Person {
    var grade: String = "F"
    
    func study() {
        print("Study hard...")
    }
}
~~~

> Person 클래스를 상속받은 Student 클래스는 Person의 인스턴스 메서드, 타입 메서드, 인스턴스 프로퍼티, 타입 프로퍼티, 서브스크립트 등 모든 특성을 포함한다  
> 원래 클래스로부터 기능 확장할 때 사용  

## 18.2 재정의

> 자식클래스는 부모클래스로부터 물려받은 특성을 그대로 사용하지 않고 자신만의기능으로 변경하여 사용할 수 있다. -> Override(재정의)  
> 자식클래스에서 부모클래스의 특성을 자식클래스에서 사용하고 싶다면 **super** 키워드 사용(클래스 정의부분 내에서만 사용가능한 키워드)  

### 18.2.1 메서드 재정의

~~~ swift
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
~~~

### 18.2.2 프로퍼티 재정의

> 프로퍼티 자체를 재정의 한다는 뜻이 아님  
> 프로퍼티 접근자(Getter), 설정자(Setter), 감시자를 재정의한다는 것을 의미  

> 저장 프로퍼티, 연산 프로퍼티 모두 접근자와 설정자를 재정의 할 수 있음  
> 읽기 전용 프로퍼티 -> 읽고 쓰기가 가능한 프로퍼티 재정의 가능 O  
> 읽고 쓰기가 모두 가능한 프로퍼티 -> 읽기 전용 프로퍼티 재정의 불가 X  

~~~ swift
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
        // 설정자만 따로 재정의 할 수 없다.
        // 접근자에 따로 기능 변경이 필요 없다면 아래처럼 super을 사용해서
        // 부모클래스의 접근자를 그대로 사용 가능하다
        get {
            return super.koreanAge
        }
        
        set {
            self.age = newValue - 1
        }
    }
}
~~~

### 18.2.3 프로퍼티 감시자 재정의

> *프로퍼티 감시자 재정의 할 떄*, 조상클래스의 프로퍼티가 연산 프로퍼티인지 저장 프로퍼티인지 상관 X  
> 상수 저장 프로퍼티, 읽기 전용 연산 프로퍼티는 프로퍼티 감시자 재정의 X  
> 프로퍼티 접근자와 프로퍼티 감시자는 동시에 재정의 X  
> -> 둘 다 동작하길 원하면 재정의하는 접근자에 프로퍼티 감시자의 역할을 구현해야  

~~~ swift
struct Coordinate {
    var x: Int
    var y: Int
}

class Person {
    var name: String = ""
    var x: Int = 0
    var y: Int = 0
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
    
    var coordinate: Coordinate {
        get {
            print("hahaha!")
            let xy: Coordinate = Coordinate(x: self.x, y: self.y)
            return xy
        }
        
        set {
            print("setting...")
            self.x = newValue.x
            self.y = newValue.y
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
            print("\(oldValue), Full Name : \(self.fullName)")
        }
    }
    
    override var coordinate: Coordinate {
        willSet {
            print(" to \(newValue)")
        }
        
        didSet {
            print(" from \(oldValue)")
        }
    }
}

let a: Student = Student()

a.x = 10
a.y = 20
print(a.coordinate)

print("----")

a.coordinate = Coordinate(x: 40, y: 50)
~~~

### 18.2.4 서브스크립트 재정의

> 매개변수와 반환타입이 다르면 다른 서브스크립트로 취급하므로, 재정의하려면 배개변수와 반환 타입을 갖게 해야한다  
> 메서드 재정의와 방법이 같다  

~~~ swift
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
~~~

### 18.2.5 재정의 방지

> **final** 키워드 사용  
> 몇몇 특성을 재정의 할 수 없도록 제한하고 싶을때

## 18.3 클래스의 이니셜라이저 - 상속과 재정의

### 18.3.1 지정 이니셜라이저와 편의 이니셜라이저

> - 지정 이니셜라이저(Designated Initializer)
>     - 클래스의 주요 이니셜라이저
>     - 이니셜라이저가 정의된 클래스의 모든 프로퍼티를 초기화해야 하는 임무 갖고 있음
>     - 모든 클래스에는 하나 이상의 지정 이니셜라이저를 갖는다

> - 편의 이니셜라이저(Convenience Initializer)
>     - 초기화를 좀 더 손쉽게 도와주는 역할
>     - 지정 이니셜라이저를 자신 내부에서 호출
>     - 필수 요소는 아님

### 18.3.2 클래스의 초기화 위임

세 가지 규칙
> 1 자식클래스의 지정 이니셜라이저는 부모클래스의 지정 이니셜라이저를 반드시 호출해야 한다  
> 2 편의 이니셜라이저는 자신을 정의한 클래스의 다른 이니셜라이저를 반드시 호출해야 한다  
> 3 편의 이니셜라이저는 궁극적으로는 지정 이니셜라이저를 반드시 호출해야 한다

### 18.3.3 2단계 초기화

**\*추후 수정\***

### 18.3.4 이니셜라이저 상속 및 재정의

> 기본적으로 스위프트의 이니셜라이저는 부모클래스의 이니셜라이저를 상속받지 않는다  
> 부모클래스와 동일한 지정 이니셜라이저를 자식클래스에서 구현해주려면 재정의(override 키워드)해주면 된다  
> 부모클래스의 편의 이니셜라이저와 동일한 이니셜라이저를 자식클래스에서 구현할 때는 override 키워드를 붙이지 않는다  
> -> 자식클래스에서 부모클래스의 편의 이니셜라이저는 절대로 호출할 수 없기 때문

### 18.3.5 이니셜라이저 자동 상속

> 대부분의 경우 자식클래스에서 이니셜라이저를 재정의 해 줄 필요가 없다  

**자식 클래스에서 프로퍼티 기본값을 모두 제공한다고 가정할 때** 다음 규칙에 따라 자동으로 이니셜라이저가 상속된다.
- 규칙1: 자식클래스에서 별도의 지정 이니셜라이저를 구현하지 않는다면, 부모클래스의 지정 이니셜라이저가 자동으로 상속된다
- 규칙2: 만약 규칙1에 따라 자식클래스에서 부모클래스의 지정 이니셜라이저를 자동으로 상속받은 경우 또는 부모클래스의 지정 이니셜라이저를 모두 재정의하여 부모클래스와 동일한 지정 이니셜라이저를 모두 사용할 수 있는 상황이라면 부모클래스의 편의 이니셜라이저가 모두 자동으로 상속된다

~~~ swift
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
~~~

### 18.3.6 요구 이니셜라이저

> required 키워드를 클래스의 이니셜라이저 앞에 명시해주면 이 클래스를 상속받은 자식클래스에서 반드시 해당 이니셜라이저를 구현해주어야 한다

~~~ swift
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
~~~
