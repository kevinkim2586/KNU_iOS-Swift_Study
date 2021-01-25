# Chapter25 패턴

> **패턴**: 단독 또는 복합 값의 구조를 나타내는 것  
> **패턴 매칭**: 코드에서 어떤 패턴의 형태를 찾아내는 행위  

> 값을 해체(추출)하거나 무시하는 패턴  
> -> 와일드카드 패턴, 식별자 패턴, 값 바인딩 패턴, 튜플 패턴  
> 패턴 매칭을 위한 패턴  
> -> 열거형 케이스 패턴, 옵셔널 패턴, 표현 패턴, 타입캐스팅 패턴  

## 25.1 와일드카드 패턴

> _ 사용  
> "이 자리에 올 것이 무엇이든간에 상관하지 마라"

## 25.2 식별자 패턴

> 변수 또는 상수의 이름에 알맞는 값을 어떤 값과 매치시키는 패턴  

let someValue: Int = 32에서 Int와 32의 타입이 매치되면 somValue는 42라는 값의 식별자가 되므로 식별자 패턴이 성립한다

## 25.3 값 바인딩 패턴

~~~ swift
let yagom = ("yagom", 99, "Male")
let (a, b, c) = yagom // a, b, c를 yagom의 각각읭 요소와 바인딩한다
// a: "yagom", b: 99, c: "Male"
~~~

## 25.4 튜플 패턴

> 소괄호 내에 쉼표로 분리하는 리스트

~~~ swift
let (x, y): (Int, Int) = (1, 2)
~~~

## 25.5 열거형 케이스 패턴

> 값을 열거형 타입의 case와 매치시킨다

~~~ swift
let someValue: Int = 30
if case 0...100 = someValue {
    print("0 <= \(someValue) <= 100")
} // 0 <= 30 <= 100
~~~

## 25.6 옵셔널 패턴

> 옵셔널 또는 암시적 추출 옵셔널 열거형에 감싸져 있는 값을 매치시킬 때 사용  

## 25.7 타입캐스팅 패턴

> is 패턴: switch문의 case레이블에서만 사용할 수 있다. 값의 타입이 is 우측에 쓰여진 타입 또는 그 타입의 자식클래스 타입이면 값과 매치시킨다  
> as 패턴: 값의 타입이 as 우측에 쓰여진 타입 또는 그 타입의 자식클래스 타입이면 값과 매치시킨다  

~~~ swift
let someValue: Any = 100

switch someValue {
// 타입이 Int인지 확인하지만 캐스팅된 값을 사용할 수는 없습니다.
case is String: print ("It's String!")
    
// 타입 확인과 동시에 캐스팅까지 완료되어 value에 저장됩니다.
// 값 바인딩 패턴과 결합된 모습입니다.
case let value as Int: print(value + 1)
default: print("Int도 String도 아닙니다.")
}   // 101
~~~

## 25.8 표현 패턴

> 표현식의 평가한 결과를 이용하는 것  
> switch구문의 case 레이블에서만 이용가능  
> ~= 연산자의 연산 결과가 true를 반환하면 매치시킨다

~~~ swift
// 코드 25-8 표현 패턴의 사용
switch 3 {
case 0...5: print("0과 5 사이")
default: print("0보다 작거나 5보다 큽니다.")
}   // 0과 5 사이


var point: (Int, Int) = (1, 2)

// 같은 타입 간의 비교이므로 == 연산자를 사용하여 비교할 것입니다.
switch point {
case (0, 0): print("원점")
case (-2...2, -2...2): print("(\(point.0), \(point.1))은 원점과 가깝습니다.")
default: print("point (\(point.0), \(point.1))")
}   // (1, 2)는 원점과 가깝습니다.


// String 타입과 Int 타입이 매치될 수 있도록 ~= 연산자를 정의합니다.
func ~= (pattern: String, value: Int) -> Bool {
    return pattern == "\(value)"
}

point = (0, 0)

// 새로 정의된 ~= 연산자를 사용하여 비교합니다.
switch point {
case ("0", "0"): print("원점")
default: print("point (\(point.0), \(point.1))")
}
// 원점


struct Person {
    var name: String
    var age: Int
}

let lingo: Person = Person(name: "Lingo", age: 99)

func ~= (pattern: String, value: Person) -> Bool {
    return pattern == value.name
}

func ~= (pattern: Person, value: Person) -> Bool {
    return pattern.name == value.name && pattern.age == value.age
}


switch lingo {
case Person(name: "Lingo", age: 99): print("Same Person!!")
case "Lingo": print("Hello Lingo!!")
default: print("I don't know who you are")
}   // Same Person!!
~~~

