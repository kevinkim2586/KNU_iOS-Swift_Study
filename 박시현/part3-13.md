<h1>Chapter 13. 클로저</h1>

* 일정 기능을 하는 코드를 하나의 블록으로 모아놓은 것 (함수는 클로저의 한 형태)

* 클로저는 매개변수와 반환 값의 타입을 문맥을 통해 유추할 수 있기 때문에 매개변수와 반환 값의 타입을 생략 할 수 있다

* 클로저에 단 한 줄의 표현만 들어있다면 암시적으로 이를 반환 값으로 취급함

* 축약된 전달인자 이름을 사용할 수 있음

* 후행 클로저 문법을 사용할 수 있음

<h2>13.1 기본 클로저</h2>

함수를 이용한 정렬

```swift
func backwards(first: String, second: String) -> Bool {
  print("\(first) \(second) 비교중")
  return first > second
}

let reversed: [String] = names.sorted(by: backwards)
print(reversed) // ["yagom", "wizplan", "jenny", "eric"]
```

클로저 표현

```swift
{ (매개변수들) -> 반환 타입 in
  실행 코드
}
```
클로저를 이용한 정렬

```swift
let reversed: [String] = names.sorted (by: { (first: String, second: String) -> Bool in
  return first > second
})
print(reversed) // ["yagom", "wizplan", "jenny", "eric"]
```

<h2>13.2 후행 클로저</h2>

클로저가 조금 길어지거나 가독성이 떨어진다 싶을 때 사용

```swift
// 후행 클로저의 사용
let reversed: [String] = names.sorted() { (first: String, second: String) -> Bool in
  return first > second
}

// sorted(by:) 메서드의 소괄호까지 생략 가능함
let reversed: [String] = names.sorted { (first: String, second: String) -> Bool in
  return first > second
}

func doSomething(do: (String) -> Void,
                 onSuccess: (Any) -> Void,
                 onFailure: (Error) -> Void) {
   // do something...
}

// 다중 후행 클로저의 사용
doSomething { (someString: String) in
  // do closure
} onSuccess: { (result: Any) in
  // success closure
} onFailure: { (error: Error) in
  // failure closure
}
```
              
                 

<h2>13.3 클로저 표현 간소화</h2>

<h3>13.3.1 문맥을 이용한 타입 유추</h3>

문맥에 따라 적절히 타입을 유추할 수 있다

```swift
// 클로저의 매개변수 타입과 반환 타입을 생략하여 표현 가능
let reversed: [String] = names.sorted { (first, second) in
  return first > second
}
```

<h3>13.3.2 단축 인자 이름</h3>

```swift
// 단축 인자 이름을 사용한 표현
let reversed: [String] = names.sorted {
  return $0 > $1 // first, second 대신 사용
}

<h3>13.3.3 암시적 반환 표현</h3>

클로저에서는 return 키워드마저 생략이 가능함

```swift
// 암시적 반환 표현의 사용
let reversed: [String] = names.sorted {$0 > $1} // 13.1 코드에서 이렇게 줄이는게 가능함

<h3>13.3.4 연산자 함수</h3>

클로저로서의 연산자 함수 사용

```swift
// 연산자 함수를 클로저의 역할로 사용
let reversed: [String] = names.sorted(by: >)
```

<h2>13.4 값 획득</h2>

클로저는 자신이 정의된 위치의 주변 문맥을 통해 상수나 변수를 획득 할 수 있음

```swift
func makeIncrementer(forIncrement amount: Int) -> (() -> Int) {
  var runningTotal = 0
  func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}
```

```swift
func incrementer() -> Int {
  runningTotal += amount
  return runningTotal
} // 잘못된 코드, 그러나 앞 쪽의 코드처럼 incrementer() 함수 주변에 runningTotal과 amount 변수가 있다면 incrementer() 함수는 두 변수의 참조를 획득할 수 있음
```

<h2>13.5 클로저는 참조 타입</h2>

```swift
let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)
let sameWithIncrementByTwo: (() -> Int) = incrementByTwo

let first: Int = incrementByTwo()
let second: Int = sameWithIncrementByTwo()
```

<h2>13.6 탈출 클로저</h2>

* 탈출 클로저 - 비동기 작업으로 함수가 종료되고 난 후 호출할 필요가 있는 클로저

* 비탈출 클로저 - 함수로 전달된 클로저가 함수의 동작이 끝난 후 사용할 필요가 없을 때 사용하는 클로저

```swift

// 탈출 클로저를 매개변수로 갖는 함수

var completionHandlers: [() -> Void] = []

func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
  completionHandlers.append(completionHandler)
}
```

```swift

// 비탈출 클로저
typealias VoidVoidClosure = () -> Void

func functionWithNoescapeClosure(closure: VoidVoidClosure) {
  closure()
}

func functionWithEscapingClosure(completionHandler: @escaping
  VoidVoidClosure) -> VoidVoidClosure {
  return completionHandler
}

class SomeClass {
  var x = 10
  
  func runNoescapeClosure() {
    // 비탈출 클로저에서 self 키워드 사용은 선택 사항입니다
    functionWithNoescapeClosure { x = 200 }
  }
  
  func runEscapingClosure() -> VoidVoidClosure {
    // 탈출 클로저에서는 명시적으로 self를 사용해야 합니다.
    return functionWithEscapingClosrue { self.x = 100 }
  }
}

let instance: SomeClass()
instance.runNoescapeClosure()
print(instance.x) // 200

let returnedClosure: VoidVoidClosure = instance.runEscapingClosure()
returnedClosure()
print(instance.x) // 100
```

<h3>13.6.1 withoutActuallyEscaping</h3>

비탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야 하는 경우, 실제로는 탈출하지 않는데 다른 함수에서 탈출 클로저를 요구하는 상황

<h2>13.7 자동 클로저</h2>

자동 클로저 - 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저 ( 전달인자를 갖지 않는다 )

```swift
// 자동 클로저를 사용하기 이전
// customersInLine is ["YoangWha", "SangYong", "SungHun", "HaMi"]
var customersInLine: [String] = ["YoangWha", "SangYong", "SungHun", "HaMi"]
func serveCustomer(_ customerProvider: () -> String) {
  print("Now serving \(customerProvider())!")
}

serveCustomer( { coustomersInline.removeFirst() } ) // "Now serving YoangWha!"
```

```swift
// 자동 클로저 사용
// customersInLine is ["YoangWha", "SangYong", "SungHun", "HaMi"]
var customersInLine: [String] = ["YoangWha", "SangYong", "SungHun", "HaMi"]

func serveCustomer(_ customerProvider: @autoclosure () -> String) {
  print("Now serving \(customerProvider())!")
}

serveCustomer(customersInline.removeFirst()) // "Now serving YoangWha!"
```
