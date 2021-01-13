<h1>Chapter 16. 모나드</h1>

<h3> 프로그래밍에서 모나드가 갖춰야 하는 조건 </h3>

모나드 - 함수형 프로그래밍에서 순서가 있는 연산을 처리할 때 자주 활용하는 디자인 패턴

* 타입을 인자로 받는 타입(특정 타입의 값을 포장)

* 특정 타입의 값을 포장한 것을 반환하는 함수(메서드)가 존재

* 포장된 값을 변환하여 같은 형태로 포장하는 함수(메서드)가 존재

<h2>16.1 컨텍스트</h2>

컨텍스트 - 컨텐츠를 담은 그 무엇 (ex. 물을 담은 컵)

컨텐츠 - (ex. 물컵 안의 물)

ex) 2라는 숫자를 옵셔널로 둘러싸면, 컨텍스트 안에 2라는 컨텐츠가 들어 있는 모양새

<h2>16.2 함수객체</h2>

Array, set, Dictionary 등의 컬렉션 타입은 모두 함수객체이다.

함수객체와 맵 메서드의 모식도

1. map(a->b) - 맵이 함수를 인자로 받음 (ex - addThree(_:))

2. f(a) - 함수객체에 맵이 전달 받은 함수를 적용 (ex - Optional(2))

3. f(b) - 새로운 함수객체를 반환 (ex - Optional(5))

```swift
extension Optional {
  func map<U>(f: (Wrapped) -> U) -> U? {
    switch self {
      case .some(let x): return f(x)
      case. none: return .none
    }
  }
}
```

<h2>16.3 모나드</h2>

모나드는 닫힌 함수객체이다 닫힌 함수객체란 함수객체 중에서 자신의 컨텍스트와 같은 컨텍스트의 형태로 맵핑할 수 있는 함수객체를 뜻한다

플랫맵 - 컨텍스트에 포장된 값을 처리하여 포장된 값을 컨텍스트에 다시 반환하는 맵핑을 수행하도록 하는 메서드

```swift
func doubledEven(_ num: Int) -> Int? {
  if num.isMultiple(of: 2) {
    return num * 2
  }
  return nil
}

Optional(3).flatMap(doubledEven) // nil( == Optional<Int>.none)
```

플랫맵은 맵과 다르게 컨텍스트 내부의 컨텍스트를 모두 같은 위상을 평평하게 펼쳐준다는 차이가 있다

플랫멥이 옵셔널 타입의 element를 포장한 경우에 compactMap(_:) 이라는 이름으로 사용한다

```swift
// 맵과 컴팩트의 차이
let optionals: [Int?] = [1, 2, nil, 5]

let mapped: [Int?] = optionals.map{ $0 }
let compactMapped: [Int] = optionals.compactMap{ $0 }

print(mapped) // [Optional(1), Optional(2), nil, Optional(5)]
print(compactMapped) // [1, 2, 5]
```
