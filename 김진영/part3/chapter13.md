# Chapter13 클로저

> 일정 기능을 하는 코드를 하나의 블록으로 모아놓은 것(함수는 클로저의 한 형태)  
> 변수나 상수가 선언된 위치에서 참조를 획득하고 저장할 수 있다

클로저의 세 가지 형태:
- 이름이 있으면서 어떤 값도 획득하지 않는 전역함수의 형태
- 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
- 이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성한 형태

## 13.1 기본 클로저

~~~ swift
func backwards(first: String, second: String) -> Bool {
    return first > second
}

let names: [String] = ["a1", "c2", "b3", "d4", "z5", "w6", "y7"]
// 함수 이용한 방식
let reversed1 = names.sorted(by: backwards)
// 클로저 이용한 방식
let reversed2 = names.sorted(by: { (first: String, second: String) -> Bool in
    return first > second
})
print(reversed1)
print(reversed2)
~~~

## 13.2 후행 클로저

> 함수나 메서드의 마지막 전달인자로 위치하는 클로저는 함수나 메서드의 소괄호를 닫은 후 작성해도 된다  
> (Xcode에서 자동완성 기능을 사용하면 자동으로 후행 클로저로 유도함)

~~~ swift
// 후행 클로저의 사용
let reversed = names.sorted() { (first: String, second: String) -> Bool in
    return first > second
}
// 후행 클로저에서 괄호 생략
let reversed = names.sorted { (first: String, second: String) -> Bool in
    return first > second
}
~~~

## 13.3 클로저 표현 간소화

### 13.3.1 문맥을 이용한 타입 유추

~~~ swift
let reversed = names.sorted { (first, second) -> Bool in
    return first > second
}
~~~

### 13.3.2 단축 인자 이름

> 단축 인자 이름은 첫 번째 전달인자부터 $0, $1, $2, ... 순서로 표현한다

~~~ swift
let reversed = names.sorted {
    return $0 > $1
}
~~~

### 13.3.3 암시적 반환 표현

> 클로저가 반환 값을 갖는 클로저이고 클로저 내부의 실행문이 단 한줄 이라면 return 생략 가능

~~~ swift
let reversed = names.sorted { $0 > $1 }
~~~

### 13.3.4 연산자 함수

> \> 연산자 정의에 따르면 \> 자체가 두 개의 매개변수를 받아 Bool을 반환하는 함수의 이름이다  
> -> 즉 sorted함수의 전달인자로 보내기에 충분한 조건을 가지고 있다!

~~~ swift
// > 연산자 정의
public func ><T: Comparable>(lhs: T, rhs: T) -> Bool
~~~
~~~ swift
let reversed = names.sorted(by: >)
~~~

## 13.4 값 획득

> 값 획득을 통해 클로저는 주변에 정의한 상수나 변수가 더 이상 존재하지 않더라도 해당 상수나 변수의 값을 자신 내부에서 참조하거나 수정할 수 있다

~~~ swift
func makeIncrementer(forIncrement amount: Int) -> (() -> Int) {
    var runningTotal = 0
    func incrementer() -> Int {
        runningTotal += amount
        return runningTotal
    }
    return incrementer
}

let incrementByTwo: (() -> Int) = makeIncrementer(forIncrement: 2)

let first: Int = incrementByTwo()       // 2
let second: Int = incrementByTwo()      // 4
let third: Int = incrementByTwo()       // 6
~~~

## 13.5 클로저는 참조 타입

> 13.4의 예제코드에서 incrementByTwo는 상수이다. 하지만 획득한 값(runningTotal)을 수정할 수 있다. 왜냐하면 함수와 클로저는 참조 타입이기 때문이다.  
> -> 즉 let incrementByTwo 선언은 incrementByTwo에 클로저의 참조 값이 들어가서 incrementByTwo가 다른 값을 참조하도록 변경하지 못한다는 뜻이다.

## 13.6 탈출 클로저

> 탈출(Escape): 함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 탈출한다고 표현  
> @escaping 키워드 사용  
> @escaping 키워드를 따로 명시하지 않으면 비탈출 클로저  

~~~ swift
typealias VoidVoidClosure = () -> Void

var closures: [VoidVoidClosure] = []

// closure 매개변수 클로저는 함수 외부 변수에 저장될 수 있으므로 탈출 클로저이다
func appendClosure(closure: @escaping VoidVoidClosure) {
    // 전달인자로 전달받은 클로저가 함수 외부의 변수 내부에 저장되므로 함수를 탈출합니다
    closures.append(closure)
}
~~~

*탈출 클로저 안에서 해당 타입의 프로퍼티나 메서드, 서브스크립트 등에 접근하려면 명시적으로 self를 사용하여야 한다*

### 13.6.1 withoutActuallyEscaping

**\*추추에 다시보고 수정추가하기\***

## 13.7 자동 클로저

> 함수의 전달인자로 전달하는 표현을 자동으로 변환해주는 클로저  
> **전달인자를 갖지 않는다**  
> 호출되었을 때 자신이 감싸고 있는 코드의 결과값을 반환한다  
> @autoclosure 키워드 사용  
> @autoclosure 속성은 기본적으로 @noescape 속성을 포함한다
