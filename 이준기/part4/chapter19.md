# Chapter19 타입캐스팅

- 스위프트는 암시적 데이터 타입 변환을 지원하지 않는 엄격한 언어

## 19.1 기존 언어의 타입 변환과 스위프트의 타입 변환

## 19.2 스위프트 타입 캐스팅

- 스위프트의 타입캐스팅은 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용할수 있음
- is, as 연산자로 구현가능

```swift
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
```

## 19.3 데이터 타입 확인

- is 연산자를 통해 인스턴스가 어떤 클래스의 인스턴스인지 타입 확인 가능

```swift
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
```

## 19.4 다운캐스팅

- 클래스의 상속 모식도에서 자식클래스보다 더 상위에 있는 부모클랫의 타입을 자식 클래스의 타입으로 캐스팅함
- 타입캐스트 연산자 as? as!가 있음
→ as? 연산자는 실패할 경우 nil 을 반환하지만 as!는 강제 다운캐스팅이니 실패할 경우 런타임 오류 발생

```swift
if let actingOne: Americano = coffee as? Americano {
    print("This is Americano")
} else {
    print(coffee.description)
}
// 1 shot(s) coffee

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

if let actingOne: Coffee = myCoffee as? Coffee {
    print("This is Just Coffee")
} else {
    print(coffee.description)
}
// This is Just Coffee

// Success
let castedCoffee: Coffee = yourCoffee as! Coffee

// 런타임 오류!!! 강제 다운캐스팅 실패!
let castedAmericano: Americano = coffee as! Americano
```

## 19.5 Any.AnyObject의 타입캐스팅

- Any는 함수 타입을 포함한 모든 타입을 뜻함
- AnyObject는 클래스 타입만을 뜻

```swift
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
```