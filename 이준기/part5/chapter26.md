# Chapter26 where 절

- 특정 패턴과 결합하여 조건을 추가하는 역할

## 26.1 where 절의 활용

- 패턴과 결합하여 조건 추가
- 타입에 대한 제약 추가
- 각각의 타입에 결합이 가능함
값 바인딩 와일드카드 패턴, 옵셔널 패턴... 앞서 언급한 패턴과 모두 결합이 가능

- 값 바인딩 와일드카드 패턴과 where 절의 활용

```swift
let tuples: [(Int, Int)] = [(1, 2), (1, -1), (1, 0), (0, 2)]

// 값 바인딩, 와일드카드 패턴
for tuple in tuples {
    switch tuple {
    case let (x, y) where x == y: print("x == y")
    case let (x, y) where x == -y: print("x == -y")
    case let (x, y) where x > y: print("x > y")
    case (1, _): print("x == 1")
    case (_, 2): print("y == 2")
    default: print("\(tuple.0), \(tuple.1)")
    }
}
/*
 x == 1
 x == -y
 x > y
 y == 2
 */


var repeatCount: Int = 0
// 값 바인딩 패턴
for tuple in tuples {
    switch tuple {
    case let (x, y) where x == y && repeatCount > 2: print("x == y")
    case let (x, y) where repeatCount < 2: print("\(x), \(y)")
    default: print("Nothing")
    }
    
    repeatCount += 1
}
/*
 1, 2
 1, -1
 Nothing
 Nothing
 */


let firstValue: Int = 50
let secondValue: Int = 30

// 값 바인딩 패턴
switch firstValue + secondValue {
case let total where total > 100: print("total > 100")
case let total where total < 0: print("wrong value")
case let total where total == 0: print("zero")
case let total: print(total)
}   // 80
```