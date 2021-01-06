# Chapter6 흐름 제어

## 6.1 조건문

if와 switch구문

### 6.1.1 if 구문

> **if 구문은 조건의 값이 꼭 Bool 타입이어야 한다**

~~~ swift
if condition {
    statements
} else {
    statements
}
~~~

### 6.2.2 switch 구문

> switch 구문의 입력 값으로 숫자 표현 외에도 문자, 문자열, 열거형, 튜플, 범위, 패턴이 적용된 타입 등 다양한 타입의 값도 사용 가능

~~~ swift
switch value {
case pattern1:
    code
case pattern2:
    code
case pattern3, pattern4, pattern5:
    code
default:
    code
}
~~~

## 6.2 반복문

for-in, while 그리고 repeat-while

### 6.2.1 for-in

~~~ swift
for item in items {
    code
}
~~~

### 6.2.2 while

> condition 은 무조건 Bool 타입이어야 한다

~~~ swift
while condition {
    code
}
~~~

### 6.2.3 repeat-while

> condition 은 무조건 Bool 타입이어야 한다

~~~ swift
repeat {
    code
} while condition
~~~

