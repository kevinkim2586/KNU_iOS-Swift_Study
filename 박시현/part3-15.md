<h1>Chapter 15. 맵, 필터, 리듀스</h1>

<h2>15.1 맵</h2>

맵 사용 - 기존 컨테이너의 값은 변경되지 않고 새로운 컨테이너가 생성되어 반환되기때문에 맵은 기존 데이터를 변형하는데 많이 사용함

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

<h2>15.2 필터</h2>

필터 - 말 그대로 컨테이너 내부의 값을 걸러서 추출하는 역할을 하는 고차함수

필터 메서드의 사용

```swift
let numbers: [Int] = [0, 1, 2, 3, 4, 5]

let evenNumbers: [Int] = numbers.filter { (number: Int} -> Bool in
  return number % 2  == 0
}
print(evennumbers) // [0, 2, 4]

let oddNumbers: [Int] = numbers.filter{ $0 % 2 == 1 }
print(oddNumbers) // [1, 3, 5]
```

<h2>15.3 리듀스</h2>

<h2>15.4 맵, 필터, 리듀스의 활용</h2>
