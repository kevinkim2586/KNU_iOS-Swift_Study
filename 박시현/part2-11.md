<h1>Chater 11. 인스턴스 생성 및 소멸</h1>

<h2>11.1 인스턴스 생성</h2>

클래스, 구조체, 열거형의 기본적인 형태의 이니셜라이저

```swift
class SomeClass {
  init() {
    // 초기화할 때 필요한 코드
  }
}

struct SomeStruct {
  init() {
    // 초기화할 때 필요한 코드
  }
}

enum SomeEnum {
  case someCase
  
  init() {
    // 열거형은 초기화할 때 반드시 case중 하나가 되어야 한다
    self = .someCase
    // 초기화할 때 필요한 코드
  }
}
```

<h3>11.1.1 프로퍼티 기본 값</h3>
구조체와 클래스의 인스턴스는 처음 생성할 때 옵셔널 저장 프로퍼티를 제외한 모든 저장 프로퍼티에 적절한 초깃값을 할당해야함 

프로퍼티를 정의할 때 프로퍼티 기본값을 할당하면 이니셜라이저에서 따로 초깃값을 할당하지 않더라도 프로퍼티 기본값으로 저장 프로퍼티의 값이 초기화됨

이니셜라이저를 통해 초기값 할당
```swift
struct Area {
  var squareMeter: Double
  
  init() {
    squareMeter = 0.0 // squqreMeter의 초깃값 할당
  }
}

let room: Area = Area()
print(room.squareMeter) // 0.0
프로퍼티 기본값 지정
```swift
struct Area {
  var squareMeter: Double = 0.0 // 프로퍼티 기본값 할당
}

let room: Area = Area()
print(room.squareMeter) // 0.0
```



<h3>11.1.2 이니셜라이저 매개변수</h3>

함수나 메서드를 정의할ㄷ 때와 마찬가지로 이니셜라이저도 매개변수를 가질 수 있다

```swift
struct Area {
  var squareMeter: Double
  
  init(fromPy py: Double) { // 첫번째 이니셜라이저
    squareMeter = py * 3.3058
  }
  
  init(fromSquareMeter squareMeter: Double) { // 두번째 이니셜라이저
    self.squareMeter = squareMeter
  }
  
  init(value: Double) { // 세번째 이니셜라이저
    squareMeter = value
  }
  
  init(_ value: Double) { // 네번쨰 이니셜라이저
    squareMeter = value
  }
}

let roomOne: Area = Area(fromPy: 15.0)
print(roomOne.squareMeter) // 49.587

let roomTwo: Area = Area(fromSquareMeter: 33.06)
print(roomTwo.squareMeter) // 33.06

let roomThree: Area = Area(value: 30.0)
let roomFour: Area = Area(55.0)

Area() // 오류발생
```

<h3>11.1.3 옵셔널 프로퍼티 타입</h3>

인스턴스가 사용되는 동안에 값을 꼭 갖고있지 않아도 되는 저장 프로퍼티가 있다면 해당 프로퍼티를 옵셔널로 선언가능
```swift
class Person {
  var name: String
  var age: Int? // 옵셔널 선언
  
  init(name: String) {
    self.name = name
  }
}

let yagom: Person = Person(name: "yagom")
print(yagom.name) // "yagom"

print(yagom.age) // nil

yagom.age = 99
print(yagom.age) // 99

yagom.name = "Eric"
print(yagom.name) // "Eric"
```

<h3>11.1.4 상수 프로퍼티</h3>
이름 프로퍼티를 상수가 아닌 변수로 선언하면 이름을 할당한 후 전혀 다른 사람으로 변할 수 있기 때문에 이름프로퍼티를 상수로 선언해야함

```swift
class Person {
  let name: String
  var age: Int?
  
  init(name: String) {
    self.name = name
  }
}

let yagom: Person = Person(name: yagom)
yagom.name = "Eric" // 오류 발생!!
```


<h3>11.1.5 기본 이니셜라이저와 멤머와이즈 이니셜라이저</h3>

구조체는 멤버와이즈 이니셜라이저를 기본으로 제공하나 클래스는 제공하지않음

구조체 선언과 멤버와이즈 이니셜라이저 사용
```swift
struct Point {
  var x: Double = 0.0
  var y: Double = 0.0
}

struct Size {
  var width: Double = 0.0
  var height: Double = 0.0
}

let point: Point = Point(x: 0, y: 0)
let size: Size = Size(width: 50.0, height: 50.0)

// 구조체의 저장 프로퍼티에 기본값이 있는경우 필요한 매개변수만 사용하여 초기화 할 수 있음
let somePoint: Point = Point()
let someSize: Size = Size(width: 50)
let anotherPoint: Point = Point(y: 100)
```


<h3>11.1.6 초기화 위임</h3>

초기화 위임 - 값 타입인 구조체와 열거형에서 코드의 중복을 피하기 위하여 이니셜라이저가 다른 이니셜라이저에게 일부 초기화를 위임

사용자 정의 이니셜라이저를 정의하면 기본 이니셜랄이저와 멤버와이즈 이니셜라이저를 사용할 수 없기 때문에 초기화 위임을 하려면 최소 두 개 이상의 사용자 정의 이니셜라이저를 정의해야함

```swift
enum Student {
  case elementary, middle, high
  case none
  // 사용자 정의 이니셔라이저가 있는 경우, init() 메서드를 구현해주어야 기본 이니셜라이저를 사용할 수 있다
  
  init() {
    self = .none
  }
  
  init(koreanAge: Int) { // 첫 번째 사용자 정의 이니셜라이저
    switch KoreanAge {
    case 8...13:
      self = .elementary
    case 14...16:
      self = .middle
    case 17...19:
      self = .high
    default:
      self = .none
    }
  }
  
  init(bornAt: Int, currentYear: Int) { // 두 번째 사용자 정의 이니셜라이저
    self.init(koreanAge: currentYear - bornAt + 1)
  }
}

var younger: Student = Student(koreanAge: 16)
print(younger) // middle

younger = Student(bornAt: 1998, currentYear: 2016)
print(younger) // high
```

<h3>11.1.7 실패 가능한 이니셜라이저</h3>

이니셜라이저의 전달인자로 잘못된 값이나 적절치 못한 값이 전달되었을 때, 이니셜라이저는 인스턴스 초기화에 실패할 수 있음 

이런 실패가능성을 내포한 이니셜라이저를 실패 가능한 이니셜라이저라고 부른다 실패가능한 이니셜라이저는 실패했을 때 nil을 반환해주므로 반환 타입이 옵셔널로 지정된다

```swift
class Person {
  let name: String
  var age: Int?
  
  init?(name: String) {
    
    if name.isEmpty {
      return nil
    }
    
    self.name = name
  }
  
  init?(name: String, age: Int) {
    if name.isEmpty || age < 0 {
      return nil
    }
    self.name = name
    self.age = age
  }
}
let yagom: Person? = Person(name: "yagom", age: 99)

if let person: Person = yagom {
  print(person.name)
}
else {
  print("Person wasn't initialized")
} // yagom

let chope: Person? = Person(name: "chope", age: -10)

if let person: Person = chope {
  print(person.name)
}
else {
  print("Person wasn't initialized")
} // Person wasn't initilized

let eric: Person? = Person(name: "", age: 30)

if let person: Person = eric {
  print(person.name)
}
else {
  print("Person wasn't initialized")
} // Person wasn't initialized
```

<h3>11.1.8 함수를 사용한 프로퍼티 기본값 설정</h3>

사용자 정의 연산을 통해 저장 프로퍼티 기본값을 설정하고자 한다면 클로저나 함수를 사용하여 프로퍼티 기본값을 제공할 수 있다 

인스턴스를 초기화할 때 함수나 클로저가 호출되면서 연산 결과값을 프로퍼티 기본값으로 제공해주기때문에 클로저나 함수의 반환 타입은 프로퍼티의 타입과 일치해야한다

```swift
struct Student {
  var name: String?
  var number: Int?
}

class SchoolClass {
  var students: [Student] = { 
    // 새로운 인스턴스를 생성하고 사용자 정의 연산을 통한 후 반환해준다. 반환되는 값의 타입은 [Student] 타입이어야 한다
    var arr: [Student] = [Student]()
    
    for num in 1...15 {
      var student: Student = Student(name: nil, number: num)
      arr.append(student)
    }
    
    return arr
  }()
}

let myclass: SchoolClas = SchoolClass()
print(myClass.students.count) // 15
```

<h2>11.2 인스턴스 소멸</h2>

디이니셜라이저는 클래스의 인스턴스가 메모리에서 소멸되기 바로 직전에 호출됨 , 디이니셜라이저는 클래스의 인스턴스에만 구현할 수 있다

클래스에는 디이니셜라이저를 단 하나만 구현할 수 있다 , 디이니셜라이저는 인스턴스를 소멸하기 직전에 호출되므로 인스턴스의 모든 프로퍼티에 접근 할 수 있다

```swift
class SomeClass {
    deinit {
      print("Instance will be deallocated immediately")
    }
}

var instance: SomeClass? = SomeClass()
instance = nil // Instance will be deallocated immediately
```
