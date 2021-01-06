# Chapter7 함수

>함수 대부분은 작업의 가장 작은 단위이자 하나의 작은 프로그램임<br>

## 7.1 함수와 메서드

- 함수나 메서드는 기본적으로 같다
- 구제체, 클래스, 열거형 등 특정 타입에 연관되어 사용하는 함수를 메서드
- 모듈 전체에서 전역적으로 사용할 수 있는 함수를 그냥 함수 라고 함

## 7.2 함수의 정의의 호출

- 함수와 메서드는 정의하는 위치와 호출되는 범위만 다를 뿐 정의하는 키워드와 구현하는 방법은 같다.
- 오버라이드와 오버로드를 지원함

### 7.2.1 기본적인 함수의 정의와 호출

- 매개변수와 반환타입등을 사용하여 함수를 정의한다.
- 함수를 정의하는 키워드는 func이며, 반환 타입은 -> 통해 명시한다. 반환을 위한 키워드는 return이다.

```swift
func hello(name: String) -> String{
	return "Hello \(name)"!
}

let helloJenny: String = hello(name:"Jenny")
print(helloJenny) // Hello Jenny!

func introduce(name: String) -> String{
	// [retrun "제 이름은 "+ name + "입니다"]와 같은 동작을 합니다.
}

let introduceJenny: String = introduce(name:"Jenny")
print(introduceJenny) // 제 이름은 Jenny 입니다.

```
- 위와 같이 return 키워드 생략 가능

### 7.2.2 매개변수

- 매개변수가 없는 함수와 매개변수가 여러 개인 함수
- 매개변수가 필요 없다면 매개변수 위치를 공란으로 배워둔다.
- 함수의 매개변수가 여러 개 있을 때는 함수 호출 시 매개변수 이름을 붙여주고 콜론(:)을 적어준 후 전달인자를 보내야함

```swift
// 매개 변수가 여러개인 함수의 사용
func sayHello(myName: String, yourName: String) -> String{
	return "Hello \(yourName)! I'm \(myName)"
}

print(sayHello(myName: "yagom", yourName: "Jenny")) // Hello Jenny! I'm yagom
```

- 매개변수 이름과 더불어 전달인자 레이블을 지정 가능하다.

```swift
// from과 to라는 전달인자 레이블이 있으며
// myName과 name이라는 매개변수 이름이 있는 sayHello 함수
func sayHello(from myName:String, to name:String) -> String{
	return "Hello \(name)! I'm \(myName)"
}

print(sayHello(from: "yagom", to: "Jenny")) // Hello Jenny! I'm yagom
```

- 전달인자 레이블을 사용하고 싶지 않으면 와일드카드 식별자를 사용

```swift
func sayHello(_ name: String, _ times: Int)->String{
	var result: String = ""

	for _ in 0..<times{
		result += "Hello \(name)!" + " "
	}
	return result
}

print(sayHello("Chope", 2)) // Hello Chope! Hello Chope!
```

- 가변 매개변수와 입출력 함수
- 매개변수로 몇개의 값이 들어올지 모를 때, 가변 매개변수를 사용 함

```swift
func sayHelloFriend(me: String, friends names: String...) -> String{
	var result: String = ""

	for friend in names{
		result += "Hello \(friend)!" + ""
	}
	result += "I'm" + me + "!"
	return result
}

print(sayHelloFriend(me:"yagom", friends: "johansson, Jay, Wizplan"))
```

### 7.2.3 반환이 없는 함수

- 반환값이 없는 함수라면 반환 타입을 void로 표기하거나 아에 생략

```swift
func sayHelloWorld() {
    print("Hello, world!")
}
sayHelloWorld()

func sayGoodbye() -> Void {
    print("Good bye")
}
sayGoodbye()
```

### 7.2.4 데이터 타입으로서의 함수

- 함수는 일급 객체이므로 하나의 데이터 타입으로 사용 가능

```swift
func sayHello(name: String, times: Int) -> String{ }

// sayHello의 함수의 타입은 (String, Int) → String 이다

func sayHelloWorld() { }

// sayHellowWorld함수의 타입은 (Void) → Void 이다
```

## 7.3 중첩 함수

- 함수안에 함수를 넣을 수 있다.

```swift
typealias MoveFunc = (Int) -> Int

func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc {
    func goRight(_ currentPostion: Int) -> Int {
        return currentPosition + 1
    }
    
    func goLeft(_ currentPosition: Int) -> Int {
        return currentPosition - 1
    }
    
    return shouldGoLeft ? goLeft : goRight
}

var position: Int = -4 // 현 위치

// 현위치가 0보다 작으므로 전달되는 인자 값은 false가 된다.
// 그러므로 goRigjt(_:) 함수가 할당될 것이다.

let moveToZero: MoveFunc = functionForMove(position > 0)

// 원머에 도착하면(현위치가 0이면) 반복문이 종료됩니다.
while position != 0 {
    print("\(position)...")
    position = moveToZero(position)
}
print("원점 도착!")
// -4
// -3
// -2
// -1
// 원점 도착!
```

## 7.4 종료되지 않는 함수

- 정상적으로 끝나지 않는 함수
- 비반환 함수 또는 비반환 메서드라고 부른다.
- 비반환 함수는 안에서 오류를 던진다든가 중대한 시스템 오류를 보고하는 등의 일을 하고 프로세스 종료 시 사용
- 비반환 함수의 반환 타입은 Never라고 명시

```swift
func crashAndBurn() -> Never{
	fatalError("Something very, very bad happend")
}

crashAndBurn() // 프로세서 종료 후 오류 보고

func someFunction(isAllIsWell: Bool){
	guard isAllIsWell else{
		print("마을에 도둑이 들었습니다.")
		crashAndBurn()
	}
	print("All is well")
}

someFunction(isAllIsWell: true) // All is well
someFunction(isAllIsWell: false) // 마을에 도둑이 들었습니다.
// 프로세서 종료 후 오류 보고
```

## 7.5 반환값을 무시할 수 있는 함수

- 만약 반환값을 의도적으로 사용하지 않을때 함수의 반환값을 무시해도 된다는 
- \*@discardabelResult\* 선언 속성을 사용하면된다.

```swift
@discardableResult func discardableResultsay(_ something: String) -> String {
    print(something)
    return something
}

discardableResultsay("Hello") // 경고를 내보내지 않
```

