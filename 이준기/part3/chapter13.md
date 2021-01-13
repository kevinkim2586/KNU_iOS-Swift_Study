# Chapter13 클로저

> 클로저는 변수나 상수가 선언된 위치에서 참조를 획득하고 저장할수 있음<br>

- 기본 클로저와 후행 클로저가 존재
- 이름이 있으면서 어떤 값도 획득하지 않는 전역함수의 형태
- 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
-이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성한 형태

## 13.1 기본 클로저

- 클로저 표현
- 파이썬의 lambda와 비슷
```swift
{ (매개변수들) -> 반환 타입 in
  실행 코드
}
```

- 예시

```swift
// sum이라는 상수에 클로저를 할당
let sum: (Int, Int) -> Int = { (a: Int, b: Int) in
    return a + b
}

let sumResult: Int = sum(1, 2)
print(sumResult) // 3
```

- 전달 인자로써의 클로저

```swift
let add: (Int, Int) -> Int
add = { (a: Int, b: Int) in
    return a + b
}

let substract: (Int, Int) -> Int
substract = { (a: Int, b: Int) in
    return a - b
}


func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int {
    return method(a, b)
}

var calculated: Int

calculated = calculate(a: 50, b: 10, method: add)

print(calculated) // 60

calculated = calculate(a: 50, b: 10, method: substract)

print(calculated) // 40

```

## 13.2 후행 클로저

- 클로저가 조금 길어지거나 가독성이 떨어진다 싶을 때 사용

```swift
// 후행 클로저의 사용
let reversed: [String] = names.sorted() { (first: String, second: String) -> Bool in
  return first > second
}

// 메서드의 소괄호까지 생략 가능합니다
let reversed: [String] = names.sorted { (first: String, second: String) -> Bool in
  return first > second
}
```

## 13.3 클로저 표현 간소화

### 13.3.1 문맥을 이용한 타입 유추

- 문맥 타입에 따라서 타입을 유추 가능하므로 타입을 굳이 표현 해주지 않아도 됨

```swift
// 클로저의 매개변수 타입과 반환 타입을 생략하여 표현 가능
let reversed: [String] = names.sorted { (first, second) in
  return first > second
}
```

### 13.3.2 단축 인자 이름

- 인자의 이름을 단축을 위해서 $와 숫자의 조합으로 표현

```swift
// 단축 인자 이름을 사용한 표현
let reversed: [String] = names.sorted {
  return $0 > $1
}
```

### 13.3.3 암시적 반환 표현

- 더더 줄이기 return 키워드 마저 줄임

```swift
// 암시적 반환 표현의 사용
let reversed: [String] = names.sorted {$0 > $1}
```

### 13.3.4 연산자 함수

- 매개변수의 타입과 반환 타입이 연산자를 구현한 함수의 모양과 동일하다면 연산자만 표현하더라도 알아서 연산하고 반환함

```swift
public func ><T : Comparable>(lhs: T, rhs: T) -> Bool

// 연산자 함수를 클로저의 역할로 사용
let reversed: [String] = names.sorted(by: >)
```

## 13.4 값 획득

- 클로저는 자신이 정의된 위치의 주변 문맥을 통해 상수나 변수를 획득 할수 있음
- 값 획등을 통해 클로저는 주변에 정의한 상수나 변수가 더 이상 존재하지 않더라도 해당 상수나 변수의 값을 자신 내부에서 참조하거나 수정할 수 있음.

```swift
func makeIncrementer(forIncrement amount: Int) -> (() -> Int) {
  var runningTotal = 0
  func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}

let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)

let first: Int = incrementByTwo() //2
let second: Int = incrementByTwo() //4
let third: Int = incrementByTwo()  //6
```

- incrementer란느 함수를 중첩 함수로 표현하는 makeIncrementer 함수
- 함수객체를 반환한다는 의미


## 13.5 클로저는 참조 타입

```swift
let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)
let sameWithIncrementByTwo: (() -> Int) = incrementByTwo

let first: Int = incrementByTwo() //2
let second: Int = sameWithIncrementByTwo() //4
```

- 서로 같은 클로저를 참조하기 때문에 동일한 클로저가 동작하는것을 확인

## 13.6 탈출 클로저

- 탈출 클로저 : 비동기 작업으로 함수가 종료되고 난 후 호출할 필요가 있는 클로저를 사용해야 할때 탈출 클로저가 필요함

- 비탈출 클로저 : 함수로 전달된 클로저가 함수의 동작이 끝난 후 사용할 필요가 없을 때 비탈출 클로저를 사용

- 탈출 클로저를 매개변수로 갖는 함수
```swift
var completionHandlers: [() -> Void] = []

func someFunctionWithEscapingClosure(completionHandler: @escaping () -> Void) {
  completionHandlers.append(completionHandler)
}
```

- 비탈출 클로저
```swift
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

### 13.6.1 withoutActuallyEscaping

- 비 탈출 클로저로 전달한 클로저가 탈출 클로저인 척 해야하는 경우
- 실제로는 탈출하지 않는데 다른 함수에서 탈출 클로저를 요구하는 상황에 해당

```swift
func hasElements(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
    return (array.lazy.filter{predicate($0) }.isEmpty = false )
}
```

- match 클로저는 탈출할 필요가 없음 이때 해당 클로저를 탈출 클로저 인양 사용할수 있게 돕는 withoutActuallyEscaping(_:do:) 함수가 있음

```swift
func hasElements(in array: [Int], match predicate: (Int) -> Bool) -> Bool {
    return withoutActuallyEscaping(predicate, do: {escapablePredicate in
    return (array.lazy.filter{predicate($0) }.isEmpty = false )
})
}
```

## 13.7 자동 클로져

- 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저를 자동 클로저라고 함.
- 자동 클로저는 전달인자를 갖지 않습니다.
- 자동 클로저는 호출되었을 때 자신이 감싸고 있는 코드의 결과값을 반환

```swift
var customersInLine: [String] = ["YoangWha", "SangYong", "SungHun", "HaMi"]

func serveCustomer(_ customerProvider: @autoclosure () -> String) {
  print("Now serving \(customerProvider())!")
}

serveCustomer(customersInline.removeFirst()) // "Now serving YoangWha!"
```
- 매개변수에 자동 클로저 기능을 주었기때문에 실행결과의 값을 인자로 전달 받는다.