<h1>Chapter 28. 오류처리</h1>

<h2>28.1 오류처리란</h2>

오류처리 - 프로그램이 오류를 일으켰을 때 이것을 감지하고 회복시키는 일련의 과정

<h2>28.2 오류의 표현</h2>

자판기 동작 오류의 종류를 표현한 vendingMachineError 열거형

```swift
enum VendingMachineError: Error {
    case invalidSelection // 유효하지 않은 선택 
    case insufficientFunds(coinsNeeded: Int) // 자금 부족 - 필요한 동전 개수
    cas outOfStock // 품절
}
```

자판기 동작 오류를 표현한 코드인데 이렇게 오류의 종류를 미리 예상한 다음 오류를 throw 해주면 됨

만약 자금이 부족하고 동전이 5개 더 필요한 상황이라면 throw VendingMachineError.insufficientFunds(coinsNeede: 5)

<h2>28.3 오류 포착 및 처리</h2>

오류를 처리하기 위한 네 가지 방법

* 함수에서 발생한 오류를 해당 함수를 호출한 코드에 알리는 방법

* do-catch 구문을 이용하여 오류를 처리하는 방법

* 옵셔널 값으로 오류를 처리하는 방법

* 오류가 발생하지 않을 것이라고 확신하는 방법

<h3>28.3.1 함수에서 발생한 오류 알리기</h3>

<h3>28.3.2 do-catch 구문을 이용하여 오류처리</h3>

<h3>28.3.3 옵셔널 값으로 오류처리</h3>

<h3>28.3.4 오류가 발생하지 않을 것이라고 확신하는 방법</h3>

<h3>28.3.5 다시 던지기</h3>

<h3>28.3.6 후처리 defer</h3>

