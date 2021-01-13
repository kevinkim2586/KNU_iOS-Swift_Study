# Chapter15 맵, 필터, 리듀스

> 스위프트에 대표적인 고차함수<br>

## 15.1 맵

- 자신을 호출할때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환해주는 함수입니다.
- 맵은 기존 데이터를 변형하는데 많이 사용

```swift
let numbers: [Int] = [0, 1, 2, 3, 4]

var doubledNumbers: [Int] = [Int]()
var strings: [String] = [String]()

// for 구문 사용
for number in numbers {
  doulbedNumbers.append(number * 2)
  strings.append("\(number)")
}

print(doubledNumbers) // [0, 2, 4, 6, 8]
print(strings) // ["0", "1", "2", "3", "4"]

// map 메서드 사용
doubledNumbers = numbers.map({ (number: Int) -> Int in 
  return number * 2
})
strings = numbers.map({ (number: Int) -> String in
  return "\(number)"
})

print(doubleNumbers) // [0, 2, 4, 6, 8]
print(strings) // ["0", "1", "2", "3", "4"]
```

## 15.2 필터

- 컨테이너 내부의 값을 걸러서 추출하는 역할
- 기존 컨텐츠를 변형하지 않고 특정 조건에 맞게 걸러내는 역할

```swift
let numbers: [Int] = [0, 1, 2, 3, 4, 5]

let evenNumbers: [Int] = numbers.filter { (number: Int} -> Bool in
  return number % 2  == 0
}
print(evennumbers) // [0, 2, 4]

let oddNumbers: [Int] = numbers.filter{ $0 % 2 == 1 }
print(oddNumbers) // [1, 3, 5]
```

## 15.3 리듀스

- 기능 결합
- 컨테이너 내부의 콘텐츨르 하나로 합하는 기능을 실행하는 고차함수

```swift
let nummbers: [Int] = [1, 2, 3]
// 첫 번째 형태인 reduce(_: _:) 사용

// 초기값이 0이고 정수 배열의 모든 값을 더한다
var sum: Int = numbers.reduce(0, { (result: Int, next: Int) -> Int in
    print("\(result) + \(next)")
    // 0 + 1
    // 1 + 2
    // 3 + 3
    return result + next
})

print(num) // 6

// 문자열 배열을 reduce(_:_:) 메서드를 이용해 연결시킴
let names: [String] = ["Chope", "Jay", "Joker", "Nova"]

let reduceNames: [String = names.reduce("yagom's friend : ") {
  return $0 + ", " + $1
}

print(reduceNames) // "yagom's friend : , Chope, Jay, Joker, Nova"
```

```swift
let nummbers: [Int] = [1, 2, 3]
// 두 번째 형태인 reduce(into_: _:) 사용

// 초깃값이 0이고 정수 배열의 모든 값을 더합니다.
// 첫 번째 리듀스 형태와 달리 클로저의 값을 반환하지 않고 내부에서 직접 이전 값을 변경한다는 점이 다릅니다.
sum = numbers.reduce(into: 0, { (result: inout Int, next: Int) int
  print("\(result) + \(next)")
  // 0 + 1
  // 1 + 2
  // 3 + 3
  result += next
})

print(sum) // 6
```

## 15.4 맵, 필터, 리듀스의 활용
