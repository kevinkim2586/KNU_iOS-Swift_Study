
# Chapter 1 : 스위프트

- 2014년 처음 발표
- 스위프트 언어의 특징 :  Safe, Fast, Expressive
- 함수형 프로그래밍 + 프로토콜 지향 프로그래밍 패러다임 이용

    → 함수형 프로그래밍 사용 이유 :  대모 병렬처리가 쉬움
    

# Chapter 2 : 스위프트 처음 시작하기

### print() 함수

- print() & dump() 모두 콘솔 로그 출력을 위해 쓰임 (dump() 는 내부 콘텐츠까지 출력)
- print() 함수는 \n 를 자동으로 삽입함

### 문자열 보간법 (String Interpolation)

- \() 사용

    → C언어에서는 %d, %f 등 사용

ex)

```swift
let age: Int = 25

print("안녕하세요! 저는 \(age)살입니다")
print("안녕하세요! 저는 \(age+5)살입니다") 
```

### 변수와 상수

상수 선언 키워드: let      

```swift
let 이름: 타입 = 값
//이름: 타입 -> 이 중간에 space가 있어야 함
```

변수 선언 키워드: var

```swift
var 이름: 타입 = 값
```

- 값의 타입이 명확하다면 타입은 생략 가능

```swift
let 이름 = 값
var 이름 = 값

ex)
let constantExample: String = "상수입니다"
var variableExample: String = "변수입니다"
```

상수 선언 후에 나중에 최초 값 할당 1번 가능

```swift
let sum: Int
let inputA: Int = 100
let inputB: Int = 200

sum = inputA + inputB

sum = sum + 1 //다시 수정 불가 오류 발생
```



