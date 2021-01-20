# Chapter19 타입캐스팅

> 스위프트는 데이터 타입 안전을 위해 다른 타입끼리의 값 교환을 엄격히 제한  
> 암시적 데이터 타입 변환 지원 X

## 19.1 기존 언어의 타입 변환과 스위프트의 타입 변환

C언어
~~~ c
double value = 3.3
int convertedValue = (int)value     // 명시적 데이터 타입 변환
convertedValue = 5.5	        	// double -> int 암시적 데이터 타입 변환
~~~

Swift
~~~ swift
var value: Double = 3.3
var convertedValue: Int = Int(value)
convertedValue = 5.5		        // 오류!!
~~~

실패 가능한 Int 이니셜라이저
~~~ swift
var stringValue: String = "123"
var integerValue: Int? = Int(stringValue)

print(integerValue) // Optional(123)

stringValue = "A123"
integerValue = Int(stringValue)

print(integerValue) // nil == Optional.none
~~~

## 19.2 스위프트 타입캐스팅

> 스위프트의 타입캐스팅은 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용할 수 있다  
> **is**와 **as** 연산자로 구현  

~~~ swift
class Coffee {
    let name: String
    let shot: Int
    var description: String {
        return "\(shot) shot(s) \(name)"
    }
    
    init(shot: Int) {
        self.shot = shot
        self.name = "coffee"
    }
}


class Latte: Coffee {
    var flavor: String
    override var description: String {
        return "\(shot) shot(s) \(flavor) latte"
    }
    
    init(flavor: String, shot: Int) {
        self.flavor = flavor
        super.init(shot: shot)
    }
}

class Americano: Coffee {
    let iced: Bool
    override var description: String {
        return "\(shot) shot(s) \(iced ? "iced" : "hot") americano"
    }
    
    init(shot: Int, iced: Bool) {
        self.iced = iced
        super.init(shot: shot)
    }
}
~~~

Coffee는 Latte나 Americano인척 할 수 없다.
BUT Latte나 Americano는 Coffee인척 할 수 있다.

## 19.3 데이터 타입 확인

is 연산자 사용
~~~ swift
let coffee: Coffee = Coffee(shot: 1)
print(coffee.description)        // 1 shot(s) coffee

let myCoffee: Americano = Americano(shot: 2, iced: false)
print(myCoffee.description)      // 2 shot(s) hot americano

let yourCoffee: Latte = Latte(flavor: "green tea", shot: 3)
print(yourCoffee.description)    // 3 shot(s) green tea latte

print(coffee is Coffee)     // true
print(coffee is Americano)  // false
print(coffee is Latte)      // false

print(myCoffee is Coffee)   // true
print(yourCoffee is Coffee) // true

print(myCoffee is Latte)    // false
print(yourCoffee is Latte)  // true
~~~

메타 타입 타입 사용
~~~ swift
protocol SomeProtocol { }
class SomeClass: SomeProtocol { }

let intType: Int.Type = Int.self
let stringType: String.Type = String.self
let classType: SomeClass.Type = SomeClass.self
let protocolProtocol: SomeProtocol.Protocol = SomeProtocol.self

var someType: Any.Type

someType = intType
print(someType) // Int

someType = stringType
print(someType) // String

someType = classType
print(someType) // SomeClass

someType = protocolProtocol
print(someType) // SomeProtocol

/////////////////////////////////////////////

print(type(of: coffee) == Coffee.self)     // true
print(type(of: coffee) == Americano.self)  // false
print(type(of: coffee) == Latte.self)      // false

print(type(of: coffee) == Americano.self)     // false
print(type(of: myCoffee) == Americano.self)   // true
print(type(of: yourCoffee) == Americano.self) // false

print(type(of: coffee) == Latte.self)     // false
print(type(of: myCoffee) == Latte.self)    // false
print(type(of: yourCoffee) == Latte.self)  // true
~~~

## 19.4 다운캐스팅

> 클래스의 상속 모식도에서 자식클래스보다 더 상위에 있는 부모클래스의 타입을 자식클래스의 타입으로 캐스팅 하는 것  
> **as?** 와 **as!** 연산자 두가지가 있다

이부분 책이 좀 이상한듯. 업캐스팅과 다운캐스팅을 혼용해놓음

책의 이상한 예제들...
~~~ swift
/* 다운캐스팅 */
if let actingOne: Americano = coffee as? Americano {
    print("This is Americano")
} else {
    print(coffee.description)
}
// 1 shot(s) coffee

/* 다운캐스팅 */
if let actingOne: Latte = coffee as? Latte {
    print("This is Latte")
} else {
    print(coffee.description)
}
// 1 shot(s) coffee

if let actingOne: Coffee = coffee as? Coffee {
    print("This is Just Coffee")
} else {
    print(coffee.description)
}
// This is Just Coffee

if let actingOne: Americano = myCoffee as? Americano {
    print("This is Americano")
} else {
    print(coffee.description)
}
// This is Americano

if let actingOne: Latte = myCoffee as? Latte {
    print("This is Latte")
} else {
    print(coffee.description)
}
// 1 shot(s) coffee

/* 업캐스팅 */
if let actingOne: Coffee = myCoffee as? Coffee {
    print("This is Just Coffee")
} else {
    print(coffee.description)
}
// This is Just Coffee

// Success
let castedCoffee: Coffee = yourCoffee as! Coffee    /* 항상 성공하는 업캐스팅 */

// 런타임 오류!!! 강제 다운캐스팅 실패!
let castedAmericano: Americano = coffee as! Americano
~~~

올바른 다운캐스팅 예제
~~~ swift
class Shape {
    var color: String = "yellow"
    
    func draw() {
        print("draw shape")
    }
}

class Rectangle: Shape {
    var cornerRadius: Double = 5.0
    
    override func draw() {
        print("draw rect")
    }
}

class Triangle: Shape {
    override func draw() {
        print("draw triangle")
    }
}

let rect: Shape = Rectangle()
let tri: Shape = Triangle()

if let rect = rect as? Rectangle {
    print(rect.cornerRadius)
} else {
    print("downcasting failed")
}
//  5.0

if let tri = tri as? Triangle {
    tri.draw()
} else {
    print("downcasting failed")
}
//  draw triangle
~~~

-> rect 상수는 Shape 인스턴스를 참조하도록 선언되어 있지만, 실질적으로는 Rectangle의 인스턴스를 참조하고 있다.  
-> 이 때 Rectangle 타입에 정의되어있는 프로퍼티나 메서드에 접근해야 할 때 다운캐스팅을 사용한다.

## 19.5 Any, AnyObject의 타입캐스팅

~~~ swift
func checkType(of item: AnyObject) {
    
    if item is Latte {
        print("item is Latte")
    } else if item is Americano {
        print("item is Americano")
    } else if item is Coffee {
        print("item is Coffee")
    } else {
        print("Unknwon Type")
    }
}

checkType(of: coffee)             // item is Coffee
checkType(of: myCoffee)           // item is Americano
checkType(of: yourCoffee)         // item is Latte
checkType(of: actingConstant)     // item is Latte


func castTypeToAppropriate(item: AnyObject) {
    
    if let castedItem: Latte = item as? Latte {
        print(castedItem.description)
    } else if let castedItem: Americano = item as? Americano {
        print(castedItem.description)
    } else if let castedItem: Coffee = item as? Coffee {
        print(castedItem.description)
    } else {
        print("Unknwon Type")
    }
}

castTypeToAppropriate(item: coffee)             // 1 shot(s) coffee
castTypeToAppropriate(item: myCoffee)           // 2 shot(s) hot americano
castTypeToAppropriate(item: yourCoffee)         // 3 shot(s) green tea latte
castTypeToAppropriate(item: actingConstant)     // 2 shot(s) vanilla latte


func checkAnyType(of item: Any) {
    
    switch item {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let latte as Latte:
        print(latte.description)
    case let stringConverter as (String) -> String:
        print(stringConverter("yagom"))
    default:
        print("something else : \(type(of: item))")
    }
}

checkAnyType(of: 0)         // zero as an Int
checkAnyType(of: 0.0)       // zero as a Double
checkAnyType(of: 42)        // an integer value of 42
checkAnyType(of: 3.14159)   // a positive double value of 3.14159
checkAnyType(of: -0.25)     // some other double value that I don't want to print
checkAnyType(of: "hello")   // a string value of "hello"
checkAnyType(of: (3.0, 5.0))    // an (x, y) point at 3.0, 5.0
checkAnyType(of: yourCoffee)    // 3 shot(s) green tea latte
checkAnyType(of: coffee)    // something else : Coffee
checkAnyType(of: { (name: String) -> String in "Hello, \(name)" })  // Hello, yagom
~~~
