# Chapter11 인스턴스 생성 및 소멸

> 초기화는 클래스나 구조체 또는 열거형의 인스턴스를 사용하기 위한 준비 과정<br>

## 11.1 인스턴스 생성

- \* 이니설라이즈\*를 정의하면 초기화 과정을 직접 구현할 수 있음

```swift
class SomeClass{
	init(){
		//초기화할 때 필요한 코드
	}
}

struct SomeStruct{
	init(){
		//초기화할 때 필요한 코드
	}
}

enum SomeEnum{
	case someCase
	
	init(){
		//열거형은 초기화 할때 반드시 case중 하나가 되어야 합니다.
		self = .selfCase
		//초기화 할 때 필요한 코드
	}
}
```

### 11.1.1 프로퍼티 기본값

- 구조체와 클래스의 인스턴스는 처음 생성할 때 옵셔널 저장 프로퍼티를 제외한 모든 저장 프로퍼티에 적절한 초깃값을 할당해야 합니다. 
- 프로퍼티를 정의할 때 프로퍼티 기본값을 할당하면 이니설라이저에서 따로 초깃값을 할당하지 않더라도 프로퍼티 기본값으로  저장 프로퍼티의 값이 초기화 

```swift
struct Area{
	var squareMeter: Double
	
	init(){
		squareMeter = 0.0  // 초기값 할
	}
}

let room: Area = Area()
print(room.squareMeter) //0.0
```

- 프로퍼티에 바로 기본값을 할당하는 경우 

```swift
struct Area{
	var squareMeter: Double = 0.0 // 프로퍼티 기본값 할당
}

let room: Area = Area()
print(room.squareMether) //0.0
```

### 11.1.2 이니셜라이저 매개변수

- 이니셜라이저도 매개변수를 가질수 있음

```swift
struct Area{
	var squareMeter: Double
	
	init(fromPy py: Double){ // 이니설라이저
		squareMeter = py * 3.3
	}
	
	init(fromSquareMeter squareMeter: Double){ //이니설라이저
		self.squareMeter = squareMeter
	}
	
	init(value: Double){ //이니설라이저
		squareMeter = value
	}
	
	init(_ value: Double){ // 이니설라이저
		squareMeter = value
	}
}

let roomOne: Area = Area(fromPy: 15.0)
print(roomOne.squareMeter) //49.587

let roomTwo: Area = Area(fromSquareMeter: 33.06)
print(roomTwo.squareMeter) // 33.06

Area() // 오류 발생
```
### 11.1.3 옵셔널 프로퍼티 타입

- 초기화 과정에서 값을 꼭 초기화 하지 않아도 되는, 즉 인스턴스가 사용되는 동안에 값을 꼭 갖지 않아도 되는 저장 프로퍼티가 있으면 해당 프로퍼티를 옵셔널로 선언해줄수 있음
- 초기화 과정에서 값을 지정해주기 어려운경우도 옵셔널로 선언해줄수 있음

```swift
class Person{
	var name: String
	var age: Int?
	
	init(name: String){
		self.name = name
	}
}

let yagom: Person = Person(name: "yagom")
print(yagom.name) // yagom
print(yagom.age)  // nil
```

### 11.1.4 상수 프로퍼티

- 선언한 프로퍼티를 바꾸지 않게 하기 위해서 상수로 선언해줌

```swift
class Person{
	let name: String
	var age: Int?
	
	init(name: String){
		self.name = name
	}
}

let yagom: Person = Person(name: "yagom")
yagom.name = "Eric"   //오류 발생!
```

### 11.1.5 기본 이니설라이저와 맴버와이즈 이니설라이저

- 기본 이니설라이즈
	- 프로퍼티 기본값으로 프로퍼티를 초기화 해서 인스턴스를 생성
	- 사용자 정의 이너설라이저가 정의되지 않은 상태에서 제공된다

- 맴버와이즈 이너설라이즈
	- 사용자 정의 이니설라이저를 구현하지 않으면 프로퍼티의 이름으로 매개변수를 갖는 이니설라이저인 맴버와이저를 기본으로 제공한다.
	- 클래스는 맴버와이저를 기본으로 제공하지 않는다.


```swift
struct Point{
	var x: Double = 0.0
	var y: Double = 0.0
}

struct Size{
	var width: Double = 0.0
	var height: Double = 0.0
}

let point: Point = (x: 0, y: 0)
let size: Size = Size(width: 50.0, height: 50.0)
// 구조체의 저장 프로퍼티에 기본값이 있는 경우
// 필요한 매개변수만 사용하여 초기화할 수도 있습니다.

let somePoint: Point = Point()
let someSize: Size = Size(width:50)
let anotherPoint: Point = Point(y:100)
```

### 11.1.6 초기화 위임

- 코드의 중복을 피하기 위하여 이니설라이저가 다른 이니설나이저에게 일부 초기화를 위힘하는 초기화 위임을 간단하게 구현할 수 있다.( 클래스는 초기화 위임을 할수 없다. )

```swift
enum Student{
	case elementary, middle, high
	case none
	init(){
		self=.none
	}


	init(koreanAge: Int){
		switch koreanAge{
			case 8...13:
				self = .elementary
			case 14...16
				self = .middle
			case 17..19
				self = .high
			default:
				self = .none
		}
	}
	init(bprnAt: Int,currentYear: Int){
		self.init(koreanAge: currentYear - bornAt + 1)
	}
}
var younger: Student = Student(koreanAge: 16)
print(younger) //middle

younger = Student(bornAt:1998, currentYear: 2016)
print(younger) // high
```

### 11.1.7 실패 가능한 이니셜라이저

- if 전달인자로 잘못된 값이나 적절치 못한값이 전달 되었을때, 초기화에 실패
- 이니설라이즈를 정의할 때 이런 실패 가능성을 염두에 두기도 하는데, 이렇게 실패 가능성을 내포한 이니설라이즈를 실패 가능한 이니설라이저라고 한다.

```swift
enum Student: String{
	case elementary = "초등학생", middle = "중학생", high = "고등학생"

	init?(koreanAge: Int){
		switch koreanAge{
		case 8...13:
			self = .elementary
		case 14...16:
			self = .middle
		case 17...19:
			self = .high
		default:
			return nil 
		}
	}
	
	init?(bornAt: Int, currentYear: Int){
		self.init(koreanAge: currentYear - bornAt + 1)
	}
}

var younger: Student? = Student(koreanAge: 28)
print(younger) //nil
.
.
.
younger = Student(rawValue: "고등학생")
print(younger) //high
```

### 11.1.8 함수를 사용한 프로퍼티 기본값 설

- 만약 사용자 정의 연산을 통해 저장 프로퍼티 기본값을 설정하고자 한다면 클로저나 함수를 사용하여 프로퍼티 기본값을 제공할수 있다.

```swift
class SomeClass{
	let someProperty: SomeType = {
		//새로운 인스턴스를 생성하고 사용자 정의 연산을 통한 후 반환해줍니다.
		// 반환되는 값의 타입은 SomeType과 같은 타입이어야 합니다.
		return result
	}()
}
```

## 11.2 인스턴스 소멸

- 클래스의 인스턴스는 디이니설라이즈를 구현할 수 있음
- 디이니설라이즌느 이니설라이저와 반대 역할을 한다. 메모리에서 해제되기 직전 클래스 인스턴스와 관련하어 원하는 정리 작업을 구현할 수 있다.
-디이넛라이저는 클래스의 인스턴스에만 구현할수 있다.

```swift
class SomeClass{
	deinit{
		print("Instance will be deallocated")
	}
}

var instance: SomeClass? = SomeClass()
instance = nil // Instance will be deallocated immediately
```
