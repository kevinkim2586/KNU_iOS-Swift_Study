# Chapter 21 : 익스텐션
- 구조체, 열거형, 클래스, 프로토콜 타입에 새로운 기능을 추가할 수 있는 기능

익스텐션이 타입에 추가할 수 있는 기능은 다음과 같다:

- 연산 타입 프로퍼티 / 연산 인스턴스 프로퍼티
- 타입 메서드 / 인스턴스 메서드
- 이니셜라이저
- 서브스크립트
- 중첩 타입
- 특정 프로토콜을 준수할 수 있도록 기능 추가

**타입에 새로운 기능을 추가할 수는 있지만, 기존에 존재하는 기능 재정의는 불가능

- 클래스의 상속은 클래스 타입에서만 가능하지만 익스텐션은 구조체, 클래스, 프로토콜 등에 적용이 가능합니다

익스텐션을 사용하는 대신 원래 타입을 정의한 소스에 기능을 추가하는 방법도 있겠지만, 외부 라이브러리나 프레임워크를 가져다 썼다면 원본 소스를 수정하지 못한다.

이처럼 외부에서 가져온 타입에 내가 원하는 기능을 추가하고자 할 때 익스텐션을 사용한다. 따로 상속을 받지 않아도 되며, 구조체와 열거형에도 기능을 추가할 수 있으므로 익스텐션은 매우 편리한 기능

```swift
extension 확장할 타입 이름 {
    // 타입에 추가될 새로운 기능 구현
}

//기존에 존재하는 타입이 추가적으로 다른 프로토콜을 채택할 수 있도록 확장
extension 확장할 타입 이름: 프로토콜1, 프로토콜2, 프로토콜3 {
    // 프로토콜 요구사항 구현
}
```

익스텐션은 모든 타입에 적용할 수 있다. 

→ 모든 타입이라 함은 구조체, 열거형, 클래스, 프로토콜, 제네릭 타입 등을 뜻합니다.

즉, 익스텐션을 통해 모든 타입에 연산 프로퍼티, 메서드, 이니셜라이저, 서브스크립트, 중첩 데이터 타입 등을 추가할 수 있다.

```swift
extension Int {
    var isEven: Bool {
        return self % 2 == 0
    }
    var isOdd: Bool {
        return self % 2 == 1
    }
}

print(1.isEven) // false
print(2.isEven) // true
print(1.isOdd)  // true
print(2.isOdd)  // false

var number: Int = 3
print(number.isEven) // false
print(number.isOdd) // true

//Int 타입에 두 개의 연산 프로퍼티를 추가한 것
```

→ 모든 "타입"에 적용이 가능하니 Int 타입에도 extension 적용 가능

- 메서드를 추가하는 것도 가능

```swift
extension Int {
    func multiply(by n: Int) -> Int {
        return self * n
    }
}
print(3.multiply(by: 2))  // 6
print(4.multiply(by: 5))  // 20

var number: Int = 3
print(number.multiply(by: 2))   // 6
print(number.multiply(by: 3))   // 9
```

- 이니셜라이저 추가도 가능

인스턴스를 초기화(이니셜라이즈)할 때 인스턴스 초기화에 필요한 다양한 데이터를 전달받을 수 있도록 여러 종류의 이니셜라이저를 만들 수 있다.

타입의 정의부에 이니셜라이저를 추가하지 않더라도 익스텐션을 통해 이니셜라이저를 추가할 수 있다. 

익스텐션으로 클래스 타입에 편의 이니셜라이저는 추가할 수 있지만, 지정 이니셜라이저는 추가할 수 없다. 지정 이니셜라이저와 디이니셜라이저는 반드시 클래스 타입의 구현부에 위치해야 한다. 

*(값 타입은 상관없음 )*

```swift
extension String {

    subscript(appendValue: String) -> String {
        return self + appendValue
    }

    subscript(repeatCount: UInt) -> String {
        var str: String = ""
        for _ in 0..<repeatCount {
            str += self
        }
        return str
    }
}

print("abc"["def"]) // "abcdef" 
print("abc"[3]) // "abcabcabc"
```
