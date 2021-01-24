<h1>Chapter 26. where 절</h1>

<h2>26.1 where 절의 활용</h2>

* 패턴과 결합하여 조건 추가

* 타입에 대한 제약 추가

```swift
let tuples: [(Int, Int)] = [(1, 2), (1, -1), (1, 0), (0, 2)]
for tuple in tuples {
    switch tuple {
    case let (x, y) where x == y:
        print("x = y")
    case let (x, y) where x == -y:
        print("x = -y")
    case (let x, let y) where x > y:
        print("x > y")
    case (1, _):
        print("x = 1")
    case (_, 2):
        print("y = 2")
    default:
        print("default")
    }
}

let firstValue: Int = 50
let secondValue: Int = 30
switch firstValue + secondValue {
case let total where total > 100:
    print("total > 100")
case let total where total < 0:
    print("total < 0")
case let total where total == 0:
    print("zero")
case let total:
    print(total)
}
//80
```

옵셔널 패턴과 where 절의 활용

```swift
let arrayOfOptionalInts: [Int?] = [nil, 2, 3, nil, 5]

for case let number? in arrayOfOptionalInts where number > 2 {
    print("Found a \(number)")
}
// Found a 3
// Found a 5
```

타입캐스팅 패턴과 where절의 활용
```swift
let anyValue: Any = "ABC"

switch anyValue {
case let value where value is Int: print("value is Int")
case let value where value is String: print("value is String")
case let value where value is Double: print("value is Double")
default: print("Unknown type")
} // value is String

var things: [Any] = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append({ (name: String) -> String in "Hello, \(name)" })

for things in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

//zero as an Int
//zero as a Double
//an integer value of 42
//a positive double value of 3.14159
//a string value of "hello"
//an (x, y) point at 3.0, 5.0
//Hello, Michael
```

표현 패턴과 where 절의 활용

```swift
var point: (Int, Int) = (1, 2)

switch point {
case (0, 0): print("원점")
case(-2...2, -2...2) where point.0 != 1: print("(\(point.0), \(point.1))은 원점과 가깝습니다.")
default: print("point (\(point.0), \(point.1))")
} // point (1, 2)
```

where 절을 활용한 프로토콜 익스텐션의 프로토콜 준수 제약 추가

```swift
protocol SelfPrintable {
    func printSelf()
}

struct Person: SelfPrintable { }

extension Int: SelfPrintable { }
extension UInt: SelfPrintable { }
extension String: SelfPrintable { }
extension Double: SelfPrintable { }

extension SelfPrintable where Self: FixeWidthInteger, Self: SignedInteger {
    func printSelf() {
        print("FixedWidthInteger와 SignedInteger을 준수하면서 SelfPrintable을 준수하는 타입 \(type(of:self))")
    }
}

extension SelfPrintable where Self: CustomStringConvertible {
    func printSelf() {
        print("CoustomStringConvertible을 준수하면서 SelfPrintable을 준수하는 타입 \(type(of:self))")
    }
}

extension SelfPrintable {
    func printSelf() {
        print("그 외 SelfPrintable을 준수하는 타입 \(type(of:self))")
    }
}

// FixedWidthInteger와 SignedInteger을 준수하면서 SelfPrintable을 준수하는 타입 Int 
Int(-8).printSelf()

// CustomStringConvertible을 준수하면서 SelfPrintable을 준수하는 타입 UInt
UInt(8).printSelf()

// CustomStringConvertible을 준수하면서 SelfPrintable을 준수하는 타입 String
String("yagom").printSelf()

// CustomStringConvertible을 준수하면서 SelfPrintable을 준수하는 타입 Double
Double(8.0).printSelf()

// 그 외 SelfPrintable을 준수하는 타입 Person
Person().printSelf()
```
