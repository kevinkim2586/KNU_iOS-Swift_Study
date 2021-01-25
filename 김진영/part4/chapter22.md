# Chapter22 제네릭

> C++의 template과 비슷  
> 재사용하기 쉽고 코드의 중복을 줄일 수 있다  

## 21.1 제네릭 함수

~~~ swift
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
~~~

## 22.2 제네릭 타입

~~~ swift
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
~~~

## 22.3 제네릭 타입 확장

~~~ swift
extension Stack {
    var topElement: Element? {
        return self.items.last
    }
}

print(doubleStack.topElement)   // Optional(1.0)
print(stringStack.topElement)   // Optional("1")
print(anyStack.topElement)      // Optional("2")
~~~

## 22.4 타입 제약

> 클래스 타입 또는 프로토콜로만 타입 제약을 줄 수 있다  

~~~ swift
func swapTwoValues<T: BinaryInteger>(_ a: inout T, _ b: inout T) {
    // 함수 구현
}

struct Stack<Element: Hashable> {
    // 구조체 구현
}


// BinaryInteger || FloatingPoint가 아니라, BinaryInteger && FloatingPoint 이다.
func swapTwoValues<T: BinaryInteger>(_ a: inout T, _ b: inout T) where T: FloatingPoint {
    // 함수 구현
}
~~~

## 22.5 프로토콜의 연관 타입

> 프로토콜에서 사용할 수 있는 플레이스홀더 이름  
> 제네릭의 타입 매개변수와 유사한 기능  

~~~ swift
protocol Container {
    associatedtype ItemType
    
    var count: Int { get }
    
    mutating func append(_ item: ItemType)
    
    subscript(i: Int) -> ItemType { get }
}

class MyContainer: Container {
    var items: Array<Int> = Array<Int>()
    
    var count: Int {
        return items.count
    }
    
    func append(_ item: Int) {
        items.append(item)
    }
    
    subscript(i: Int) -> Int {
        return items[i]
    }
}

struct IntStack: Container {
    
    // 기존 IntStack 구조체 구현
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    
    // Container 프로토콜 준수를 위한 구현
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

struct Stack<Element>: Container {
    // 기존 Stack<Element> 구조체 구현
    var items = [Element]()
    mutating func push(_ item: Element) {
        items.append(item)
    }
    mutating func pop() -> Element {
        return items.removeLast()
    }
    
    // Container 프로토콜 준수를 위한 구현
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
~~~

## 22.6 제네릭 서브스크립트

~~~ swift
extension Stack {
    subscript<Indices: Sequence>(indices: Indices) -> [Element]
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
~~~

