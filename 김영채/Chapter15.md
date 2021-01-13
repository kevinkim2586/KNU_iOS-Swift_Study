# Chapter 15 : 맵, 필터, 리듀스
- 매개변수로 함수를 갖는 함수를 고차함수라고 부르는데, 스위프트의 대표적인 고차함수로 맵, 필터, 리듀스가 있다.

    → 데이터의 연산을 쉽게 실행하게 해준다. 

### 맵 (Map)

- 자신을 호출할 때 매개변수로 전달된 함수를 실행하여 그 결과를 다시 반환해주는 함수
- 배열, 딕셔너리, 세트, 옵셔널 등에서 사용 가능
- 기존 데이터를 변형할 때 많이 쓰임

```swift
let numbers: [Int] = [0,1,2,3,4]

var doubledNumbers: [Int] = [Int]()

//for 구문 사용
for number in numbers{
	doubledNumbers.append(number * 2)
}
print(doubledNumbers)
//0,2,4,6,8

//map 메서드 사용
doubledNumbers = numbers.map({ (number: Int) -> Int in
	return number*2
})
```

→ for-in 구문을 사용한 것보다 간결하고 편리

아래는 클로저를 이용해 조금 더 짧고 간결하게 표현하는 방법

```swift
doubledNumbers = numbers.map({return $0 * 2})

doubledNumbers = numbers.map({ $0 * 2 })
//한 줄일때는 return 생략 가능

doubledNumbers = numbers.map { $0 * 2 }
//소괄호 생략 가능
```

- 재사용 측면을 생각한다면 클로저 부분을 따로 함수로 미리 정의하는 것도 방법임

```swift
let evenNumbers: [Int] = [0,2,4,6,8]
let oddNumbers: [Int] = [0,1,3,5,7]

let multiply: (Int) -> Int = { $0 * 2}

let doubledEvenNum = evenNumbers.map(multiply)
//0,4,8,12,16

let doubledOddNum = oddNumbers.map(multiply)
//0,2,6,10,14
```

- 다양한 컨테이너 타입에서 맵 활용 가능

```swift
var numberSet: Set<Int> = [1,2,3,4,5]
let resultSet = numberSet.map{ $0 * 2 }
print(resultSet)
//2,4,6,8,10
```

### 필터 (Filter)

- 컨테이너 내부의 값을 걸러서 추출하는 역할을 하는 고차함수
- 새로운 컨테이너에 값을 담아 반환
- 맵처럼 기존 값을 변형하는 것이 아닌, 특정 조건에 맞게 걸러내는 역할을 함

→ filter 함수의 매개변수로 전달되는 반환 타입은 Bool이다.

```swift
let numbers: [Int] = [0,1,2,3,4,5]

let evenNumbers: [Int] = numbers.filter { (numbers: Int) -> Bool in
	return numbers % 2 == 0
}
print(evenNumbers)
//0,2,4

let oddNumbers: [Int] = numbers.filter { $0 % 2 == 1 }
print(oddNumbers)
//1,3,5
```

- 맵과 필터의 연계 사용

```swift
let numbers: [Int] = [0,1,2,3,4,5]

let mappedNumbers: [Int] = numbers.map{ $0 + 3 }
//3,4,5,6,7,8

let evenNumbers: [Int] = mappedNumbers.filter{ (numbers: Int)->Bool in
	return numbers % 2 == 0
}

print(evenNumbers)
//4,6,8
```

### 리듀스 (Reduce)

- 결합 (Combine) 이라 불리는 것도 ok
- 컨테이너 내부의 콘텐츠를 하나로 합하는 기능을 실행하는 고차함수

ex) 배열의 모든 값을 전달인자로 전달받은 클로저의 연산 결과로 합해준다.

- initial 매개변수로 초기값 세팅

```swift
let array = [0, 1, 2, 3]

let newArray = array.reduce(0) { $0 + $1 }

 print(newArray) //6

        

let charArray = ["a", "b", "c", "d"]

let charNewArray = charArray.reduce("result is ") { $0 + $1 }

 print(charNewArray) //result is abcd

```

### 맵, 필터, 리듀스의 활용

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

var result: [Friend] = friends.map{ Friend(name: $0.name, gender: $0.gender, location: $0.location, age: $0.age+1) }

// 서울 외의 지역에 거주하며 25세 이상인 친구를 필터링하는 코드
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
