# Chapter16 모나드

- 모나드가 갖춰야 하는 조건
    - 타입을 인자로 받는 타입(특정 타입의 값을 포장)

    - 특정 타입의 값을 포장한 것을 반환하는 함수(메서드)가 존재

    - 포장된 값을 변환하여 같은 형태로 포장하는 함수(메서드)가 존재


## 16.1 컨텍스트

- 컨텍스트 : 컨텐츠를 담은 그 무엇 
    - 물을 담은 컵

- 컨텐츠 : 담긴 무언가
    - 물컵 안의 물

## 16.2 함수객체

- 함수객체란 맵을 적용할 수 있는 컨테이너 타입

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

## 16.3 모나드

- 함수객체 중에서 자신의 컨텍스트와 같은 컨텍스트의 형태로 맵핑할수 있는 함수객체를 닫힌 함수객체라고 한다. 모나드는 닫힌 함수 객체

1. 컨텍스트로부터 값 추출

2. 추출한 값을 doubledEven 함수에 전달

3. 빈 컨텍스트 반환

```swift
func doubledEven(_ num: Int) -> Int? {
  if num.isMultiple(of: 2) {
    return num * 2
  }
  return nil
}

Optional(3).flatMap(doubledEven) 
// nil( == Optional<Int>.none)
```

- 플랫맵과는 차이가 없어 보이지만 플랫맵은 맵과는 다르게 컨텍스트 내부의 컨텍스트를 모두 같은 위상으로 평평하게 펼처준다는 차이가 있음