# Chapter 22 : 제네릭
- Generic
- 재사용성 up, 코드의 중복을 줄일 수 있음
- Generic 을 사용하고자 할 때는 제네릭이 필요한 타입/메서드의 이름 뒤의 < > 사이에 제네릭을 위한 타입 매개변수를 써주어 제네릭을 사용할 것임을 표시

ex.  두 Int 타입의 변수값을 교환하는 함수

```swift
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a
    a = b
    b = temporaryA
}

var someInt = 3
var anotherInt = 107
swapTwoInts(&someInt, &anotherInt)
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")
// Prints "someInt is now 107, and anotherInt is now 3"
```

→ 문제는 Int 값만 가능하다는 것. String 이나 Double 이 적용되기를 원한다면 별도의 함수를 작성해야한다.

→ 코드 중복, 재사용성 down

```swift
func swapTwoStrings(_ a: inout String, _ b: inout String) {
    let temporaryA = a
    a = b
    b = temporaryA
}

func swapTwoDoubles(_ a: inout Double, _ b: inout Double) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

→ 거의 비슷한 코드를 3개나 작성하고 있음 →  very inefficient

### Generic Functions

→ 제네릭 함수는 어떤 타입에도 적용 가능

```swift
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {
    let temporaryA = a
    a = b
    b = temporaryA
}
```

→ 매개변수의 데이터 타입이 T 

** 제네릭 함수는 실제 타입 이름 (Int, String,) 을 써주는 대신에 Placeholder 를 사용한다. 플레이스홀더는 타입의 종류를 알려주지는 않지만 말 그대로 어떤 타입이라는 것은 알려준다. 

위 함수에서, 플레이스홀더 타입이 T인 두 매개변수가 있으므로, 두 매개변수는 같은 타입이라는 것을 알 수 있음

- T의 실제 타입은 함수가 호출되는 그 순간에 결정됨

Placeholder 지정 방법 : < > 안에 placeholder 의 이름들을 나열

—> func swapTwoValues<T> 

위와 같이 작성하면 스위프트 컴파일러는 T의 실제 타입을 신경쓰지 않음.

- 여러 개의 타입 매개변수를 갖고 싶다면 <T,U,V> 이렇게 나열하면 됨.

### Generic Types

- 제네릭 타입을 구현하면 사용자 정의 타입인 구조체, 클래스, 열거형 등이 어떤 타입과도 연관되어 동작 가능

ex. Generic 을 사용하지 않은 IntStack 구조체 타입

```swift
struct IntStack {
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
```

→ 위 구조체는 Int 타입만 사용이 가능하여 굉장히 한정적

- 그런데 Generic Type 의 구조체를 선언하면 어떤 데이터 타입과도 호환되는 stack 구현 가능

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
stackOfStrings.push("uno")
stackOfStrings.push("dos")
stackOfStrings.push("tres")
stackOfStrings.push("cuatro")
// the stack now contains 4 strings
```

### Extending a Generic Type (제네릭 타입 확장)

ex. Stack 구조체 익스텐션

```swift
extension Stack {
    var topItem: Element? {
        return items.isEmpty ? nil : items[items.count - 1]
    }
}
```

→ Extension 을 이용해 제네릭 타입에 새로운 기능을 추가하고자 한다면 익스텐션 정의에 타입 매개변수를 명시하지 않아야 함. 기존에 정의해둔 Element 타입은 그대로 사용 가능

### Type Constraints (타입 제약)

- 제네릭 사용 시 타입의 제약 없이 웬만하면 사용할 수 있으나, 어떤 경우에는 특정 타입만 요구해야 할 수도 있음
- 즉 특정 타입에 한정되어야만 처리할 수 있다던가 특정 프로토콜을 반드시 따라야 한다는 제약을 걸어야 할 수도 있음
- 타입 제약은 **클래스 타입** 또는 **프로토콜**로만 줄 수 있음

ex. Dictionary 의 Key 는 Hashable protocol 을 준수해야 함

```swift
public struct Dictionary<Key : Hashable, Value> : Collection, ExpressibleByDictionaryLiteral
```

→ Key 라는 매개변수는 반드시 Hashable 프로토콜을 준수해야 한다는 의미

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

### Type Constraints 실제 적용 예시

ex. 잘못된 제네릭 함수 구현

```swift
func subtractTwoValue<T>(_ a: T, _ b: T)->T{
	return a - b
}
```

- 얼핏보면 맞아보인다.
- 뺄셈을 하려면 뺄셈 연산자를 사용할 수 있는 타입이어야 연산이 가능하다. 뺄셈 연산자를 지원하지 않는 타입이 들어오게 되면 에러가 나버린다.

ex. 올바른 구현

```swift
func subtractTwoValue<T: BinaryInteger>(_ a: T, _ b: T){
	return a - b
}
```

→ 함수로 들어오는 매개변수가 모두 BinaryInteger 프로토콜을 준수해야 들어올 수 있음

ex. 또 다른 예시

```swift
func findIndex<T>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

→ array 배열 내에서 valueToFind 가 있는지 찾고, 있으면 인덱스를 반환하고 없으면 nil 을 반환하는 간단한 함수

- 하지만 위 함수에서도 중대한 문제가 있음
- 매개변수로 들어오는 valueToFind 가 비교연산자 (==)가 호환되어야 함.
- 스위프트에서는 모든 데이터 타입에 비교연산자가 호환되지는 않음

 ex. 올바른 작성 예시

```swift
func findIndex<T: Equatable>(of valueToFind: T, in array:[T]) -> Int? {
    for (index, value) in array.enumerated() {
        if value == valueToFind {
            return index
        }
    }
    return nil
}
```

→ 즉, 들어오는 매개변수 T가 Equatable 프로토콜을 준수하는지 확인부터하게 해야 한다.

### Associated Types (연관 타입)

- 프로토콜에서 사용할 수 있는 플레이스홀더의 이름
- 어떤 타입이 들어올지 모를 때, 타입 매개변수를 통해 "종류는 알 수 없지만, 어떤 타입이 여기에 쓰일 것이다"라고 표현

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

→ Container 프로토콜은 존재하지 않는 타입인 Item 을 연관타입으로 정의하여 프로토콜 정의에서 해당 타입 이름을 활용한다. 

→ "그 어떤 것이어도 상관없지만, 하나의 타입임은 분명하다"라는 의미

```swift
class MyContainer: Container{
	var items: Array<Int>  = Array<Int>()

	var count: Int{
		return items.count
	}
	func append(_ item: Int){
		items.append(item)
	}
	subscript(i: Int) -> Int{
		return items[i]
	}
}
```

→ Container 프로토콜에 있던 모든 함수를 위에서 구현한 것을 확인 할 수 있음

- 연관타입인 Item 대신에 실제 타입인 Int 타입으로 구현해주었다.
- 프로토콜에서 Item 이라는 연관타입만 정의했을 뿐, 특정 타입을 지정하지는 않았다.

ex. 제네릭 스택 타입이 Container 프로토콜을 준수하는 예시

```swift
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
