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

