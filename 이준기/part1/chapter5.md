# Chapter5 연산자

>특정한 문자로 표현한 함수<br>
>대문자 카멜 케이스를 이용

## 5.1 연산자의 종류

### 5.1.1 할당 연산자

- 값을 할당 할때 사용하는 연산자 "="를 사용

### 5.1.2 산술 연산자

- 수학에서 쓰이는 연산자와 같은 역할을 수행함 
- +,-,,/,% 등이 사용

### 5.1.3 비교 연산자

- 두 값을 비교할 때 사용
- ==,>=,<=,<,>,!=,=== 등이 사용

### 5.1.4 삼항 조건 연산자

- 피연산자가 세개인 삼항 조건 연산

```swift
var valueA: Int = 3
var valueB: Int = 5
var biggerValue: Int  valueA > valueB ? valueA : valueB // 5


valueA = 0
valueB = -3
biggerValue = valueA > valueB ? valueA : valueB // 0

var stringA: String = ""
var stringB: String = "String"
var resultValue: Double = stringA.isEmpty ? 1.0 : 0.0  // 1.0
resultValue = stringB.isEmpty ? 1.0 : 0.0 // 0.0
```

### 5.1.5 범위 연산자

- 값(수)의 범위를 나타내고자 할 때 사용합니다.
- A...B,A..<B,A...,...A,..<A 등이 사용

### 5.1.6 부울 연산자

- 불리언 값의 논리 연사을 할 때 사용합니다.
- !, &&,|| 등이 사용

### 5.1.7 비트 연산자

- 갑스이 비트 논리 연산을 위한 연산자

### 5.1.8 복합 할당 연산자

- 할당 연산자와 다른 연산자가 하는 일을 한 번에 할 수 있도록 연산자를 결합

### 5.1.9 오버 플로 연산자

- 스위프트는 기본 연사자를 통해 오버플로에 대비할 수 있도록 준비

### 5.1.10 기타 연산자

- 기타 연산자

## 5.2 연산자 우선순위와 결합방향

- 연산자 우선순위를 지정해 둠
- 연산하는 결합 방향도 지정됨

## 5.3 사용자 정의 연산자

- \*이번 사용자 정의 연산자 파트는 함수 클래스 매세드 등의 선수개념이 필요함\*

### 5.3.1 전위 연산자 정의와 구현

### 5.3.2 후위연산자 정의와 구현

### 5.3.3 중위 연산자 정의와 구현