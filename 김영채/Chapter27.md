# Chapter 27 : ARC
- Automatic Reference Counting
- 참조 타입 → 메모리 해제는 중요한 문제

    → 인스턴스가 적절한 시점에 메모리에서 해제되지 않으면 메모리 자원 낭비 → 성능 저하

- 스위프트는 메모리 사용을 관리하기 위해 메모리 관리 기법인 ARC를 사용
- Reference Counting → 참조 횟수 계산

** Reference Counting 은 참조 타입인 ***클래스의 인스턴스***에만 적용

→ 구조체, 열거형은 값 타임이므로 적용 불가

### ARC 란

- 자동으로 메모리 관리해주는 방식
- 프로그래머가 메모리 관리에 신경을 덜 쓸 수 있어 편함
- 더 이상 필요하지 않은 클래스 인스턴스를 메모리에서 해제

→ Java 의 Garbage Collection 기법과 어떤 차이?

ARC는 **컴파일 타임**(compile time) 에 기존에 개발자가 직접 코드를 작성해야 되던 부분을 자동으로 구문을 분석해서 적절하게 레퍼런스 감소 코드삽입해 주어, 실행 중에 별도의 메모리 관리가 이루어 지지 않는다

**가비지 컬렉션**( `GC`, `Garbage Collection`)은 프로그램 실행 중(Runtime)에 동적으로 감시하고 있다가, 더 이상 사용할 필요가 없다고 여겨지는 것을 소멸(해제) 시켜버린다.

** 즉, ARC는 컴파일과 동시에 인스턴스를 메모리에서 해제하는 시점이 아예 결정되어 버린다.

- 클래스의 인스턴스를 생성할 때마다 ARC는 그 인스턴스에 대한 정보를 저장하기 위한 메모리 공간을 따로 또 할당한다. → 타입 정보, 저장 프로퍼티 등 저장
- 인스턴스가 계속 필요할 수도 있으니 ARC는 인스턴스가 메모리에서 해제되지 않도록 인스턴스 참조 여부를 계속 추적한다. 즉, 메모리에서 유지시키려면 명분을 계속 제공해야 함 → 규칙 필요

### 강한 참조 (Strong Reference)

- 인스턴스는 참조 횟수가 0이 되는 순간 메모리에서 해제가 된다
- 인스턴스를 다른 인스턴스의 프로퍼티나 변수 상수 등에 할당할 때 강한참조를 사용하면 참조 횟수가 1 증가
- 강한참조를 사용하는 프로퍼티, 변수, 상수 등에 nil 할당 시 참조 횟수 1 감소
- 참조의 기본은 강한 참조

```swift
class Person {
    
    let name: String
    
    init(name: String) {
        self.name = name
        print("\(name) is being initialized")
    }
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

var reference1: Person?
var reference2: Person?
var reference3: Person?

reference1 = Person(name: "yagom")
// yagom is being initialized
// 인스턴스의 참조 횟수 : 1

reference2 = reference1 // 인스턴스의 참조 횟수 : 2
reference3 = reference1 // 인스턴스의 참조 횟수 : 3

reference3 = nil    // 인스턴스의 참조 횟수 : 2
reference2 = nil    // 인스턴스의 참조 횟수 : 1
reference1 = nil    // 인스턴스의 참조 횟수 : 0
// yagom is being deinitialized
```

→ 참조 횟수가 0이 되는 순간 인스턴스는 ARC의 규칙에 의해 메모리에서 해제되며 메모리에서 해제되기 직전에 deinitializer 를 호출한다.

```swift
func foo() {
    let yagom: Person = Person(name: "yagom")   // yagom is being initialized
    // 인스턴스의 참조 횟수 : 1
    
    
    // 함수 종료 시점
    // 인스턴스의 참조 횟수 : 0
    // yagom is being deinitialized
}

foo()
```

→ 지역변수의 참조 횟수 확인

: 강한참조 지역변수가 사용된 범위의 코드 실행이 종료되면 그 지역변수가 참조하던 인스턴스의 참조 횟수가 1 감소함

### 강한참조 순환 문제 (Strong Reference Cycle)

- 인스턴스끼리 서로가 서로를 강한참조할 때 문제 발생 가능성 있음

```swift
class Person {
    
    let name: String
    
    init(name: String) {
        self.name = name
    }
    
    var room: Room?
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

class Room {
    
    let number: String
    
    init(number: String) {
        self.number = number
    }
    
    var host: Person?
    
    deinit {
        print("Room \(number) is being deinitialized")
    }
}

var yagom: Person? = Person(name: "yagom")  // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "505")   // Room 인스턴스의 참조 횟수 : 1

room?.host = yagom  // Person 인스턴스의 참조 횟수 : 2
yagom?.room = room  // Room 인스턴스의 참조 횟수 : 2

yagom = nil // Person 인스턴스의 참조 횟수 : 1
room = nil  // Room 인스턴스의 참조 횟수 : 1

// Person 인스턴스를 참조할 방법 상실 - 메모리에 잔존
// Room 인스턴스를 참조할 방법 상실 - 메모리에 잔존
```

→ 메모리 누수 발생

- 수동으로 해결하려면 아래와 같이 참조하는 곳 하나하나를 해제시켜야 한다.

```swift
room?.host = yagom  // Person 인스턴스의 참조 횟수 : 2
yagom?.room = room  // Room 인스턴스의 참조 횟수 : 2

yagom?.room = nil   // Room 인스턴스의 참조 횟수 : 1
yagom = nil // Person 인스턴스의 참조 횟수 : 1

room?.host = nil    // Person 인스턴스의 참조 횟수 : 0
// yagom is being deinitialized

room = nil  // Room 인스턴스의 참조 횟수 : 0
// Room 505 is being deinitialized
```

→ 하지만 이렇게 하나하나 해제시키는 것은 힘들다.

더 깔끔한 방법은 아래의 약한참조

### 약한참조 (Weak Reference)

- 강한참조와는 달리 자신이 참조하는 인스턴스의 참조 횟수를 증가시키지 않는다
- weak 키워드 사용

** 옵셔널 변수만 약한 참조 가능

→ 메모리 해제 시 nil 이 할당되어야 하는데 상수에는 불가능

ex. 강한참조 순환 문제를 약한참조로 해결

```swift
class Person {
    
    let name: String
    
    init(name: String) {
        self.name = name
    }
    
    var room: Room?
    
    deinit {
        print("\(name) is being deinitialized")
    }
}

// 코드 27-6 강한참조 순환 문제를 약한참조로 해결
class Room {
    
    let number: String
    
    init(number: String) {
        self.number = number
    }
    
    weak var host: Person?
    
    deinit {
        print("Room \(number) is being deinitialized")
    }
}

var yagom: Person? = Person(name: "yagom")  // Person 인스턴스의 참조 횟수 : 1
var room: Room? = Room(number: "505")   // Room 인스턴스의 참조 횟수 : 1

room?.host = yagom  // Person 인스턴스의 참조 횟수 : 1
yagom?.room = room  // Room 인스턴스의 참조 횟수 : 2

yagom = nil // Person 인스턴스의 참조 횟수 : 0, Room 인스턴스의 참조 횟수 : 1
// yagom is being deinitialized

print(room?.host)   // nil

room = nil  // Room 인스턴스의 참조 횟수 : 0
// Room 505 is being deinitialized
```

### 미소유참조 (Unowned Reference)

약한참조와 마찬가지로 인스턴스의 참조 횟수를 증가시키지 않는다.

- 미소유참조는 자신이 참조하는 인스턴스가 항상 메모리에 존재할 것이라는 전제를 기반으로 동작한다.
- 미소유참조를 하면서 메모리에서 해제된 인스턴스에 접근하려 한다면 runtime error 발생

```swift
class Person {
    
    let name: String
    
    var card: CreditCard?	// 카드를 소지할 수도, 소지하지 않을 수도 있기 때문에 옵셔널로 정의합니다.
    // 또, 카드를 한 번 가진 후 잃어버리면 안 되기 때문에 강한참조를 해야 합니다.
    init(name: String) {
        self.name = name
    }
    
    deinit { print("\(name) is being deinitialized") }
}

class CreditCard {
    
    let number: UInt
    
    unowned let owner: Person	// 카드는 소유자가 분명히 존재해야 합니다.
    
    init(number: UInt, owner: Person) {
        self.number = number
        self.owner = owner
    }
    
    deinit {
        print("Card #\(number) is being deinitialized")
    }
}

var jisoo: Person? = Person(name: "jisoo")  // Person 인스턴스의 참조 횟수 : 1

if let person: Person = jisoo {
    // CreditCard 인스턴스의 참조 횟수 : 1
    person.card = CreditCard(number: 1004, owner: person)
    // Person 인스턴스의 참조 횟수 : 1
		// 증가하지 않음
}

jisoo = nil // Person 인스턴스의 참조 횟수 : 0
// CreditCard 인스턴스의 참조 횟수 : 0
// jisoo is being deinitialized
// Card #1004 is being deinitialized
```

→ CreditCard 의 owner 프로퍼티가 미소유참조임.

(출처: docs.swift.org)

![Screen Shot 2021-01-21 at 1 57 40 PM](https://user-images.githubusercontent.com/44637101/105282818-7e0e1f00-5bf2-11eb-811e-1594ebbf90b3.png)


The `Customer` instance now has a strong reference to the `CreditCard` instance, and the `CreditCard` instance has an unowned reference to the `Customer` instance.

Because of the unowned `customer` reference, when you break the strong reference held by the `john` variable, there are no more strong references to the `Customer` instance:


![Screen Shot 2021-01-21 at 1 56 38 PM](https://user-images.githubusercontent.com/44637101/105282820-7fd7e280-5bf2-11eb-94c8-441c14ac5bb3.png)



Because there are no more strong references to the Customer instance, it’s deallocated. After this happens, there are no more strong references to the CreditCard instance, and it too is deallocated

** 이후 차후 업데이트 필요
