# Chapter8 옵셔널

- 안전성(Safe)를 문법으로 담보하는 기능
- 옵셔널이면: **It can be NULL**
- 옵셔널이 아니면: **It can NOT be NULL**
- 위와 같이 문서에 명시하지 않아도 옵셔널 하나만으로 이 의미를 충분히 전달할 수 있다

## 8.1 옵셔널 사용

~~~ swift
var myName: String? = "yagom"
print(myName)                   // yagom
myName = nil
print(myName)                   // nil
~~~

**옵셔널은 어떤 상황에 사용할까?**
- 함수에 전달되는 전달인자의 값이 잘못된 값일 경우 제대로 처리하지 못했음을 nil을 반환하여 표현
- 매개변수를 굳이 넘기지 않아도 된다는 뜻으로 매개변수의 타입을 옵셔널로 정의할 수도 있음
- -> 암묵적인 커뮤니케이션

## 8.2 옵셔널 추출

옵셔널의 값을 옵셔널이 아닌 값으로 추출하는 방법

### 8.2.1 강제 추출

! 를 사용해서 강제 추출할 수 있다

~~~ swift
var myName: String? = "yagom"
var yagom: String = myName!
myName = nil
yagom = myName!                 // 런타임 오류!
~~~

### 8.2.2 옵셔널 바인딩

옵셔널에 값이 있다면 옵셔널에서 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할당해서 옵셔널이 아닌 형태로 사용할 수 있도록 해줌  

if또는 while 구문 등과 결합하여 사용

~~~ swift
var myName: String? = "yagom"

if let name = myName {
    print("My name is \(name)")
} else {
    print("myName == nil")
}
// My name is yagom~~~
~~~

### 8.2.3 암시적 추출 옵셔널

**옵셔널 바인딩으로 매번 값을 추출하기 귀찮거나 로직상 nil 때문에 런타임 오류가 발생하지 않을 것 같다는 확신이 들 때**

! 사용

~~~ swift
var myName: String! = "yagom"
var name: String = myName
print(name)                     // yagom
myName = nil

// 옵셔널 바인딩 또한 사용가능
~~~
