옵셔널 - 값이 있을 수도, 없을 수도 있음 을 나타내는 표현 // 변수 또는 상수의 값이 nil일수도 있다는 뜻

오류가 발생하는 nil 할당

```swift
var myName: String = "yagom"
myName = nil //오류  nil은 옵셔널로 선언된 곳에서만 사용가능
```
옵셔널 변수의 선언 및 nil 할당

```swift
var myName: String? = "yagom" // 옵셔널 변수 또는 상수 등은 데이터 타입 뒤에 물음표(?)를 붙여 표현해줌
print(myName) // Optional("yagom")

myName = nil
print(myName) // nil
```

옵셔널 표현
```swift
//(1) 느낌표를 이용한 암시적 추출 옵셔널
//Implicitly Unwrapped Optional

var implicitlyUnwrappedOptionalValue: Int! = 100

switch implicitlyUnwrappedOptionalValue{
case.none:
    print("This Optional variable is nil")
case.some(let value):
    print("Value is \(value)")
}
```
```swift
//(2) 물음표(?)를 이용한 옵셔널
//Optional
var OptionalValue: Int? = 100

switch optionalValue {
case .none:
    print("This Optional variable is nil")
case .some(let value):
    print("Value is \(value)")
}
```

옵셔널 강제추출이란? // 보호막을 강제로 부심
```swift
//* 옵셔널에 들어있는 값을 사용하기 위해 꺼내오는 것
```

옵셔널 바인딩 // 안에 값이 있습니까? 라고 물어보는거라고 생각하면됌
```swift
//(1). nil체크 + 안전한 추출
//(2). 옵셔널 안에 값이 들어있는지 확인하고 값이 있으면 값을 꺼내온다.
//(3). if-let 방식 사용

func printName(_ name: String) {
    print(name)
}

var myName: String? = nil

//printName(myName)
// 전달되는 값의 타입이 다르기 때문에 컴파일 오류발생

if let name: String = myName {
    printName(name)
} else {
    print("myName == nil")
}


var yourName: String! = nil

if let name: String = yourName {
    printName(name)
} else {
    print("yourName == nil")
}

// name 상수는 if-let 구문 내에서만 사용가능합니다
// 상수 사용범위를 벗어났기 때문에 컴파일 오류 발생
//printName(name)
```
```swift
// ,를 사용해 한 번에 여러 옵셔널을 바인딩 할 수 있습니다
// 모든 옵셔널에 값이 있을 때만 동작합니다
myName = "yagom"
yourName = nil

if let name = myName, let friend = yourName {
    print("\(name) and \(friend)")
}
// yourName이 nil이기 때문에 실행되지 않습니다
yourName = "hana"

if let name = myName, let friend = yourName {
    print("\(name) and \(friend)")
}
// yagom and hana
```
