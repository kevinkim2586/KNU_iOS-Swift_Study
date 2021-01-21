# Chapter22 제네릭

- 스위프트의 가장 강력한 기능중 하나
- 제네릭을 이용해 코드를 구현하면 어떤 타입에도 유연하게 대응 가능
- 제네릭이 필요한 타입/메서드의 이름 뒤의 < > 사이에 제네릭을 위한 타입 매개변수를써주어 제네릭을 사용할 것임을 표시

```swift
제네릭을 사용하고자 하는 타입 이름 <타입 매개변수>
제네릭을 사용하고자 하는 함수 이름 <타입 매개변수> (함수의 매개변수...)
```

## 22.1 제네릭 함수

- 같은 타입인 두 변수의 값을 교환 한다는 목적을 타입에 상관없이 할수 있도록 단 하나의 함수로 구현할 수 있음
- 제네릭 함수는 실제 타입의 이름을 써주는 대신에 placeholder를 사용

```swift
var numberOne: Int = 5
var numberTwo: Int = 10

var stringOne: String = "A"
var stringTwo: String = "B"

var anyOne: Any = 1
var anyTwo: Any = "Two"

// 코드 22-6 제네릭을 사용한 swapTwoValues(_:_:) 함수
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA: T = a
    a = b
    b = temporaryA
}

swapTwoValues(&numberOne, &numberTwo)
print("\(numberOne), \(numberTwo)") // 10, 5

swapTwoValues(&stringOne, &stringTwo)
print("\(stringOne), \(stringTwo)") // "B, A"

swapTwoValues(&anyOne, &anyTwo)
print("\(anyOne), \(anyTwo)") // "Two", 1

swapTwoValues(&numberOne, &stringOne)   // 오류!! - 같은 타입끼리만 교환 가능
```

## 22.2 제네릭 타입

- 제네릭 타입을 구현하면 사용자 정의 타입인 구조체,클래스, 열거형 등이 어떤 타입과도 연관되어 동작할수 있음.

```swift
struct Stack<Element> {
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
}

var doubleStack: Stack<Double> = Stack<Double>()

doubleStack.push(1.0)
print(doubleStack.items)    // [1.0]
doubleStack.push(2.0)
print(doubleStack.items)    // [1.0, 2.0]
doubleStack.pop()
print(doubleStack.items)    // [1.0]

var stringStack: Stack<String> = Stack<String>()

stringStack.push("1")
print(stringStack.items)    // ["1"]
stringStack.push("2")
print(stringStack.items)    // ["1", "2"]
stringStack.pop()
print(stringStack.items)    // ["1"]

var anyStack: Stack<Any> = Stack<Any>()

anyStack.push(1.0)
print(anyStack.items)    // [1.0]
anyStack.push("2")
print(anyStack.items)    // [1.0, "2"]
anyStack.push(3)
print(anyStack.items)    // [1.0, "2", 3]
anyStack.pop()
print(anyStack.items)    // [1.0, "2"]
```

## 22.3 제네릭 타입 확정

- 익스텐션을 통해 제네릭을 사용하는 타입에 기능을 추가하고자 한다면 익스텐션 저의에 타입 매개변수를 명시하지 않아야함

```swift
extension Stack {
    var topElement: Element? {
        return self.items.last
    }
}

print(doubleStack.topElement)   // Optional(1.0)
print(stringStack.topElement)   // Optional("1")
print(anyStack.topElement)      // Optional("2")
```

## 22.4 타입 제약

- 웬만하면 제약없이 사용할수 있으나, 어떤 경우에는 특정 타입만 요구해야 할 수도 있음
- \*타입제약은 클래스 타입 또는 프로토콜로만 \* 줄수 있다.

```swift
func swapTwoValues<T: BinaryInteger>(_ a: inout T, _ b: inout T) {
    // 함수 구현
}

struct Stack<Element: Hashable> {
    // 구조체 구현
}

```

## 22.5 프로토콜의 연관 타입

- 프로토콜은 정의할 때 연관 타입을 함께 정의하면 유용할때가 있음
- 연관타입은 프로토콜에서 사용할 수 있는 플레이스홀더 이름
- 즉 제네릭에 어떤 타입이 들어올지 모를때, 타입 매개변수를 통해 '종류는 알수 없지만, 어떤 타입이 여기에 쓰일것이다'라고 표현해주면 됨.

```swift
protocol Container {
    associatedtype ItemType
    
    var count: Int { get }
    
    mutating func append(_ item: ItemType)
    
    subscript(i: Int) -> ItemType { get }
}

```


## 22.6 제네릭 서브스크립트

- 서브스크립트도 제네릭을 활용하여 타입에 큰 제한 없이 유연하게 구현 가능

```swift
extension Stack {
    subscript<Indices: Sequence>(indices: Indices) -> [ItemType]
        where Indices.Iterator.Element == Int {
            var result = [ItemType]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}

var integerStack: Stack<Int> = Stack<Int>()
integerStack.append(1)
integerStack.append(2)
integerStack.append(3)
integerStack.append(4)
integerStack.append(5)

print(integerStack[0...2])  // [1, 2, 3]
```


