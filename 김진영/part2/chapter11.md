# Chapter11 인스턴스 생성 및 소멸

## 11.1 인스턴스 생성

### 11.1.1 프로퍼티 기본값

> 구조체와 클래스의 인스턴스를 생성할 때 옵셔널 저장 프로퍼티를 제외한 모든 저장 프로퍼티에 적절한 초깃값을 할당해야 함  
> 프로퍼티를 정의할 때 프로퍼티 기본값을 할당하면 이니셜라이저에서 따로 초깃값을 할당하지 않아도 됨

~~~ swift
struct Area {
    var squareMeter: Double

    init() {
        squareMeter = 0.0
    }
}
// or
struct Area {
    var squareMeter: Double = 0.0
}
~~~

### 11.1.2 이니셜라이저 매개변수

~~~ swift
struct Area {
    var squareMeter: Double
    
    init(fromPy py: Double) {
        squareMeter = py * 3.3058
    }
    
    init(fromSquareMeter squaremeter: Double) {
        self.squareMeter = squaremeter
    }
    
    init(value: Double) {
        squareMeter = value
    }
    
    init(_ value: Double) {
        squareMeter = value
    }
}
~~~

### 11.1.3 옵셔널 프로퍼티 타입

> 옵셔널로 선언한 저장 프로퍼티는 초기화 과정에서 값을 할당해주지 않는다면 자동으로 nil이 할당된다

### 11.1.4 상수 프로퍼티

> 상수로 선언된 저장 프로퍼티는 인스턴스를 초기화하는 과정에서만 값을 할당할 수 있으며, 처음 할당된 이후로는 값을 변경할 수 없다  
> *클래스 인스턴스의 상수 프로퍼티는 프로퍼티가 정의된 클래스에서만 초기화 할 수 있다(자식클래스에서는 초기화 불가)

### 11.1.5 기본 이니셜라이저와 멤버와이즈 이니셜라이저

> 기본 이니셜라이저  
> 프로퍼티 기본값으로 프로퍼티를 초기화해서 인스턴스를 생성  
> 저장 프로퍼티의 기본값이 모두 지정되어 있고, 동시에 사용자 정의 이니셜라이저가 정의되어 있지 않은 상태에서 제공

> 멤버와이즈 이니셜라이저  
> 구조체는 사용자 정의 이니셜라이저를 구현하지 않으면 프로퍼티의 이름으로 매개변수를 갖는 이니셜라이저인 멤버와이즈 이니셜라이저를 기본으로 제공  
> 클래스는 멤버와이즈 이니셜라이저를 지원하지 않음

### 11.1.6 초기화 위임

> 값 타입인 구조체와 열거형은 코드의 중복을 피하기 위해 이니셜라이저가 다른 이니셜라이저에게 일부 초기화를 위임하는 초기화 위임을 구현 가능  
> 클래스는 나중에..(18장)

### 11.1.7 실패 가능한 이니셜라이저

> 실패 가능성을 내포한 이니셜라이저: 실패했을 때 nil반환되므로 반환 타입이 옵셔널(실제로 값을 반환하지는 않음)

~~~ swift
let yuju: Person? = Person(name: "yuju", age: 25)
let jin: Person? = Person(name: "jin", age: -27)

if let person1 = yuju {
    print(person1.name)
} else {
    print("Person wasn't initialized")
}
// yuju

if let person1 = jin {
    print(person1.name)
} else {
    print("Person wasn't initialized")
}
// Person wasn't initialized
~~~

### 11.1.8 함수를 사용한 프로퍼티 기본값 설정

> 클로저나 함수를 사용해서 프로퍼티 기본값을 제공할 수 있다

~~~ swift
struct Student {
    var name: String?
    var number: Int?
}

class SchoolClass {
    var stduents: [Student] = {
        // 새로운 인스턴스를 생성하고 사용자 정의 연산을 통한 후 반환해준다
        // 반환되는 값의 타입은 [Studnet]타입이어야 한다
        
        var arr: [Student] = []
        for num in 1...15 {
            var student: Student = Student(name: nil, number: num)
            arr.append(student)
        }
        return arr
    }()
}

let myClass: SchoolClass = SchoolClass()
print(myClass.stduents.count)               // 15
~~~

## 11.2 인스턴스 소멸

> 메모리에서 해제되기 직전 클래스 인스턴스와 관련하여 원하는 정리 작업 구현  
> **클래스의 인스턴스에만 구현 가능**

~~~ swift
class SomeClass {
    deinit {
        print("Instance will be deallocated immediately")
    }
}

var instance: SomeClass? = SomeClass()
instance = nil      // Instance will be deallocated immediately
~~~
