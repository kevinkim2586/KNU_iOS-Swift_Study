<h1>Chapter 22. 제네릭</h1>

제네릭 - 더 유연하고 재사용 가능한 함수와 타입의 코드를 작성하는 것을 가능하게 해 줍니다.

ex) Array, Dictionary, Set 등의 타입은 모두 제네릭 컬렉션

제네릭을 사용하고자 할 때는 제네릭이 필요한 타입 또는 메서드의 이름 뒤의 <> 사이에 제네릭을 위한 타입 매개변수를 써주어 제네릭을 사용할 것임을 표시함

<h2>22.1 제네릭 함수</h2>

```swift
제네릭을 사용한 함수

func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
var someInt = 3
var anotherInt = 107
swapTwoValues(&someInt, &anotherInt)
print("\(someInt), \(anotherInt)")
// someInt is now 107, and anotherInt is now 3

var someString = "hello"
var anotherString = "world"
swapTwoValues(&someString, &anotherString)
print("\(someString), \(anotherString)")
// someString is now "world", and anotherString is now "hello"
```

<h2>22.2 제네릭 타입</h2>

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
var stackOfStrings = Stack<String>()
stackOfStrings.push("uno") // ["uno"]
print(stackOfStrings.items)
stackOfStrings.push("dos") // ["uno", "dos"]
print(stackOfStrings.items)
stackOfStrings.push("tres") // ["uno", "dos", "tres"]
print(stackOfStrings.items)
stackOfStrings.push("cuatro") // ["uno", "dos", "tres", "cuatro"]
print(stackOfStrings.items)
// the stack now contains 4 strings

let fromTheTop = stackOfStrings.pop()
print(stackOfStrings.items) // ["uno", "dos", "tres"]
```


<h2>22.3 제네릭 타입 확장</h2>

```swift

위의 코드에서 연결

extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}

if let topItem = stackOfStrings.topItem {
    print("The top item on the stack is \(topItem).")
}
// Prints "The top item on the stack is tres."
```

<h2>22.4 타입 제약</h2>

타입 제약 - 클래스 타입 또는 프로토콜로만 줄 수 있음

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}

let doubleIndex = findIndex(of: 9.3, in: [3.14159, 0.1, 0.25])
print(doubleIndex)
// doubleIndex is an optional Int with no value, because 9.3 isn't in the array
let stringIndex = findIndex(of: "Andrea", in: ["Mike", "Malcolm", "Andrea"])

print(stringIndex)
// stringIndex is an optional Int containing a value of 2
```


<h2>22.5 프로토콜의 연관 타입</h2>

연관타입은 프로토콜의 일부분으로 타입에 플레이스홀더 이름을 부여합니다. 다시말해 특정 타입을 동적으로 지정해 사용할 수 있습니다.

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

```swift
item을 Int형으로

struct IntStack: Container {
    // original IntStack implementation
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

```swift
item을 element형으로

struct Stack<Element>: Container {
    // original Stack<Element> implementation
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    // conformance to the Container protocol
    mutating func append(_ item: Element) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Element {
        return items[i]
    }
}
``` 

<h2>22.6 제네릭 서브스크립트</h2>

제네릭의 서브스크립트에도 조건을 걸 수 있다

```swift
extension Container {
    subscript<Indices: Sequence>(indices: Indices) -> [Item]
        where Indices.Iterator.Element == Int {
            var result = [Item]()
            for index in indices {
                result.append(self[index])
            }
            return result
    }
}
```
