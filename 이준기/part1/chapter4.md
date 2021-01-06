# Chapter4 데이터 타입 고급

>\*안정성\*이 가장 뛰어나야함<br>

## 4.1 데이터 타입 안심

- 안정성이 가장 중요함

### 4.1.1 데이터 타입 안심이란

- 컴파일시 타입을 확인해줌 이를 '타입 확인'이라고함

### 4.1.2  타입추론

- 타입을 지정을 안해줘도 컴파일러가 컴파일하면서 알아서 적당한 타입으로 지정해준다

## 4.2 타입 별칭

- 이미 존재하는 데이터 타입에 임의로 다른 이름(별칭) 부여 할수 있음

```swift
typealias MyInt = Int
let age : MyInt = 100
```

## 4.3 튜플

- \*지정된 데이터의 묶음\*

```swift
// String, Int, Double 타입을 갖는 튜플
var person: (String, Int, Double) = ("yagom", 100, 182.5)

// 인덱스를 통해 값을 빼 올 수 있습니다.
print("이름: \(person.0),나이 : \(person.1),신장 : \(person.2)")

person.1 = 99
person.2 = 178.5
```

- 튜플 요소 마다 이름을 붙여줄 수 있음

```swift
// String, Int, Double 타입을 갖는 튜플
var person: (name: String, age: Int, height: Double) = ("yagom", 100, 182.5)

// 인덱스를 통해 값을 빼 올 수 있습니다.
print("이름: \(person.name),나이 : \(person.age),신장 : \(person.height)")

person.age = 99 // 요소 이름을 통해 값을 할당할 수 있습니다.
person.2 = 178.5 // 인덱스를 통해서도 값을 할당할 수 있습니다.
```

- 튜플도 별칭이 지정 가능하다.

## 4.4 컬렉션형

- 수많은 데이터를 묶어서 저장하고 관리할수 있는 컬렉션 타입 제공
- 배열, 딕셔너리, 세트

### 4.4.1 배열

- 데이터를 일렬로 나열한 후  순서대로 저장하는 형태의 컬렉션 타입
- 순서가 존재

```swift
// 대괄호를 사용하여 배열임을 표현합니다.
var names: Array<String> = ["yagom", "chulsoo", "younghee", "yagom"]

// 위 선언과 정확히 동일한 표현. [String]은 Array<String>의 축약 표현
var names: [String] = ["yagom", "chulsoo", "younghee", "yagom"]

// 배열의 타입을 정확히 명시해줬다면 []만으로도 빈 배열 생성 할수 있음

var emptyArray: [Any] = []
print(emptyArray.isEmpty) //true
print(names.count) // 4
```

- 배열의 사용

```swift
print(names[2]) //younghee
name[2] = "jenny" 
print(names[2]) // jenny
print(names[4]) // 인덱스 범위를 벗어났기 때문에 오류가 발생

names[4] = "elsa" //인덱스 범위를 벗어났기 때문에 오류가 발생
names.append("elsa") // 마지막에 elsa 추가
names.append(contentsOf: ["john", "max"]) //맨 마지막에 john과 max가 추가됌
names.insert("happy",at:2) // 인덱스 2에 삽입
```

### 4.4.2 딕셔너리

- 요소들이 순서 없이 키와 값의 쌍으로 구성 되는 컬렉션 타입
- 키는 중복될수 없다.

```swift
// 키는 String, 값은 Int 타입인 빈딕셔너리를 생성합니다.
var numberForName: Dictionary<String, Int> = Dictionary<String, Int>()

//위 표현과 정확히 같은 표현입니다.
var numberForName: [String: Int] =  [String: Int]()

//위 표현과 정확히 같은 표현
var numberForName: StringIntDictionary = StringIntDictionary()

// 딕셔너의 키와 값 타입을 정확히 명시해줬다면 [:}만으로도 빈 딕셔너리를 생성할 수 있습니다.
var numberForName: [String: Int] = [:]
```

### 4.4.3 세트

- 데이터를 순서 없이 하나의 묶음으로 지정하는 형태의 컬렉션 타입
- \*순서가 중요하지 않거나 각 요소가 유일한 값이어야 하는 경우\*에 사용
- 해시가능한 값이 들어가야

```swift
var names: Set<String> = Set<String>() // 빈 세트 생성
var names: Set<String> = [] // 빈 세트 생성

// Array와 마찬가지로 대괄호 사용
var names: Set<String> = ["yagom", "chulsoo", "younghee", "yagom"]
```

## 4.5 열거형

- 연관된 항목들을 묶어서 표현할 수 있는 타입

>제한된 선택지를 주고 싶을때
>정해진 값 외에는 입력받고 싶지 않을때
>예상된 입력 값이 한정되어 있을때

### 4.5.1 기본 열거형

- enum을 통해서 선언 가능

```swift
enum School {
    case primary // 유치원
    case elementary // 초등
    case middle // 중등
    case high // 고등
    case college // 대학
    case university // 대학교
    case graduate // 대학원
    //모두 한줄로 나열가능
}

//변수 생성
var highestEducationLevel: School = School.university

//위 표현과 정확히 같은 표현
var highestEducationLevel: School = .university
```

### 4.5.2 원시 값

- 열거형의 각 항목은 자체로도 하나의 값이지만 항목의 원시 값도 가질수 있음
- 즉, 특정 타입으로 지정된 값을 가질수 있음

```swift
enum School : String{
	case primary = "유치원"
	case elementary = "초등학교"
	case middle = "중학교"
	case high = "고등학교"
	case college = "대학"
	case university = "대학교"
	case graduate = "대학원"
}
```

### 4.5.3 연관 값

- 열거형 내의 항목(case)이 자신과 연관된 값을 가질 수 있음
- 연관값은 각 항목 옆에 소괄호로 묶어 표현 할 수 있음

```swift
enum MainDish{
	case pasta(taste: String)
	case pizza(dough: String, topping: String)
	case chicken(withSauce: Bool)
	case rice
}

var dinner : Maindish = Maindish.pasta(taste: "크림") // 크림 파스타
dinner = .pizza(dough: "치즈크르스트", topping: "불고기") // 불고기 치즈크러스트 피자
dinner = .chicken(withSauce: true) // 양념 통닭
dinner = .rice //밥
```

### 4.5.4. 항목 순회

- 열거형에 포함된 모든 케이스를 알아야할 때 사용
- 열거형의 이름 뒤에 콜론(:)을 작성하고 한 칸 띄운 뒤 CaseIteravle 프로토콜을 채택함 

### 4.5.5 순환 열거형

- 열거형 항목의 연관 값이 열거형 자신의 값이고자 할 때 사용함
- 순환 열거형을 명시할 땐 indirect 키워드를 사
