<h1>Chapter 28. 오류처리</h1>

<h2>28.1 오류처리란</h2>

오류처리 - 프로그램이 오류를 일으켰을 때 이것을 감지하고 회복시키는 일련의 과정

<h2>28.2 오류의 표현</h2>

자판기 동작 오류의 종류를 표현한 vendingMachineError 열거형

```swift
enum VendingMachineError: Error {
    case invalidSelection // 유효하지 않은 선택 
    case insufficientFunds(coinsNeeded: Int) // 자금 부족 - 필요한 동전 개수
    cas outOfStock // 품절
}
```

자판기 동작 오류를 표현한 코드인데 이렇게 오류의 종류를 미리 예상한 다음 오류를 throw 해주면 됨

만약 자금이 부족하고 동전이 5개 더 필요한 상황이라면 throw VendingMachineError.insufficientFunds(coinsNeeded: 5)

<h2>28.3 오류 포착 및 처리</h2>

오류를 처리하기 위한 네 가지 방법

* 함수에서 발생한 오류를 해당 함수를 호출한 코드에 알리는 방법

* do-catch 구문을 이용하여 오류를 처리하는 방법

* 옵셔널 값으로 오류를 처리하는 방법

* 오류가 발생하지 않을 것이라고 확신하는 방법

<h3>28.3.1 함수에서 발생한 오류 알리기</h3>

* try 키워드로 던져진 오류를 받을 수 있음, try 키워드는 try 또는 try?, try! 등으로 표현 할 수 있음

* 함수, 메서드, 이니셜라이저의 매개변수 뒤에 throws 키워드를 사용하면 해당 함수, 메서드, 이니셜라이저는 오류를 던질 수 

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
// Biscuit 제공
// Biscuit
// yagom Chips
// Chips 제공
// jinsung Biscuit
// heejin Chocolate

// 오류 발생!!
```

<h3>28.3.2 do-catch 구문을 이용하여 오류처리</h3>

* do 절 내부의 코드에서 오류를 던지면 catch 절에서 오류를 전달받아 적절하게 처리해줌

```swift
do {
    try 오류 발생 가능코드
    오류가 발생하지 않으면 실행할 코드
    } catch 오류 패턴1 {
        처리 코드
    } catch 오류 패턴2 where 추가 조건 {
        처리 코드
    }
```

<h3>28.3.3 옵셔널 값으로 오류처리</h3>

try?를 사용하여 옵셔널 값으로 반환하여 오류처리. try? 표현을 통해 동작하던 코드가 오류를 던지면 그 코드의 반환 값은 nil이 됨

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
print(x) // nil

let y: Optional = try? someThrowingFunction(shouldThrowError: false)
print(y) // Optional(100)
```

<h3>28.3.4 오류가 발생하지 않을 것이라고 확신하는 방법</h3>

오류가 발생하지 않을 것이라는 확신을 갖고 처리하는 방법 try! 표현을 사용 , 실제 오류가 발생하면 런타임 오류가 발생하여 프로그램이 강제로 종료됨

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
print(y) // 100

let x: Int = try! someThrowingFunction(shouldThrowError: true) // 런타임 오류
```

<h3>28.3.5 다시 던지기</h3>

함수나 메서드는 rethrows 키워드를 사용하여 자신의 매개변수로 전달받은 함수가 오류를 던진다는 것을 나타냄

```swift
// 오류를 던지는 함수
func someThrwoingFunction() throws {
    enum SomeError: Error {
        case justSomeError
    }
    
    throw SomeError.justSomeError
}

// 다시 던지기 함수
func someFunction(callback: () throws -> Void) rethrows {
    try callback() // 다시 던지기 함수는 오류를 다시 던질 뿐 따로 처리하지는 않습니다.
}

do {
    try someFunction(callback: SomeThrowingFunction)
} catch {
    print(error)
}
// justSomeError
```

<h3>28.3.6 후처리 defer</h3>

defer 구문은 코드가 블록을 어떤 식으로 빠져나가든 간에 꼭 실행해야 하는 마무리 작업을 할 수 있도록 도와줌, defer 구문은 코드가 블록을 빠져 나가기 전에 무조건 실행되는 것을 보장

```swift
func writeData() {
    let file = openFile()
    
    // 함수 코드 블록을 빠져나가기 직전 무조건 실행되어 파일을 잊지 않고 닫아줍니다.
    defer {
        closeFile(file)
    }
    
    if ... {
        return
    }
    
    if ... {
        return
    }
    
    // 처리 끝
}
```
