# Chapter 11 : 인스턴스 생성과 소멸
### 인스턴스 생성

- Initializer 를 정의하면 초기화 과정을 직접 구현 가능
- Initializer → 새로운 인스턴스를 생성할 수 있는 특별한 Method
- Swift 에서 Initializer 는 반환값 X → 이니셜라이저의 역할은 그저 인스턴스의 첫 사용을 위해 초기화하는 것
- func 키워드가 없는 메서드

```swift
class SomeClass{
	init(){
	}
}

struct SomeStruct{
	init(){
	}
}

enum SomeEnum{
	case someCase
	
	init(){
		self = .selfCase
	}
}
//클래스나 구조체는 init 이 비워있어도 되지만, 열거형은 초기화할 때 반드시 case 중 하나가 되어야 함
```

### 프로퍼티 기본 값 (Default Value of Properties)

- 구조체와 클래스의 인스턴스는 처음 생성할 때 옵셔널 변수 빼고는 모든 저장 프로퍼티에 적절한 초기값 (Initial Value)를 할당해야 한다.
- 다만 프로퍼티를 처음에 정의를 할 때 먼저 프로퍼티 기본값을 할당하면 후에 이니셜라이저에서 따로 초기값을 할당 안 해도 됨

```swift
struct Area{
	var squareMeter: Double
	
	init(){
		squareMeter = 0.0  //Default Value 를 미리 작성하는 것
	}
}

let room: Area = Area()
print(room.squareMeter) //0.0
```

**초기화 후에 값이 확정되지 않은 저장 프로퍼티는 존재할 수 없음

→ 위와 같이 init( ) 함수 안에서 기본값을 할당할 수 있긴 하지만, 프로퍼티에 바로 기본값을 할당하는 법도 있음

```swift
struct Area{
	var squareMeter: Double = 0.0
}

let room: Area = Area()
print(room.squareMether) //0.0
```

### 이니셜라이저 매개변수 (Parameters for Initializer)

- init ( ) 호출 시 안에 매개변수를 넣을 수도 있음

```swift
struct Area{
	var squareMeter: Double
	
	init(fromPy py: Double){
		squareMeter = py * 3.3
	}
	
	init(fromSquareMeter squareMeter: Double){
		self.squareMeter = squareMeter
	}
	
	init(value: Double){
		squareMeter = value
	}
	
	init(_ value: Double){
		squareMeter = value
	}
}

let roomOne: Area = Area(fromPy: 15.0)

let roomTwo: Area = Area(fromSquareMeter: 33.06)
```

→ 위와 같이 여러 개의 initializer 를 사용할 수 있음.

### 옵셔널 프로퍼티 타입 (Optional Property Type)

- 초기화 과정에서 꼭 값을 갖지 않아도 되는 프로퍼티가 있다면 옵셔널로 선언 가능
- 자동으로 nil 할당

```swift
class Person{
	var name: String
	var age: Int?
	
	init(name: String){
		self.name = name
	}
}
```

### 상수 프로퍼티 (Constant Property)

- 상수로 선언된 프로퍼티는 인스턴스를 초기화하는 과정에서만 값을 할당 가능 → 이후 값 변경 불가

**상수 프로퍼티는 본인 클래스에서만 초기화 가능 → 상속 받은 자식 클래스는 상수 프로퍼티 초기화 불가

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

### 기본 이니셜라이저와 멤버와이즈 이니셜라이저 (Default Initializer & Memberwise Initializer)

**Default Initializer**:

- 옵셔널 타입이 아닌 모든 저장 프로퍼티에 초기값이 설정되어 있다면 기본 이니셜라이저 사용 가능
- 사용자 정의 이니셜라이저가 정의되어 있지 않은 상태에서 제공된다.

**Memberwise Initializer**:

- 모든 저장 프로퍼티의 이름과 할당할 값을 매개변수로 넣으면서 초기화하는 것
- 클래스는 Memberwise Initializer 제공 X → 구조체는 제공
- The Size structure automatically receives an init(width:height:) memberwise initializer, which you can use to initialize a new Size instance:

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
//위와 같이 매개변수로 프로퍼티의 이름과 할당할 값을 모두 포함하여 전달함.

let somePoint: Point = Point()
let someSize: Size = Size(width:50)
//width 에만 값을 할당하고 싶으면 그렇게 해도 됨
//그럼 자동으로 height 는 0.0 으로 초기화됨
let anotherPoint: Point = Point(y:100)
//위도 마찬가지
```

**Memberwise Initializer 는 구조체에서만 가능! 클래스는 X

### 초기화/이니셜라이저 위임 (Initializer Delegation)

- Initializer 는 인스턴스 초기화의 일부를 수행하기 위해 다른 이니셜라이저를 호출할 수 있다

    → 그러려면 2개의 사용자 정의 init ( ) 함수가 있어야 함

- 값 유형 (구조체, 열거형)에서만 사용 가능
- 클래스의 초기화 위임은 상속임

Why use? → 코드의 중복 예방

```swift
struct Frame {
    var x: Int
    var y: Int
    
    init(x: Int, y: Int) {
        self.x = x
        self.y = y
    }
    
    init(y: Int) {
        self.init(x: 0, y: y) // 다른 이니셜라이저를 호출
    }
}

let rect = Frame(x: 10, y: 10)
let originRect = Frame(y: 5)
```

### 실패 가능한 이니셜라이저 (Failable Initializer)

- 초기화 과정 중에서 실패할 가능성이 있는 이니셜라이저 (init ( ) ) → 예외 상황
- 반환 값으로 옵셔널을 생성
- 초기화 실패 시 nil 을 반환
- init?( )  사용
- 구조체, 열거형, 클래스 모두 정의 가능
- 잘못된 전달인자를 전달받았을 때 초기화하지 않을 수 있음

```swift
struct Animal {

    let species: String

    init?(species: String) {
        if species.isEmpty { return nil } // nil 반환 시 return 사용
		
        self.species = species
    }

}
//init?을 옵셔널로 설정하였기에 nil 반환이 가능₩
//Animal? 도 옵셔널로 정의
if let animal: Animal? = Animal(species: "Dog"){
	print(animal.species)
}
else{
	print("error initializing")
}
//Dog

if let animal: Animal? = Animal(species: 2){
	print(animal.species)
}
else{
	print("error initializing")
}
//error initializing
```

- 열거형에서 유용하게 쓰임 → 잘못된 case 가 들어왔을 시 사용

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
			return nil   //이것도 init? () 으로 선언해서 가능한 것
		}
	}
	
	init?(bornAt: Int, currentYear: Int){
		self.init(koreanAge: currentYear - bornAt + 1)  //초기화 위임!
	}
}

var younger: Student? = Student(koreanAge: 25)
print(younger)
//nil

younger = Student(rawValue: "고등학생")
print(younger)
//high
```

### 클로저나 함수를 이용해 기본 프로퍼티 값을 설정하기 (Setting a Default Property Value with a Closure or Function)

- 기본 값 설정이 단순히 값을 할당하는 것이 아니라 다소 복잡한 계산을 필요하다면 클로저나 함수를 이용해 값을 초기화 하는데 이용할 수 있음

```swift
class SomeClass{
	let someProperty: Int = {
		
		return result
	}()
}

//result 는 Int 와 반드시 같은 타입이어야 함
//즉, 반환 값의 타입은 동일
```

→ 클로저 뒤에 소괄호 ( ) 가 붙는 이유는 클로저를 실행하기 위함. 소괄호를 붙여주지 않으면 프로퍼티의 기본값은 클로저 그 자체가 됨. 

```swift
struct Student{
	var name: String?
	var number: Int?
}

class SchoolClass{
	var students: [Student] = {
		//여기서 반환값은 반드시 [Student]이어야 한다
		var arr: [Student] = [Student]()
	
		for num in 1...15{
			var student: Student = Student(name: nil, number: num)  //하나씩 증가
			arr.append(student)
		}
	}()
}

let myClass: SchoolClass = SchoolClass()
print(myClass.students.count)
//15

```

### 인스턴스 소멸

- Deinitializer 구현 가능
- 메모리에서 해제되기 직전 클래스 인스턴스와 관련하여 원하는 정리 작업 구현 가능
- deinit 키워드 사용
- **Deinitializer 는 클래스의 인스턴스에만 구현 가능!

→ Deinitializer 는 단 하나만 구현 가능

→ 매개변수가 없고, 소괄호를 적어주지도 않음

→ 자동 호출이기 때문에 별도 코드로 호출 불가능

```swift
class SomeClass{
	deinit{
		print("Instance will be deallocated")
	}
}

var instance: SomeClass? = SomeClass()
instance = nil
```

→ 즉, 따로 deinit 코드를 우리가 실행해 주는 것이 아니라, instance = nil 이 실행되면 인스턴스가 소멸되는데, 소멸 직전에 deinit { } 이 실행되는 것. 여기서 파일을 저장하거나, 제대로 닫아주는 등의 추가 작업 가능
