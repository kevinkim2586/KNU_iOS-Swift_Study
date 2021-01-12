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

리듀스 - 컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행하는 고차함수 (결합이라고 불려야 마땅한 기능)

리듀스는 두 가지 형태로 구현되어 있음. 첫 번째 리듀스는 클로저가 각 요소를 전달받아 연산한 후 값을 다음 클로저 실행을 위해

반환하며 컨테이너를 순환하는 형태

두 번째 리듀스는 컨테이너를 순환하며 클로저가 실행되지만 클로저가 따로 결괏값을 반환하지 않는 형태

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

// 초기값이 3이고 정수 배열의 모든 값을 더한다
let sumFromThree: Int = numbers.reduce(3) {
  print("\($0) + \($1)")
  // 3 + 1
  // 4 + 2
  // 6 + 3
  return $0 + $1
}

print(sumFromThree) // 9

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

맵, 필터, 리듀스 메서드의 연계 사용

```swift
let numbers: [Int] = [1, 2, 3, 4, 5, 6, 7]

// 짝수를 걸러내어 각 값에 3을 곱해준 후 모든 값을 더한다
var result: Int = numbers.filter{ $0.isMultiple(of: 2) }.map{ $0 * 3 }.reduce(0){
$0 + $1 }
print(result) // 36

// for-in 구문 사용 시
result = 0

for number in numbers {
  guard number.isMultiple(of: 2) else {
    continue
  }
  
  result += number * 3
}

print(result) // 36
```

<h2>15.4 맵, 필터, 리듀스의 활용</h2>

```swift

enum Gender {
  case male, female, unknown
}

struct Friend {
  let name: String
  let gender: Gender
  let location: String
  var age: UInt
}

var friends: [Friend] = [Friend]()

friends.append(Friend(name: "Yoobato", gender: .male, location: "발리", age: 26))
friends.append(Friend(name: "Jisoo", gender: .male, location: "시드니", age: 24))
friends.append(Friend(name: "JuHyun", gender: .male, location: "경기", age: 30))
friends.append(Friend(name: "JiYoung", gender: .female, location: "서울", age: 22))
friends.append(Friend(name: "SungHo", gender: .male, location: "충북", age: 20))
friends.append(Friend(name: "JungKi", gender: .unknown, location: "대전", age: 29))
friends.append(Friend(name: "youngMin", gender: .male, location: "경기", age: 24))

// 서울 외의 지역에 거주하며 25세 이상인 친구
var result: [Friend] = friends.map{ Friend(name: $0.name, gender: $0.gender, location: $0.location, age: $0.age+1) }

result = result.filter{ $0.location != "서울" && $0.age >= 25}

let string: String = result.reduce("서울 외의 지역에 거주하며 25세 이상인 친구") {$0
  + "\n" + "\($1.name) \($1.gender) \($1.location) \($1.age)세"}
  
  print(string)
  // 서울 외의 지역에 거주하며 25세 이상인 친구
  // yoobato male 발리 27세
  // Jisoo male 시드니 25세
  // JuHyun male 경기 31세
  // JungKi unknown 대전 30세
  // YoungMin male 경기 25
```
