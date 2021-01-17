<h1>Chapter 19. 타입캐스팅</h1>


<h2>19.1 기존 언어의 타입 변환과 스위프트의 타입 변환</h2>

<h2>19.2 스위프트 타입캐스팅</h2>

스위프트의 타입캐스팅 - 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용할 수 있음

```swift
class coffee {
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
    return "\(shot) shot(s) \(flavor) lattee"
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
```

// Latte나 Americano는 Coffee인 척할 수 있음 왜냐면 Coffee가 갖는 특성을 모두 갖기 때문에

<h2>19.3 데이터 타입 확인</h2>

타입 확인연산자 is

```swift
let coffee: Coffee = Coffee(shot: 1)
print(coffee.description) // 1 shot(s) coffee

let myCoffee: Americano = Americano(shot: 2, iced: false)
print(myCoffee.description) // 2 shot(s) hot americano

let yourCoffee: Latte = Latte(flavor: "green tea", shot: 3)
print(yourCoffee.description) // 3 shot(s) green tea latte

print(coffee is Coffee) // true
print(coffee is Americano) // false
print(coffee is Latte) // false

print(myCoffee is Coffee) // true
print(yourCoffee is Coffee) // true

print(myCoffee is Latte) // false
print(yourCoffee is latte) //true
```

메타 타입
```swift
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
```

<h2>19.4 다운캐스팅</h2>

다운캐스팅 - 클래스의 상속에서 자식클래스보다 더 상위에 있는 부모클래스의 타입을 자식클래스의 타입으로 캐스팅
타입캐스트 연산자 - as? as!
as? - 다운캐스팅에 실패할 가능성이 있을때 사용 성공하면 옵셔널 타입으로 인스턴스 반환, 실패할 경우 nil 반환
as! - 다운캐스팅이 무조건 성공할 것이라고 확신할 때 사용

```swift
if let actingOne: Americano = coffee as? Americano {
    print("This is Americano")
} else {
    print(coffee.description)
}
// 1 shot(s) coffee

if let actionOne: Coffee = coffe as? Coffee {
    print("This is Just Coffee")
} else {
    print(coffee.description)
}
// This is Just Coffee

let castedCoffee: Coffee = yourcoffee as! Coffee // 성공

let castedAmericano: Americano = coffe as! Americano // 
```

<h2>19.5 Any, AnyObject의 타입캐스팅</h2>

AnyObject의 타입캐스팅
```swift
func castTypeToAppropriate(item: AnyObject) {
    if let castedItem: Latte = item as? Latte {
        print(castedItem.description)
    } else if let castedItem: Coffee = item as? Coffee {
        print(castedItem.description)
    } else {
        print("UnKnown Type")
    }
}

castTypeTosf

