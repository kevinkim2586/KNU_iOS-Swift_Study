# Chapter 10 :  프로퍼티와 메서드
→ **프로퍼티** : 클래스, 구조체, 열거형 등에 관련된 값을 뜻함

→ **메서드** : 특정 타입에 관련된 함수

### 프로퍼티 (Property)

1. 저장 프로퍼티 (Stored Properties)

    → 인스턴스의 변수/상수

2. 연산 프로퍼티 (Computed Properties)

    → 특정 연산을 실행한 결과값

3. 타입 프로퍼티 (Type Properties)

    → 특정 타입에만 사용되는 프로퍼티

### 저장 프로퍼티

구조체 ex)

```swift
struct Coordinate{
	var x: Int
	var y: Int
}
//x,y = 저장 프로퍼티

let point: Coordinate = Coordinate(x: 10, y: 10)
```

클래스 ex)

```swift
class Position{
	var point: Coordinate
	let name: String

	//프로퍼티 기본값을 지정해주지 않는다면 initializer 를 따로 정의해야 함
	init(name: String, currentPoint: Coordinate){
		self.name = name
		self.point = currentPoint
	}
}

let myPosition: Position = Position(name: "kevin", point: point)
```

** 인스턴스를 생성할 때 initializer 를 통해 초깃값을 보내야 하는 이유는 프로퍼티가 ***옵셔널이 아닌 값*** 으로 선언되어 있기 때문이다.

→ 그러므로 인스턴스는 생성할 때 프로퍼티에 값이 꼭 있는 상태여야 함

→ 물론 옵셔널도 설정되어 있으면 상관 X

### 지연 저장 프로퍼티 (Lazy Stored Properties)

→ 호출이 있어야 값을 초기화하며, 이때 lazy 키워드 사용

→ 상수 X, var 사용하여 변수로 정의 

```swift
struct Coordinate{
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

let myPosition: Position = Position(name: "yagom")
```

- When to use?

    → 복잡한 구조체나 클래스 구현할 때 많이 사용

    → 불필요한 성능 저하나 공간 낭비를 줄일 수 있음

### 연산 프로퍼티

- 실제 값 저장 X
- 특정 상태에 따른 값을 연산하는 프로퍼티
- getter/setter 역할도 가능

Why use?

→ 인스턴스 외부에서 메서드를 통해 인스턴스 내부 값에 접근하려면 (접근자, 설정자)를 구현해야 함. 이는 분산되어 있어 가독성이 떨어질 수 있음

 

```swift
struct Coordinate{
	var x: Int
	var y: Int

	//연산 프로퍼티
	var oppositePoint: Coordinate{
		
		get{
			return Coordinate(x: -x, y: -y)
		}
		
		set(opposite){
			x = -opposite.x
			y = -opposite.y
		}

		//아래도 사용 가능 (newValue는 관용적인 표현)
		set{
			x = -newValue.x
			y = -newValue.y
	}
	//즉, oppositePoint 라는 변수에 대한 get set 메소드를 설정해 주는 것
	//get set 중 하나만 구현해도 무방, 대신 안 쓰면 그건 못 씀 
}

var myPosition: Coordinate = Coordinate(x: 10, y:20)

print(myPosition)
//10,20

print(myPosition.oppositePoint)
//-10, -20

myPosition.oppositePoint = Coordinate(x: 15, y:10)
print(myPosition)
//15, 10
```

→ 이런 식으로 연산 프로퍼티를 사용하면 하나의 프로퍼티에 접근자와 설정자가 모두 모여 있고, 더 간결하고 명확하게 표현 가능

### 프로퍼티 감시자 (Property Observers)

- 프로퍼티의 값이 변경됨에 따라 적절한 작업을 취할 수 있음
- 프로퍼티의 값이 새로 할당될 때마다 호출됨
- 지연 저장 프로퍼티에 사용 X

- 프로퍼티 감시자

    → 프로퍼티 값 변경되기 직전에 호출되는 willSet 메서드 (매개변수: newValue) → 변경될 값

    → 값 변경된 직후에 호출하는 didSet 메서드 (매개변수: oldValue) → 변경되기 전의 값

```swift
class Account{
	var credit: Int = 0{
		willSet{
			print("잔액이 \(credit)원에서 \(newValue)원으로 변경될 예정")
		}
		didSet{
			print("잔액이 \(oldValue) 에서 \(credit)으로 변경되었음")
		}
	}
}
//newValue 하고 oldValue 는 따로 변수 선언을 안 해줘도 있는 값

let myAccount: Account = Account()

//잔액이 0원에서 1000원으로 변경될 예정
myAccount.credit = 1000
//잔액이 0원에서 1000원으로 변경되었음
```

### 저장변수의 감시자와 연산변수

- 연산 프로퍼티와 감시자는 전역변수와 지역변수 모두에 사용 가능

```swift
var wonInPocket: Int = 2000{
	willSet{
		print("주머니의 돈이 \(wonInPocket) 에서 \(newValue)로 변경 예정")
	}
	didSet{
		print("주머니의 돈이 \(oldValue) 에서 \(wonInPocket)으로 변경 완료")
	}
}
```

### 타입 프로퍼티 (Type Properties)

- 타입 자체에 속하는 프로퍼티를 타입 프로퍼티라 말한다.
- 그 타입의 모든 인스턴스가 공통으로 사용하는 값, 또는 모든 인스턴스에서 공용으로 접근하고 값을 변경할 수 있는 변수를 정의할 때 유용

```swift
class AClass{
    
    static var typeProperty: Int = 0
    
    var instanceProperty: Int = 0{
        didSet{
            Self.typeProperty = instanceProperty+100
        }
    }
    
    static var typeComputedProperty: Int{
        get{
            return typeProperty
        }
        set{
            typeProperty = newValue
        }
    }
}

let classInstance: AClass = AClass()

classInstance.typeProperty = 1000 //오류! 인스턴스로 접근 불가
AClass.typeProperty = 100 //이렇게 공통으로 접근해야 함 

classInstance.instanceProperty = 200
print(AClass.typeProperty) //400
```

### 키 경로 (Key Path)

- 값을 바로 **꺼내는게** 아니라 프로퍼티의 **위치만 참조**할 때 사용 가능
- 키 경로를 사용하여 간접적으로 특정 타입의 어떤 프로퍼티 값을 **가리켜야** 할지 미리 지정 후 사용 가능

\타입이름.경로.경로.경로

→ 경로 : 프로퍼티 이름

```swift
class Person{
	var name: String
	
	init(name: String){
		self.name = name
	}
}

print(type(of: \Person.name))
//ReferenceWritableKeyPath<Person, String>
```

ex)

```swift
struct Person {
    var firstname: String
    var secondname: String
    var age: Int
}
let dave = Person(firstname: "Dave", secondname: "Trencher" , age: 21)

let firstname: String = dave[keyPath: \Person.firstname]
print (firstname) 
// Dave
```

### 메서드 (Method)

- 특정 타입에 관련된 함수
- 다른 언어와는 달리 구조체와 열거형도 메서드를 가질 수 있음 (클래스는 물론)

### 인스턴스 메서드

- 특정 타입의 인스턴스에 속한 함수를 뜻함
- 인스턴스와 관련된 기능을 실행 (ex. Property 값 변경

```swift
class LevelClass{
	var level: Int = 0
	
	func levelUp(){
		level += 1
	}
}

var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp()
```

→ 위 instance method 는 level 인스턴스 프로퍼티 값을 수정함

****mutating 키워드**

→ 자신의 property 값을 수정할 때 instance of class 는 크게 신경 안 써도 되지만, struct 나 enum 은 "값 타입"이므로 메서드 앞에 mutating 키워드를 붙여서 해당 메서드가 인스턴스 내부의 값을 변경한다는 것을 명시해야 함.
→ 즉, 구조체 내부에서 구조체 자체의 프로퍼티 값을 수정할 때는, 그 행동을 수행하는 메서드 앞에 키워드 mutating 을 꼭 붙여줘야 한다는 얘기. 구조체 외부에서 수정할 때는 상관 없음.

```swift
struct LevelStruct{
	var level: Int = 0

	mutating func levelUp(){
		level += 1
	}
}
var levelClassInstance: LevelClass = LevelClass()
levelClassInstance.levelUp()
```

### Self Property

- 모든 instance 는 암시적으로 생성된 self property 를 갖는다. (Java 의 this와 비슷)
- 인스턴스 자기 자신을 가리키는 프로퍼티

When to use? → Instance 를 더 명확히 지칭하고 싶을 때 사용

```swift
class LevelClass{
	var level: Int = 0
	
	func jumpLevel(to level: Int){
		print("Jump to \(level)")
		self.level = level
		//매개변수로 들어온 level과 명확하게 구분을 짓기 위함
	}
}
```

→ 만약 위에서 self 를 붙여주지 않고 그냥 level = level 로 적어준다면 컴파일러는 좌측의 level을 함수의 매개변수로 넘어온 level 을 지칭하는 것으로 판단해버림.

- "값 타입" (구조체, 열거형) 인스턴스 자체의 값을 치환도 가능

```swift
struct LevelStruct{
	var level: Int = 0
	
	mutating func levelUp(){
		level+=1
	}
	mutating func reset(){
		self = LevelStruct()
	}
}

var levelStructInstance: LevelStruct = LevelStruct()
levelStructInstance.levelUp()  //1
levelStructInstance.reset())

print(levelStructInstance.level)  //0

enum OnOffSwitch{
	case on, off
	mutating func nextState(){
		self = self == .on? .off : .on
	}
}

var toggle: OnOffSwitch = OnOffSwitch.off //case 둘 중 하나를 할당
toggle.nextState()
print(toggle) //on
```

### 타입 메서드 (Type Method)

- Type Property 가 있듯이 메서드도 타입 메서드가 있음
- 메서드 앞에 static 키워드를 사용하여 타입 메서드임을 나타냄

→ static 으로 정의하면 상속 후 메서드 재정의가 불가능. 

→ 그러나 method 앞에 class 키워드를 쓰면 상속 후 재정의 가능

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

		override static func staticTypeMethod(){ }//오류! 재정의 불가!
		
		override class func classTypeMethod(){
			print("Bclass class type method")
		}
}
```
