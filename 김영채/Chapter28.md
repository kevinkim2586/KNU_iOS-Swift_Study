# Chapter 28 : 오류처리
- Error Handling → 프로그램이 오류를 일으켰을 때 이것을 감지하고 회복시키는 일련의 과정

### 오류의 표현

- 스위프트에서의 오류는 Error 라는 프로토콜을 준수하는 타입의 값을 통해 표현
- 오류를 표현하기 위한 타입 ( 주로 열거형) 은 이 프로토콜을 채택한다

ex. 자판기 오류의 종류를 열거형으로 표현한 것

```swift
enum VendingMachineError: Error {
    case invalidSelection
    case insufficientFunds(coinsNeeded: Int)
    case outOfStock
}
```

→ 위 프로토콜은 Error 프로토콜을 채택하고 있음

- 열거형을 통해 오류의 종류를 표현하는 것이 가장 일반적 & 편리

** 위와 같이 오류의 종류를 미리 예상한 다음, 오류 때문에 다음에 행할 동작이 정상적으로 진행되지 않을 때라면 오류를 던져 (Throw error) 주면 된다.

→ throw 구문을 사용하여 오류를 던진다.

ex. throw VendingMachineError.insufficientFunds(coinsNeeded: 5)

### 오류 포착 및 처리

오류를 throw 하는 것도 가능하지만, 오류가 던져지는 것에 대비하여 던져진 오류를 처리하기 위한 코드도 따로 작성해야함.

ex. 던져진 오류가 무엇인지 파악하여 이를 해결

ex. 오류를 알리고 사용자에게 선택 권한을 넘겨주어 다음에 어떤 동작을 하게 할 것인지 결정하도록 유도하는 코드

- ***오류를 처리하기 위한 4가지 방법:***
1. 함수에서 발생한 오류를 해당 함수를 호출한 코드에 알리는 방법
2. do-catch 구문을 이용하여 오류를 처리하는 방법 
3. 옵셔널 값으로 오류를 처리하는 방법 (?)
4. 오류가 발생하지 않을 것이라고 확신하는 방법 (!)

### 함수에서 발생한 오류 알리기

- try 키워드로 던져진 오류를 받을 수 있음
- try, try?, try! 로 표현 가능
- 함수, 메서드, 이니셜라이저의 매개변수 뒤에 throws 키워드를 사용하면 오류 던지기 가능

일반적인 함수: func cannotThrowErrors()→ String

오류 던지는 함수: func canThrowErrors() throws → String

note: throws 를 포함한 함수, 메서드, 이니셜라이저는 일반 함수, 메서드, 이니셜라이저로 재정의 불가능. 엄격하게 타입 구분을 함

```swift
struct Item {
    var price: Int
    var count: Int
}

class VendingMachine {
    var inventory = [
        "Candy Bar": Item(price: 12, count: 7),
        "Chips": Item(price: 10, count: 4),
        "Biscuit": Item(price: 7, count: 11)
    ]
    
    var coinsDeposited = 0
    
    func dispense(snack: String) {
        print("\(snack) 제공")
    }
    
    func vend(itemNamed name: String) throws {
        guard let item = self.inventory[name] else {
            throw VendingMachineError.invalidSelection
        }
        
        guard item.count > 0 else {
            throw VendingMachineError.outOfStock
        }
        
        guard item.price <= self.coinsDeposited else {
            throw VendingMachineError.insufficientFunds(coinsNeeded: item.price - self.coinsDeposited)
        }
        
        self.coinsDeposited -= item.price
        
        var newItem = item
        newItem.count -= 1
        self.inventory[name] = newItem
        
        self.dispense(snack:name)
    }
}

let favoriteSnacks = [
    "yagom": "Chips",
    "jinsung": "Biscuit",
    "heejin": "Chocolate"
]

func buyFavoriteSnack(person: String, vendingMachine: VendingMachine) throws {
    let snackName = favoriteSnacks[person] ?? "Candy Bar"
    try vendingMachine.vend(itemNamed: snackName)
}

struct PurchasedSnack {
    let name: String
    init(name: String, vendingMachine: VendingMachine) throws {
        try vendingMachine.vend(itemNamed: name)
        self.name = name
    }
}

let machine: VendingMachine = VendingMachine()
machine.coinsDeposited = 30

var purchase: PurchasedSnack = try PurchasedSnack(name: "Biscuit", vendingMachine: machine)
// Biscuit 제공

print(purchase.name)    // Biscuit

for (person, favoriteSnack) in favoriteSnacks {
    print(person, favoriteSnack)
    try buyFavoriteSnack(person: person, vendingMachine: machine)
}
// yagom Chips
// Chips 제공
// jinsung Biscuit
// heejin Chocolate

// 오류 발생!!
```

### do-catch 구문을 이용하여 오류처리

- 함수,메서드, 이니셜라이저 등에서 오류를 던져주면 오류 발생을 전달받은 코드 블록은 do-catch 구문을 사용하여 오류를 처리해주어야 한다.
- do 절 내부의 코드에서 오류를 던지면, catch 절에서 오류를 전달받아 적절이 처리

```swift
do {
	try 오류 발생 가능코드
	오류가 발생하지 않으면 실행할 코드
} catch 오류 패턴1{
	처리코드
} catch 오류 패턴2 where 추가조건{
	처리코드
}
```

```swift
func tryingVend(itemNamed: String, vendingMachine: VendingMachine) {
    do {
        try vendingMachine.vend(itemNamed: itemNamed)
    } catch VendingMachineError.invalidSelection {
        print("유효하지 않은 선택")
    } catch VendingMachineError.outOfStock {
        print("품절")
    } catch VendingMachineError.insufficientFunds(let coinsNeeded) {
        print("자금부족 - 동전 \(coinsNeeded)개를 추가로 지급해주세요.")
    } catch {
        print("그 외 오류 발생 : ", error)
    }
}
```

### 옵셔널 값으로 오류처리

- try? 를 사용하여 옵셔널 값으로 변환하여 오류를 처리할 수도 있음
- try?를 실행하고 오류를 던지면 그 코드의 반환값은 nil 이 됨

```swift
func someThrowingFunction(shouldThrowError: Bool) throws -> Int {
    
    if shouldThrowError {
        
        enum SomeError: Error {
            case justSomeError
        }
        
        throw SomeError.justSomeError
    }
    
    return 100
}

let x: Optional = try? someThrowingFunction(shouldThrowError: true)
print(x)    // nil

let y: Optional = try? someThrowingFunction(shouldThrowError: false)
print(y)    // Optional(100)
```

→ x, y 도 모두 옵셔널로 선언되어 있어야 반환타입이 nil 일 때 받아오기 가능.

### 오류가 발생하지 않을 것이라고 확신하는 방법

- try!
- 대신 실패하면 runtime 오류 발생하여 앱 강제 종료

```swift
func someThrowingFunction(shouldThrowError: Bool) throws -> Int {
    
    if shouldThrowError {
        
        enum SomeError: Error {
            case justSomeError
        }
        
        throw SomeError.justSomeError
    }
    
    return 100
}

let y: Int = try! someThrowingFunction(shouldThrowError: false)
print(y)    // 100

let x: Int = try! someThrowingFunction(shouldThrowError: true)	// 런타임 오류!!
```

### 다시 던지기

- rethrows 키워드 → 자신의 매개변수로 전달받은 함수가 오류를 던진다는 것을 나타낼 수 있음
- rethrows 가 가능하게 하려면 최소 하나 이상의 오류 발생 가능한 함수를 매개변수로 전달받아야 한다.

```swift
func someThrowingFunction() throws {
    
    enum SomeError: Error {
        case justSomeError
    }
    
    throw SomeError.justSomeError
}

// 다시던지기 함수
func someFunction(callback: () throws -> Void) rethrows {
    try callback()   // 다시던지기 함수는 오류를 다시 던질 뿐 따로 처리하지는 않습니다.
}

do {
    try someFunction(callback: someThrowingFunction)
} catch {
    print(error)
}
// justSomeError
```

### 후처리 defer

- 현재 코드 블록을 나가기 전에 꼭 실행해야 하는 코드를 작성 가능
- 코드가 블록을 어떤 식으로 빠져나가든 간에 꼭 실행해야 하는 마무리 작업 실행 가능

ex. 파일을 열어 사용하다가 오류 발생하여 코드가 블록을 빠져나가더라도 파일을 정상적으로 닫아 메모리에서 해제해야 하기 때문에 defer 구문 내부에는 파일을 닫는 코드 추가 가능

```swift
func someThrowingFunction(shouldThrowError: Bool) throws -> Int {
    
    defer {
        print("First")
    }
    
    if shouldThrowError {
        
        enum SomeError: Error {
            case justSomeError
        }
        
        throw SomeError.justSomeError
    }
    
    defer {
        print("Second")
    }
    
    defer {
        print("Third")
    }
    
    return 100
}

try? someThrowingFunction(shouldThrowError: true)
// First
// 오류를 던지기 직전까지 작성된 defer 구문까지만 실행됩니다.

try? someThrowingFunction(shouldThrowError: false)
// Third
// Second
// First

// 코드 28-13 복합적인 defer 구문의 실행 순서
func someFunction() {
    print("1")
    
    defer {
        print("2")
    }
    
    do {
        defer {
            print("3")
        }
        print("4")
    }
    
    defer {
        print("5")
    }
    
    print("6")
}

someFunction()

// 1
// 4
// 3
// 6
// 5
// 2
```
