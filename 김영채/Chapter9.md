# Chapter 9 : 구조체와 클래스 
- 구조체의 인스턴스 → 값 타입
- 클래스의 인스턴스 → 참조 타입

### 구조체

```swift
struct BasicInformation{
	var name: String
	var age: Int
}
```

→ 인스턴스 생성 후 프로퍼티 값에 접근하기 위해서는 마침표 (.) 사용

- let, var 모두 사용 가능

```swift
var info: BasicInformation = BasicInformation(name: "yagom", age: 25)
//BasicInformation() 초기화를 해야하 함

let info2: BasicInformation = BasicInformation(name: "jack", age: 21)
info2.name = "alex"   //상수니까 변경 불가
```

### 클래스

- 상속을 받을 때에는 클래스 이름 뒤에 콜론 ( : ) 을 써주고 부모클래스 이름 명시

```swift
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0
}
//기본값을 0.0으로 지정해주는 것.
//프로퍼티만 정의해준 것임
```

- 인스턴스 생성 후 초기화할 때는 ***이니셜라이저*** 를 사용한다.

** 위 코드 예시에서는 class property 값의 기본값이 설정되어 있기 때문에 initialize 할 때 따로 전달인자를 "꼭" 넘길 필요는 없음 / 이후 수정 가능

note: 스위프트에서는 "객체"라는 용어보다는 인스턴스라는 용어를 더 사용함. 

- 구조체와는 다르게 instance 는 참조 타입이므로 인스턴스를 let 으로 선언해도 내부 property 값 변경 가능

```swift
var yagom: Person = Person()
yagom.height = 12.3
yagom.weight = 12.42

let jenny: Person = Person()
jenny.weight = 24.3
```

### Deinitializer

- 인스턴스를 더 이상 참조할 필요가 없을 때 메모리에서 해제시키는 용도
- deinit 라는 메서드 호출
- 매개변수 X, 반환 값 X

```swift
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0

	deinit{
		print("소멸")
	}
}

var yagom: Person? = Person()
yagom = nil
```

→ 따로 deinit 메소드를 호출하는 것이 아니라 인스턴스에 nil 값을 할당하면 알아서 deinit 을 호출하여 소멸함

### 구조체와 클래스의 다른 점

- 구조체는 상속 불가능
- 타입캐스팅은 클래스의 인스턴스에만 허용
- Deinit 은 클래스만 가능
- 참조 횟수 계산(Reference Counting)은 인스턴스만 가능

** 가장 큰 차이점 : **값 타입이냐 참조 타입이냐**

### 값 타입 & 참조 타입

→ "무엇이 전달되는가?"

- 값 타입 : 전달될 값이 복사 (call by value)
- 참조 타입 : 참조 (주소) 가 전달 (call by reference) ex. 포인터

    → 그러나 여기서 * 는 사용하지 않음

    구조체 / 값 타입 ex)

```swift
struct BasicInformation{
	let name: String
	var age: Int
}

var yagomInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
yagomInfo.age = 100

//friendInfo 에 yagomInfo 의 값을 "복사"하여 할당 (extra memory)
var friendInfo: BasicInformation = yagomInfo

print("yagom's age: \(yagomInfo.age)")
print("friend's age: \(friendInfo.age)")

friendInfo.age = 25
//friendInfo 는 별도의 메모리를 할당 받았기 때문에 yagomInfo.age 에는 값 변경 X

func changeBasicInfo(_ info: BasicInformation){
	var copiedInfo: BasicInformation = info
	copiedInfo.age = 1
}

changeBasicInfo(yagomInfo)
print("yagom's age: \(yagomInfo.age)")
//call by reference 가 아닌 call by value 이기 때문에 따로 값이 변경되지 않고 그대로 100임
```

클래스 / 참조 타입 ex)

```swift
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0
}

var yagom: Person = Person()
var friend: Person = yagom
//yagom의 참조를 할당한다

friend.height = 182.3
print("yagom height: \(yagom.height)")
//friend.height 을 바꿨음에도 불구하고 yagom.height 도 같이 변경된다
//같은 메모리를 참조하기 때문

func changePersonInfo(_ info: Person){
	info.height = 155.2
}

changePersonInfo(yagom)
print("yagom height: \(yagom.height)")
//Call by reference 이기 때문에 실제 값이 변화되어 155.2 출력
```

### 식별 연산자

- 클래스의 인스턴스끼리 참조가 같은지 확인할 때 사용
- Identity Operator

```swift
var yagom: Person = Person()
let friend: Person = yagom
let anotherFriend: Person = Person()

print(yagom === friend)  //true
print(yagom === anotherFriend) //false
print(friend !== anotherFriend) //true
```

** 스위프트의 기본 데이터 타입은 모두 구조체로 되어 있음

```swift
public struct String{
	public init()
}
```

- String, Int, Array, Dictionary, Set 등등

→ 이는 모든 기본 데이터 타입이 값 타입이라는 것
