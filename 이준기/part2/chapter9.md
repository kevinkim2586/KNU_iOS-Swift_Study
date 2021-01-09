# Chapter9 구조체

> 구조체와 클래스는 프로그래머가 데이터를 용도에 맞게 묶어 표현하고자 할때 유용<br>
> 구조체와 클래스는 참조 타입

## 9.1 구조체

```swift

### 9.1.1 구조체 정의

- \* struct키워드로 정의\*

```swift
struct BasicInformation{
	var name: String
	var age: Int
}
```

### 9.1.2 구조체 인스턴스의 생성 및 초기화

- 인스턴스가 생성되고 초기화 된후 프로퍼티값에 접근하고 싶으면 마침표(.)를 사용하면 됨
-  let 으로 선언하면 변경 x, var로 선언하면 변경 o

```swift
// 프로퍼티 이름(name, age)으로 자동 생성된 이니셜라이저를 사용하여 구조체를 생성
var yagominfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
yagominfo.age = 100 // 변경가능!

let sebainfo: BasicInformation = BasicInformation(name: "seba", age: 99)
sebainfo.name = "alex"   // 변경 불가
```

## 9.2 클래스

- 상속없이 단독으로 정의 가능

### 9.2.1 클래스 정의

- 클래스를 정의할때는 class 라는 키워드를 사용

```swift
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0
}
```

### 9.2.2 클래스 인스턴스의 생성과 초기화

- 클래스를 정의한 후 인스턴스를 생성하고 초기화하고자 할때는 기본적인 이니셜라이저를 사용함
- 클래스의 기본값이 지정된 경우 전달 인자를 통해 초깃값을 전달해 주지 안해도됨

```swift
var yagom: Person = Person()
yagom.height = 123.4
yagom.weight = 123.4

let jenny: Person = Person()
jenny.height = 123.4
jenny.weight = 123.4
```

### 9.2.3 클래스 인스턴스의 소멸

- 클래스의 인스턴스는 참조 타입이므로 더는 참조할 필요가 없을 때 메모리에서 해체됨
- 이 과정을 소멸이라고 하는데 소멸되기 직전 deinit라는 메서드가 호출됨
- deinit 메서드는 \*디이니설라이저\*라고 부름
- 클래스 하나당 하나만 구현 할수 있으며 매개변수와 반환 값을 가질 수 없다

```swift
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0

	deinit{
		print("person 클래스의 인스턴스가 소멸됩니다.")
	}
}

var yagom: Person? = Person()
yagom = nil // person 클래스의 인스턴스가 소멸됩니다.
```

## 9.3 구조체와 클래스의 차이

- 구조체와 클래스의 같은점
> 값을 저장하기 위해 프로퍼티를 정의 <br>
> 기능실행을 위해 메서드를 정의 <br>
> 서브 스크립트 문법을 통해 구조체 또는 클래스가 갖는 값에 접근하도록 서브스크립트를 정의할수 있음 <br>
> 이니설라이즈를 정의할수 있음 <br>
> 초기구현과 더불어 새로운 기능 추가를 위해 익스텐션을 통해 확장할 수 있음 <br>
> 특정 기능을 실행하기 위해 특정 프로토콜을 준수할 수 있다. <br>

- 구조체와 클래스의 다른점
> 구조체는 상속 할수 없다. <br>
> 타입캐스팅은 클래스의 인스턴스에만 허용 <br>
> 디이니셜라이즈는 클래스의 인스턴스에만 활용 <br>
> 참조 횟수 계산은 인스턴스만 적 <br>

### 9.3.1 값 타입과 참조 타입

- 구조체는 값 타입
- 클래스는 참조 타입

> 값 타입 : 전달될 값이 복사 <br>
> 참조 타입 : 참조 (주소) 가 전달 <br>

```swift
//구조체
struct BasicInformation{
	let name: String
	var age: Int
}

var yagomInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
yagomInfo.age = 100

// yagomInfo 의 값을 복사하여 할당
var friendInfo: BasicInformation = yagomInfo

print("yagom's age: \(yagomInfo.age)") //100
print("friend's age: \(friendInfo.age)") //100

friendInfo.age = 999

print("yagom's age: \(yagomInfo.age)") //100 - yagom의 값은 변동 없습니다.
print("friend's age: \(friendInfo.age)") 
//999- friendInfosms yagomInfo의 값을 복사해왔기 때문에 별개의 값을 가짐
```

```swift
//클래스
class Person{
	var height: Float = 0.0
	var weight: Float = 0.0
}

var yagom: Person = Person()
var friend: Person = yagom //yagom의 참조를 할당한다

print("yagom's height: \(yagom.height)") // 0.0
print("friend's height: \(friend.height)") // 0.0

friend.height = 185.5
print("yagom height: \(yagom.height)")
// 185.5-friend는 yagom을 참조하기 때문에 값이 변동
```
\*식별 연산자\*
- 클래스의 인스턴스끼리 참조가 같은지 확인할 때 사용

```swift
var yagom: Person = Person()
let friend: Person = yagom // yagom의 참조를 할당
let anotherFriend: Person = Person() // 새로운 인스턴스를 생성

print(yagom === friend)  //true
print(yagom === anotherFriend) //false
print(friend !== anotherFriend) //true
```

### 9.3.2 스위프트의 기본 데이터 타입은 모두 구조체

## 9.4 구조체와 클래스 선택해서 사용하기

- 아래 해당하는 조건 중 하나 이상에 해당한다면 구조체를 사용하는 것을 권장함
- 1.연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
- 2.캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할때
- 3.구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
- 4. 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 
