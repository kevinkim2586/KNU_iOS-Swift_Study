# Chapter 7 : 함수
- 재정의 (오버라이드)와 중복 정의 (오버로드) 모두 지원

```swift
func hello(name: String) -> String{
	return "Hello \(name)"!
}

let helloJenny: String = hello(name:"Jenny")
print(helloJenny)
```

** return 키워드 생략 역시 가능

→ When? : 함수 내부의 코드가 단 한 줄의 표현이고, 그 표현의 return 값이 반환 타입과 일치할 때 가능

```swift
func introduce(name: String) -> String{
	"제 이름은 " + name + "입니다."
}
```

### 매개변수 관련

** 함수의 매개변수가 여러 개 있을 때 : 함수 호출 시 매개변수 이름을 붙여주고 콜론(:)을 적어준 후 전달인자를 보내줘야 함

```swift
func sayHello(myName: String, yourName: String) -> String{
	return "Hello \(yourName)! I'm \(myName)"
}

print(sayHello(myName: "yagom", yourName: "Jenny"))
//Parameter name 사용
```

- 매개변수 이름과 더불어 **전달인자 레이블 (Argument label)** 역시 사용 가능

func 함수 이름(전달인자 레이블 매개변수 이름: 매개변수 타입) → 반환타입){ }

```swift
func sayHello(from myName:String, to name:String) -> String{
	return "Hello \(name)! I'm \(myName)"
}
//함수 내에서는 매개변수로 control 함
//함수 내부에서는 전달인자 레이블 사용 불가
//함수 호출 할 시 매개변수 이름 사용 불가
```

→ 전달인자 레이블을 사용하고 싶지 않으면 와일드카드 식별자 사용

```swift
func sayHello(_ name: String, _ times: Int)->String{
	var result: String = ""

	for _ in 0..<times{
		result += "Hello \(name)!" + " "
	}
	return result
}

print(sayHello("Chope", 2))
```

** 전달인자 레이블을 변경하면 함수의 이름 자체가 변경된다

→ Overload 로 동작 (다른 두 함수)

### 매개변수 기본값 설정

- 매개변수가 전달되지 않으면 기본값을 사용

```swift
func sayHello(_ name: String, times: Int = 3)->String{
. 
.
.
}

print(sayHello("Hana"))
```

### 가변 매개변수

- 매개변수로 몇 개의 값이 들어올지 모를 때, 가변 매개변수 사용 가능
- 마치 배열처럼 이용 가능

```swift
func sayHello(me: String, friends names: String...) -> String{
	
	var result: String = ""

	for friend in names{
		result += "Hello \(friend)!" + ""
	}
.
.
.
}

print(sayHello(me:"yagom", friends: "Kevin, Sungjo, Wayne"))
```

### 반환이 없는 함수

```swift
func sayHelloWorld() -> Void{
	print("Hello world")
}

//or

func sayHelloWorld(){
	print("Hello world")
}
```

### 데이터 타입으로서의 함수

- 스위프트에서 함수는 일급 객체이므로 하나의 데이터 타입으로 사용 가능

```swift
func sayHello(name: String, times: Int) -> String{ }
```

위 함수의 타입은 (String, Int) → String 이다

```swift
func sayHelloWorld() { }
```

위 함수의 타입은 (Void) → Void 이다

or ( ) → Void

or ( ) → ( )

```swift
typealias CalculateTwoInts = (Int, Int) -> Int

func addTwoInts(_ a: Int, _ b: Int) -> Int{
	return a + b
}

var mathFunction: CalculateTwoInts = addTwoInts

print(mathFunction(2,5))
```

### 종료되지 않는 함수

- return 되지 않는 함수
- 오류를 던진다든가 중대한 시스템 오류를 보고하는 등의 일을 하고 프로세스 종료 시 사용
- 반환 타입 = Never

```swift
func crashAndBurn() -> Never{
	fatalError("error")
}

func someFunction(isAllIsWell: Bool){
	guard isAllIsWell else{
		print("very bad thing happened")
		crashAndBurn()
	}
	print("All is well")
}

someFunction(isAllIsWell: true)
//All is well
someFunction(isAllIsWell: false)
//very bad thing happened
//error
```
