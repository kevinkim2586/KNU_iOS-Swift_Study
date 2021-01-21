# Chapter28 오류처리

## 28.1 오류처리란

- 오류를 감지하고 회복시키는 과정

## 28.2 오류의 표현

- 스위프트에서 오류는 error라는 프로토콜을 준수하는 타입의 값을 통해 표현됩니다.
- 오류 떄문에 다음에 행할 동작이 정상적으로 진행되지 안을 때 (throw error)

## 28.3 오류 포착 및 처리

오류를 처리하기 위한 네가지 방법
1. 함수에서 발생한 오류를 해당 함수를 호출한 코드에 알리는 방법
2. do-catch 구문을 이용하여 오류를 처리하는 방법
3. 옵셔널 값으로 오류를 처리하는 바업ㅂ
4. 오류가 발생하지 않을 것이라고 확신하는 방법

### 28.3.1 함수에서 발생한 오류 알리기

- try 키워드로 던져진 오류를 받을수 있음 
- try?, try! 등으로 표현 가능

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

### 28.3.2 do-catch 구문을 이용하여 오류처리

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

### 28.3.3 옵셔널 값으로 오류처리
- try? 를 사용하여 옵셔널 값으로 변환하여 오류를 처리할수도 있음. try 표현을 통해 동작하던 코드가 오류를 던지면 그 코드의 반환값은 nil이 됩니다.

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

### 28.3.4 오류가 발생하지 않을 것이라고 확신하는방법

- 오류가 발생하지 않는다고 확신하면 try!구문을 사용할수 있다.

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

### 28.3.5 다시 던지기

- 함수나 메서드는 rethrows 키워드를 사용하여 자신의 매개변수로 전달받은 함수가오류를 단진다는 것을 나타낼수 있음

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

### 28.3.6 후처리 defer

- defer 구문을 사용하여 현재 코드 블록을 나가기 전에 꼭 실행해야 하는 코드를 작성해줄 수 있음

```swift
func writeData(){
	let file = openFile()
	//함수 코드 블록을 빠져나가기 직전 무조건 실행되어 파일을 잊지 않고 닫아 줍니다.
	defer{
		closeFile(file)
	}
	if...{
		return
	}
	if...{
		return
	}
	//처리끝
}
```
