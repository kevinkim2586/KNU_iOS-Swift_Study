<h1>5. 연산자</h1>

삼항연산자 사용

```swift
var valueA: Int = 3
var valueB: Int = 5
var biggerValue: Int  valueA > valueB ? valueA : valueB// 조건이 참이면 왼쪽값 출력 거짓이면 오른쪽 값 출력


valueA = 0
valueB = -3
biggerValue = valueA > valueB ? valueA : valueB

var stringA: String = ""
var stringB: String = "String"
var resultValue: Double = stringA.isEmpty ? 1.0 : 0.0
resultValue = stringB.isEmpty ? 1.0 : 0.0
```

조건문

스위프트 if 구문은 조건의 값이 꼭 Bool타입의 값이어야함

```swift
let first: Int = 5
let second: Int = 7

if (first > second) {
    print("first > second")
}
else if(first < second){
    print("first < second")
}
else {
    print("first == second")
} // 소괄호 생략 가능
```

switch문

```swift
let integerValue: Int = 5

switch integerValue {
case 0:
    print("Value == zero")
case 1...10:
    print("Value == 1~10")
    fallthrough // case를 마치고 switch 구문을 탈출하지 않고 아래 case로 넘어감
case Int.min..<0, 101..<Int.max:
    print("Vlaue < 0 or Value > 100")
    break
default:
    print("10 < Value <= 100") // 한정된 범위가 명확지 않다면 default는필수
}
```
튜플 switch case

```swift
typealias NameAge = (name: String, age: Int)//별칭선언

let tupleValue: NameAge = ("yagom", 99)

switch tupleValue {
case("yagom", 99):
    print("정확히 맞췄습니다 !")
default:
    print("누굴 찾나요?")
}
```

반복문

```swift
for i in 0...2 {
    print(i)
} /// 0 1 2

for i in 0...5 {
    if i.isMultiple(of: 2){
        print(i)
        continue // continue 키워드를 사용하면 바로 다음 시퀀스로 건너뜁니다
    }
    print("\(i) == 홀수")
} // 0 1==홀수 2 3==홀수 4 5 == 홀수

let helloSwift: String = "Hello Swift!"

for char in helloSwift {
    print(char)
}

var result: Int = 1

let friends: [String: Int] = ["Jay": 35, "Joe": 29, "Jenny", 31]

for tuple in friends {
    print(tuple)
} //("Joe", 29) ("Jay", 35) ("Jenny", 31)

let 주소: [String: String] = ["도": "충청북도", "시군구": "청주시 청원구", "동읍면": "율량동"]

for (키, 값) in 주소 {
    print("\(키) : \(값)")
}// 도 : 충청북도 동읍면 : 율량동 시군구 : 청주시 청원구
```

while 반복 구문사용

```swift
var names: [String] = ["Joker", "Jenny", "Nova", "yagom"]

while names.empty == false {
    print("Good bye /(names.removeFirst())")
    // removeFirst()는 요소를 삭제함과 동시에 삭제한 요소를 반환
} // Good bye Joker Good bye Jenny Good bye Nova Good bye yagom
```
