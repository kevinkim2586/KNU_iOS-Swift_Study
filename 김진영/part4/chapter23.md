# Chapter23 프로토콜 지향 프로그래밍

> 상속이 되지 않는 구조체로 다양항 공통 기능을 구현할 수 있었던 이유?  
> -> 프로토콜, 익스텐션의 기여. (물론 제네릭도)  

## 23.1 프로토콜 초기 구현

> 프로토콜의 익스텐션에는 프로토콜이 요구하는 기능을 실제로 구현할 수 있다. 이를 초기구현이라 한다  
> (저장 프로퍼티는 익스텐션에서 구현할 수 없다)  
> 익스텐션에서 구현한 기능을 사용하지 않고 변경하고 싶다면 재정의하면 된다  

~~~ swift
protocol SelfPrintable {
    func printSelf()
}

extension SelfPrintable where Self: Container {
    func printSelf() {
        print(items)
    }
}

protocol Container: SelfPrintable {
    associatedtype ItemType
    
    var items: [ItemType] { get set }
    var count: Int { get }
    
    mutating func append(item: ItemType)
    
    subscript(i: Int) -> ItemType { get }
}

extension Container {
    
    mutating func append(item: ItemType) {
        items.append(item)
    }
    
    var count: Int {
        return items.count
    }
    
    subscript(i: Int) -> ItemType {
        return items[i]
    }
}

protocol Popable: Container {
    mutating func pop() -> ItemType?
    mutating func push(_ item: ItemType)
}

extension Popable {
    mutating func pop() -> ItemType? {
        return items.removeLast()
    }
    
    mutating func push(_ item: ItemType) {
        self.append(item: item)
    }
}

protocol Insertable: Container {
    mutating func delete() -> ItemType?
    mutating func insert(_ item: ItemType)
}

extension Insertable {
    mutating func delete() -> ItemType? {
        return items.removeFirst()
    }
    
    mutating func insert(_ item: ItemType) {
        self.append(item: item)
    }
}

struct Stack<Element>: Popable {
    var items: [Element] = [Element]()
}

struct Queue<Element>: Insertable {
    var items: [Element] = [Element]()
}

var myIntStack: Stack<Int> = Stack<Int>()
var myStringStack: Stack<String> = Stack<String>()

var myIntQueue: Queue<Int> = Queue<Int>()
var myStringQueue: Queue<String> = Queue<String>()


myIntStack.push(3)
myIntStack.printSelf()  // [3]

myIntStack.push(2)
myIntStack.printSelf()  // [3, 2]

myIntStack.pop()        // 2
myIntStack.printSelf()  // [3]

myStringStack.push("A")
myStringStack.printSelf()  // ["A"]

myStringStack.push("B")
myStringStack.printSelf()  // ["A", "B"]

myStringStack.pop()        // "B"
myStringStack.printSelf()  // ["A"]


myIntQueue.insert(3)
myIntQueue.printSelf()  // [3]

myIntQueue.insert(2)
myIntQueue.printSelf()  // [3, 2]

myIntQueue.delete()     // 3
myIntQueue.printSelf()  // [2]

myStringQueue.insert("A")
myStringQueue.printSelf()  // ["A"]

myStringQueue.insert("B")
myStringQueue.printSelf()  // ["A", "B"]

myStringQueue.delete()     // "A"
myStringQueue.printSelf()  // ["B"]
~~~

## 23.2 맵, 필터. 리듀스 직접 구현해보기

~~~ swift
struct Stack<Element>: Popable {
    var items: [Element] = [Element]()
    
    func map<T>(transform: (Element) -> T) -> Stack<T> {
        
        var transformedStack: Stack<T> = Stack<T>()
        
        for item in items {
            transformedStack.items.append(transform(item))
        }
        
        return transformedStack
    }
    
    func filter(includeElement: (Element) -> Bool) -> Stack<Element> {
        
        var filteredStack: Stack<ItemType> = Stack<ItemType>()
        
        for item in items {
            if includeElement(item) {
                filteredStack.items.append(item)
            }
        }
        
        return filteredStack
    }
    
    func reduce<T>(_ initialResult: T, nextPartialResult: (T, Element) -> T) -> T {
        
        var result: T = initialResult
        
        for item in items {
            result = nextPartialResult(result, item)
        }
        
        return result
    }
}
~~~

~~~ swift
// 코드 23-6 Stack 구조체의 맵 메서드
var myIntStack: Stack<Int> = Stack<Int>()
myIntStack.push(1)
myIntStack.push(5)
myIntStack.push(2)

myIntStack.printSelf()  // [1, 5, 2]

//var myStrStack: Stack<String> = myIntStack.map{ "\($0)" }
var myStrStack: Stack<String> = myIntStack.map { (item: Int) -> String in
    return "\(item)"
}
myStrStack.printSelf()  // ["1", "5", "2"]
~~~

~~~ swift
// 코드 23-8 Stack 구조체의 필터 메서드
let filteredStack: Stack<Int> = myIntStack.filter { (item: Int) -> Bool in
    return item < 5
}
filteredStack.printSelf()   // [1, 2]
~~~

~~~ swift
// 코드 23-10 Stack 구조체의 리듀스 메서드
let combinedInt: Int = myIntStack.reduce(100) { (result: Int, next: Int) -> Int in
    return result + next
}
print(combinedInt) // 108


let combinedDouble: Double = myIntStack.reduce(100.0) { (result: Double, next: Int) -> Double in
    return result + Double(next)
}
print(combinedDouble) // 108.0


let combinedString: String = myIntStack.reduce("") { (result: String, next: Int) -> String in
    return result + "\(next) "
}
~~~