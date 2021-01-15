# Chapter 18 : 상속
- 기반 클래스 (Base Class) → 다른 클래스로부터 상속을 받지 않은 클래스
- 어떤 클래스의 자식클래스가 다른 클래스의 부모클래스가 될 수 있음

### 재정의

- 자식클래스에서 자신만의 기능으로 변경하여 사용하는 것 → Override
- 재정의를 하는데도 여전히 부모클래스의 특성을 활용하고 싶으면 super 사용
- 부모 버전의 서브스크립티에 접근하고 싶다면 → super[index]

```swift
class Student: Person{
	override func speak(){
		print("i am student")
	}
}
```

### 프로퍼티 재정의

- 부모로부터 물려받은 인스턴스 프로퍼티나 타입 프로퍼티 재정의 역시 가능

→ 다만 프로퍼티 재정의 시 저장 프로퍼티로 재정의는 불가능

→ 프로퍼티 재정의 뜻 : getter, setter, property observer 등을 재정의한다는 것

- read & write 가 가능한 프로퍼티의 부모클래스 → read 전용으로 재정의 불가 (자식)
- read 전용 프로퍼티의 부모클래스 → read & write 으로 변경 가능 (자식)

```swift
class Person{
	var name: String = ""
	var age: Int = 0
	var koreanAge: Int{
		return self.age + 1
	}
	var introduction: String{
		return "이름: \(name) 나이: \(age)"
	}
}

class Student: Person{
	var grade: String = "F"

	override var introduction: String{
		return super.introduction + "학점: \(self.grade)"
	}
	override var koreanAge: Int{
		get{
			return super.koreanAge
		}
		set{
			self.age = newValue - 1
		}
	}
}

let jay: Student = Student()
jay.name = "jay"
jay.age = 14
jay.koreanAge = 15
print(jay.introduction) //이름: jay 나이: 14 학점: F
print(jay.koreanAge)    // 15
```

### 프로퍼티 감시자 재정의

- didSet, willSet 이런 것들도 재정의 가능
- 단, 상수 저장 프로퍼티다 읽기 전용 연산 프로퍼티는 프로퍼티 감시자 재정의 불가능

**프로퍼티 감시자를 자식클래스에서 재정의하더라도, 부모클래스에 정의한 프로퍼티 감시자도 **함께 동작**한다

**프로퍼티의 접근자와 감시자 둘다 한 번에 재정의는 불가능

```swift
override var koreanAge: Int{
	get{
	}
	set{
	}
	didSet{ } //오류 발생!
}
```

### 서브스크립트 재정의

- 자식클래스에서 재정의하려는 서브스크립트라면 부모클래스 서브스크립트의 매개변수와 반환타입이 같아야 함

```swift
override subscript(index: Int) -> Int{
	..
}
```

### 재정의 방지

- 자식클래스에서 재정의하는 것을 제한하고 싶으면 final 키워드 명시

ex. final var, final func, final class, final subscript

→ 클래스를 상속하거나 재정의할 수 없도록 하고 싶어도 역시 final 을 붙여주면 된다. 그러면 자식 클래스 생성 불가능

```swift
class Person{
	final var name: String = ""
 
	final func speak(){
	}
}
```

### 클래스의 이니셜라이저 - 상속과 재정의

### 지정 이니셜라이저와 편의 이니셜라이저 (Designated Initializer & Convenience Initializer)

- 모든 클래스는 하나 이상의 지정 이니셜라이저가 있음
- 편의 이니셜라이저는 초기화를 좀 더 손쉽게 도와주는 역할을 함

    → 지정 이니셜라이저를 자신 내부에서 호출한다.

When to use? → 지정 이니셜라이저의 매개변수가 많아 외부에서 일일이 전달인자를 전달하기 어렵거나 특정 목적에 사용하기 위해서 편의 이니셜라이저 사용 가능

convenience init(매개변수){

초기화 구문

}

### 이니셜라이저 자동 상속

- 대부분의 경우 자식클래스에서 이니셜라이저를 재정의 할 필요 없음
- 자식 클래스에서 별도의 지정 이니셜라이저를 구현하지 않는다면 부모클래스의 지정 이니셜라이저가 자동 상속

```swift
class Person{
	var name: String
	init(name: String){
		self.name = name
	}
}

class Student: Person{
	var major: String= "Swift"
}

```

→ 편의 이니셜라이저도 자동 상속 가능

### 요구 이니셜라이저

- required 수식어 필요
- 이런 경우 이 클래스를 상속받은 자식클래스에서 반드시 해당 이니셜라이저 구현해야 함

```swift
class Person{
	var name: String
	required init(){
		self.name = "Unknown"
	}
}

class Student: Person{
	var major: String = "Unknown"

	required init(){
		self.major = "Unknown"
		super.init()
}

```
