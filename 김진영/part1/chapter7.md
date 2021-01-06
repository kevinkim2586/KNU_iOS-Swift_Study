# Chapter7 함수

> 스위프트에서 함수는 일급 객체이기 때문에 하나의 값 으로도 사용할 수 있다

## 7.1 함수와 메서드

메서드: 구조체, 클래스, 열거형 등 특정 타입에 연관되어 사용하는 함수  
함수: 모듈 전체에서 전역적으로 사용할 수 있는 함수

## 7.2 함수의 정의와 호출

### 7.2.1 기본적인 함수의 정의와 호출

- 오버라이드, 오버로드 모두 지원
- return 키워드 생략 가능(함수 내부의 코드가 단 한줄의 표현이고, 그 표현의 결과값의 타입이 함수의 반환 타입과 일치할 때)

~~~ swift
func hello(name: String) -> String {
    return "Hello \(name)!"
}

let helloJenny: String = hello(name: "Jenny")
print(helloJenny)
~~~

### 7.2.2 매개변수

**매개변수 이름과 전달인자 레이블**

~~~ swift
func sayHello(from myName: String, to name: String) -> String {
    return "Hello \(name)! I'm \(myName)"
}

print(sayHello(from: "yagom", to: "Jin"))
~~~

**전달인자 레이블이 없는 함수 정의와 사용**

~~~ swift
func sayHello(_ name: String, _ times: Int) -> String {
    var result: String = ""
    
    for _ in 0..<times {
        result += "Hello \(name)!\n"
    }
    
    return result
}

print(sayHello("z3ro", 3))
~~~

### 7.2.3 반환이 없는 함수

~~~ swift
// 둘 다 가능

func sayHelloWorld() {
    print("Hello, world!")
}
sayHelloWorld()

func sayGoodbye() -> Void {
    print("Good bye")
}
sayGoodbye()
~~~

### 7.2.4 데이터 타입으로서의 함수

~~~ swift
typealias CalculateTwoInts = (Int, Int) -> Int

func addTwoInts(first a: Int, second b: Int) -> Int {
    return a + b
}

var mathFunction: CalculateTwoInts = addTwoInts

print(mathFunction(2, 5))

func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}

mathFunction = multiplyTwoInts(_:_:)

print(mathFunction(2, 5))
~~~

## 7.3 중첩 함수

함수 안에 함수를 정의 할 수 있음

## 7.4 종료되지 않는 함수

- 종료되지 않는다는 의미는 정상적으로 끝나지 않는 함수라는 뜻
- 이 함수를 실행하면 프로세스 동작은 끝났다고 볼 수 있다
- 비반환 함수 안에서는 오류를 던진다든가, 중대한 시스템 오류를 보고하는 등의 일을 하고 프로세스를 종료해 버린다

~~~ swift
func crashAndBurn() -> Never {
    fatalError("Something very, very bad happend")
}

crashAndBurn()
~~~

## 7.5 반환 값을 무시할 수 있는 함수

@discardableResult 키워드 사용

~~~ swift
@discardableResult func say(_ something: String) -> String {
    print(something)
    return something
}

say("Hello")
~~~
