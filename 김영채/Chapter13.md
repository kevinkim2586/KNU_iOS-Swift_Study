# Chapter 13 : 클로저 (Closure)
- Closure
- 스위프트는 Closure, Generics, Protocol, Monad 등이 결합되어 강력한 언어가 됨
- 람다 (Lambda) 와 유사

→ 일정 기능을 하는 코드를 하나의 블록으로 모아놓은 것을 말한다

→ 그러니까 하나의 코드 블럭이라 보면 됨

→ 함수는 클로저의 한 형태일 뿐이다 (이름이 있는 클로저일 뿐)

- 클로저는 변수나 상수가 선언된 위치에서 참조(Reference)를 획득(Capture)하고 저장 가능

    = 변수나 상수의 Closing이라 함 

**3가지 종류의 Closure**

1. 이름이 있으면서 어떤 값도 획득하지 않는 전역함수의 형태
2. 이름이 있으면서 다른 함수 내부의 값을 획득할 수 있는 중첩된 함수의 형태
3. 이름이 없고 주변 문맥에 따라 값을 획득할 수 있는 축약 문법으로 작성한 형태

 

함수와 클로저의 차이점에 대해 간단히 살펴보도록 한다.

`Function`

- `func` 키워드를 통해 정의한다.
- 이름을 갖는다.
- `in` 키워드가 존재하지 않는다.

`Closure`

- `func` 키워드가 존재하지 않는다.
- 이름을 갖지 않는다.
- `in` 키워드를 통해 인자 & 반환타입과 몸체를 분리한다.

ex)

```swift
let add: (Int, Int) -> Int

//add에 클로저 정의
add = { (a: Int, b: Int) -> Int in
    return a + b
}

var result = add(2,3)
print(result)
//5

//Int형 매개변수 2개가 들어오고 반환 타입이 Int 인 method 클로저가 매개변수로 들어온 것
//함수형 프로그래밍에서는 함수도 일급 객체이기 때문에 매개변수로 활용가능
func calculate(a: Int, b: Int, method: (Int, Int) -> Int) -> Int{
    return method(a,b)
}

var calculated: Int

calculated = calculate(a: 50, b: 10, method: add)
print(calculated)
//60
```

ex)

```swift
func filterNumber(closure: (Int)->Bool, numbers: [Int]) -> [Int]{
    
    var list = [Int]()
    for num in numbers{
        
        if(closure(num)){
            list.append(num)
        }
    }
    return list
}

let filteredList = filterNumber(closure: { (num) -> Bool in
    return num<5
}, numbers: [1,2,3,4,5,10])

print(filteredList)
//[1,2,3,4]

//or

func greaterThanThree(value: Int)->Bool{
    return value>3
}

//매개변수로 함수가 들어간 모습
//똑같이 Int 가 들어오고 반환타입이 Bool 이니까 가능
let filteredList = filterNumber(closure: greaterThanThree, numbers: [10,5,1,2,0])
print(filteredList)
```

### 기본 클로저

```swift
let names: [String] = ["wizplan", "eric", "yagom", "jenny"]

func backwards(first: String, second: String) -> Bool {
	print("\(first) \(second) 비교중")
	return first>second      //bool값
}

let reversed: [String] = names.sorted(by: backwards) //함수가 매개변수로 들어온 모습
print(reversed

//원래 스위프트 라이브러리에서 sorted ( ) 함수에서 첫 번째 요소와 두 번째 요소를 비교해 bool 
//값을 return 함. 즉, 그것과 똑같은 형태의 backwards 함수를 정의해서 들어갈 수 있는 것
```

- 위 backwards 함수를 클로저로 표현한 것은 아래에 있음

```swift
let reversed: [String] = names.sorted(by: { (first: String, second: String)-> Bool in
	return first > second
})

print(reversed)
//["yagom", "wizplan", "jenny", "eric"]
```

→ 이렇게 클로저 형태로 작성하게 되면 이전의 backwards 함수가 어디에 있는지, 어떻게 구현되어 있는지 찾아다니지 않아도 된다.

### 후행 클로저 (Trailing Closure)

- 클로저가 조금 길어지거나 가독성이 조금 떨어진다 싶으면 Trailing Closure 를 사용
- 소괄호를 닫은 후 작성 가능 (자동 완성)

```swift
let reversed: [String] = names.sorted() { (first: String, second: String)->Bool in
	return first > second
}

//sorted(by:) 메서드의 소괄호까지 생략 가능
let reversed: [String] = names.sorted { (first: String, second: String)->Bool in
	return first > second
}
```

### 클로저 표현 간소화

- **문맥을 이용한 타입 유추**

→ 전달인자로 보냈다는 것 자체가 매개변수의 타입과 반환값을 준수한걸로 보기 때문에 굳이 아래에 String , String, → Bool 다 작성할 필요 없음. 물론 준수하지 않은걸 넣으면 오류

```swift
let reversed: [String] = names.sorted { (first, second) in
	return first > second
}
```

- **단축 인자 이름**

→ 첫 번째 전달인자부터 $0, $1, $2, $3... 수서로 $와 숫자의 조합으로 표현

→ 이걸 사용하면 매개변수 및 반환 타입과 실행 코드를 구분하기 위해 있었던 in 키워드를 사용할 필요가 없어짐 

```swift
let reversed: [String] = names.sorted{
	return $0 > $1
}
```

- **암시적 반환 표현**

→ if 클로저가 반환 값을 갖는 클로저이고, 클로저 내부의 실행문이 단 한 줄이면 return 키워드 삭제 가능

```swift
let reversed: [String] = names.sorted { $0 > $1 }
```

### 값 획득 (Capturing Values)

- 클로저는 자신이 정의된 위치의 주변 문맥을 통해 상수나 변수를 획득할 수 있다.
- 주변에 정의한 상수나 변수가 없더라도 해당 상수/변수의 값을 자신 내부에서 참조 및 수정 가능

```swift
//아래 함수의 반환 타입은 ()->Int 함수 객체다
func makeIncrementer(forIncrement amount: Int) -> () -> Int {
  var runningTotal = 0
  func incrementer() -> Int {
    runningTotal += amount
    return runningTotal
  }
  return incrementer
}

let plusTen = makeIncrementer(forIncrement: 10)
//plusTen 은 Int 타입이 아니라 ( ) -> Int 타입이다. (함수 객체)
//즉 반환하는 함수는 매개변수를 받지 않고 반환 타입은 Int인 함수다
//호출할 때마다 Int 타입의 값을 반환함
let plusSeven = makeIncrementer(forIncrement: 7)

// 함수가 각기 실행되어도 실제로는 변수 runnigTotal과 amount가 캡쳐되서 그 변수를 공유하기 때문에 누적된 결과를 가진다.
let plusedTen = plusTen() // 10
let plusedTen2 = plusTen() // 20
// 다른 클로저이기 때문에 고유의 저장소에 runningTotal과 amount를 캡쳐해서 사용한다.
let plusedSeven = plusSeven() // 7
let plusedSeven2 = plusSeven() // 14
```

- incrementer ( ) 는 반드시 중첩된 형태로 있어야 값 획득이 가능하다. 어디 동떨어진 곳에 있으면 내부의 변수를 찾지 못한다.

### **Closure 는 Reference Type**

---

**출처: [https://github.com/yeojaeng/Study_Log/blob/master/iOS/Contents/Closure.md](https://github.com/yeojaeng/Study_Log/blob/master/iOS/Contents/Closure.md)

기본적으로 클로저는 `Reference Type`이다.

앞서 우리는 클래스와 구조체는 각각 `Reference Type` & `Value Type` 이라고 배웠던 기억이 있다.

맞다, 클래스를 공부할적에 배웠던 `Reference Type`과 동일한 의미다.

즉, `Call By Reference` 방식으로 객체를 가리키고 있는 메모리의 주소값을 복사해오는 방식이다.

매개변수로 클로저에서 사용되는 매개변수는 값을 복사하는게 아니라 해당 값을 참조하여 사용하게 된다.

말로 표현하니 필자 또한 잘 와닿지가 않는다..

코드로 표현해보자!

`var a:Int = 1
var b:Int = 0

var closure = {print(a,b)}

closure()       // 1, 0

a = 0
b - 1

closure()       // 0, 1`

위와 같이 클로저 내부의 `a,b`는 외부의 `a,b`값을 `CBV` 방식으로 참조하여 가져온다.

이러한 방식으로 값을 참조하게 된다면 외부에서 값이 변경되면 참조하고 있는 값 또한 즉각 변경된다.

**꼭 명심하자, 클로저는 기본적으로 `Reference Type`이다!**

### 탈출 클로저 (Escaping Closure)

- 함수의 전달인자로 전달한 클로저가 함수 종료 후에 호출될 때 클로저가 함수를 **탈출한다고 표현**
- 매개변수 뒤에 : @escaping
- escaping 키워드가 없으면 매개변수로 사용되는 클로저는 기본으로 비탈출 클로저임

**함수의 인자로 전달된 클로저가 함수가 반환된 후 실행 되는 클로저**을 의미합니다.

(말 그대로 전달인자로 받은 클로저가 함수 내부 scope 안에서 실행되는 것이 아니라 이를 탈출해서 다른 어딘가로 가는 것 입니다.)

다음과 같은 경우를 탈출한다고 볼 수 있습니다.

1. 전달받은 클로저가 클로저 함수 외부로 다시 반환되거나
2. 외부 글로벌 변수에 저장되는 경우

이 경우 **@escaping**을 붙여줘야 하고 그렇지 않으면 compile 에러를 냅니다.

이는 기존에 우리가 알고 있던 변수의 scope 개념을 무시한다.

** 추가 이해 필요
