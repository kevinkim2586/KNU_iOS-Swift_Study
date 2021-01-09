# Chapter10 프로퍼티와 메소

> 프로퍼티는 클래스,구조체 또는 열거형 등에 관련된 값을 뜻함<br>
> 매서드는 특정 타입에 관련된 함수를 뜻

## 10.1 프로퍼티

저장 프로퍼티
> 인스턴스의 변수/상수

연산 프로퍼티
> 특정 연산을 실행한 결과값

타입 프로퍼티
> 특정 타입에만 사용되는 프로퍼티

### 10.1.1 저장 프로퍼티

- 인스턴스의 변수/상수 값 저장

```swift
struct Coordinate{
	var x: Int // 저장 프로퍼티
	var y: Int // 저장 프로퍼티
}

//구조체에는 기본적으로 저장 프로퍼티를 매개변수로 갖는 이니설라이저가 있다.
let point: Coordinate = Coordinate(x: 10, y: 5)
```

### 10.1.2 지연 저장 프로퍼티

- 호출이 있어야지 값을 초기화하며, lazy키워드를 사용

```swift
struct Coordinate{
	//위치는 x,y값이 있어야 하므로 옵셔널이면 안됨
	var x: Int = 0
	var y: Int = 0
}

class Position{
	lazy var point: Coordinate = Coordinate()
	let name: String

	init(name: String){
		self.name = name
	}
}

let yagomPosition: Position = Position(name: "yagom")
// 이 코드를 통해 point 프로퍼티로 처음 접금할 때
// point 프로퍼티의 coordinatePoint가 생성됨
```

### 10.1.3 연산 프로퍼티

- 특정값을 저장하지 않고, 특정 상태에 따른 값을 연산하는 프로퍼티

```swift
struct Coordinate{
	var x: Int // 저장 프로퍼티
	var y: Int // 저장 프로퍼

	var oppositePoint: Coordinate{ // 연산 프로퍼티
		// 접근자
		get{
			return Coordinate(x: -x, y: -y)
		}
		// 설정자
		set(opposite){
			x = -opposite.x
			y = -opposite.y
		}

	}
}

var yagomPosition: Coordinate = Coordinate(x: 10, y:20)

print(yagomPosition) //10,20

print(yagomPosition.oppositePoint) //-10, -20
```

### 10.1.4 프로퍼티 감시자

- 프로퍼티의 값이 변경됨에 따라 적절한 작업을 취할수 있음


```swift
class Account{
	var credit: Int = 0{
		willSet{
			print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정입니다.")
		}
		didSet{
			print("잔액이 \(oldValue) 에서 \(credit)으로 변경되었습니다.")
		}
	}
}

let myAccount: Account = Account()
//잔액이 0원에서 1000원으로 변경될 예정

myAccount.credit = 1000
//잔액이 0원에서 1000원으로 변경되었음
```

### 10.1.5 전역변수와 지역변수

- 프로퍼티는 전역변수와 지역변수 둘다 사용 가능

### 10.1.6 타입 프로퍼티

- 각각의 인스턴스가 아닌 타입 자체에 속하는 프로퍼티를 타입 프로퍼티라고 한다.

```swift
class AClass{
    
    // 저장 타입 프로퍼티
    static var typeProperty: Int = 0
    
    // 저장 인스턴스 프로퍼티
    var instanceProperty: Int = 0{
        didSet{
            Self.typeProperty = instanceProperty+100
        }
    }
    // 연산 타입 프로퍼티
    static var typeComputedProperty: Int{
        get{
            return typeProperty
        }
        set{
            typeProperty = newValue
        }
    }
}

AClass.typeProperty = 123

let classInstance: AClass = AClass()
classInstance.typeProperty = 100  

print(AClass.typeProperty) //200
print(AClass.typeComputedProperty) //200
```

### 10.1.7 키 경로

- 프로퍼티의 위치만 참조할수 

```swift
class Person{
	var name: String
	
	init(name: String){
		self.name = name
	}
}

struct Stuff {
	var name: String
	var owner: Person
}

print(type(of: \Person.name)) //ReferenceWritableKeyPath<Person, String>
print(type(of:\Stuff.name)) //WritableKeyPath<Stuff, String>
```

## 10.2 메서드

- 특정 타입에 관련된 함수

### 10.2.1 인스턴스 메서드

- 특정 타입의 인스턴스에 속한 함수
- 인스턴스와 관련된 기능을 실행함

```swift
class LevelClass{
	// 현재 레벨을 저장하는 저장 프로퍼티
	var level: Int = 0 {

		didSet {
		print("Level\(level)")
		}
		// 프로퍼티 값이 변경되면 호출하는 프로퍼티 감시자
		func levelUp(){
		level += 1
		}
	.
	.
	.
	}
}

var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp() // Level Up!
.
.
.
```

\*self property\*

- 모든 인스턴스는 암시적으로 생성된 self 프로퍼티를 갖습니다. 자바의 this와 비슷하게 인스턴스 자기 자신을 가리키는 프로퍼티 입니다.

```swift
class LevelClass{
	var level: Int = 0
	
	func jumpLevel(to level: Int){
		print("Jump to \(level)")
		self.level = level
		// self를 안쓰면 매개변수로 넘어온 level을 참조할수도 있
	}
}
```

### 10.2.2 타입 메서드

- 매서드 앞에 static 키워드를 사용하여 타입 메서드 임을 나타냄
- statice 으로 정의하면 상속후에 재정의가 불가능, class 키워드를 쓰면 상속후 재정의가 가능하다.

```swift
class AClass{
	static func staticTypeMethod(){
		print("static type method")
	}
	class func classTypeMethod(){
		print("class type method")
	}
}

class BClass: AClass{
		/*
		//오류 방생!! 재정의 불가!
		override static func staticTypeMethod(){ 
		}
		*/
		override class func classTypeMethod(){
			print("Bclass class type method")
		}
}
```