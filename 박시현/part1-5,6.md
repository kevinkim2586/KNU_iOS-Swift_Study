연산자

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
