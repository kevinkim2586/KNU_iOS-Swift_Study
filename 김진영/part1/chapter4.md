# Chapter4 데이터 타입 고급

### 타입 추론

~~~ swift
// 타입 지정하지 않았으나 타입 추론을 통해서 name은 String 타입으로 선언된다
var name = "kwanhee"

// 오류 발생
name = 100
~~~

## 4.2 타입 별칭

~~~ swift
typealias MyInt = Int
typealias YourInt = Int

let age: MyInt = 100
var year: YourInt = 2090

// MyInt도, YourInt도 Int이기 때문에 같은 타입으로 취급한다
year = age
~~~

## 4.3 튜플

> **지정된 데이터의 묶음**

~~~ swift
// String, Int, Double 값을 갖는 튜플
var person: (String, Int, Double) = ("yagom", 100, 182.5)

// 인덱스를 통해 값에 접근 가능
print("이름: \(person.0), 나이: \(person.1), 신장: \(person.2)")

person.1 = 99
person.2 = 178.5
~~~

~~~ swift
// String, Int, Double 값을 갖고 각각 이름을 갖는 튜플
var person: (name: String, age: Int, height: Double) = ("yagom", 100, 182.5)

// 요소 이름을 통해서 값에 접근 가능
print("이름: \(person.name), 나이: \(person.age), 신장: \(person.height)")

person.age = 99
person.2 = 178.5
~~~

튜플 별칭 지정

~~~ swift
typealias PersonTuple = (name: String, age: Int, height: Double)

let yagom: PersonTuple = ("yagom", 100, 178.5)

print("이름: \(yagom.name), 나이: \(yagom.age), 신장: \(yagom.height)")
~~~

## 4.4 컬렉션형

### 4.4.1 배열

> 같은 타입의 데이터를 일렬로 나열한 후 순서대로 저장하는 형태의 컬렉션 타입

~~~ swift
// 표현 1
var names: Array<String> = ["yagom", "chulsoo", "younghee", "yagom"]

// 표현 2
var names2: [String] = ["yagom", "chulsoo", "younghee", "yagom"]

var emptyArray: [Any] = [Any]()         // Any 데이터를 요소로 갖는 빈 배열을 생성
var emptyArray2: [Any] = Array<Any>()   // 위 선언과 정확히 같은 동작을 하는 코드

// 배열의 타입을 정확히 명시해줬다면 []만으로 빈 배열을 생성할 수 있다
var emptyArray3: [Any] = []
~~~

- 배열은 각 요소에 인덱스를 통해 접근 가능
- 인덱스는 0부터 시작, 잘못된 인덱스로 접근하려고 하면 Exception Error 발생
- first, last 프로퍼티를 통해 맨 처음, 맨 마지막 요소를 가져올 수 있음
- isEmpty 프로퍼티를 통해 빈 배열인지 아닌지 확인 가능
- count 프로퍼티를 통해 배열에 든 요소의 개수를 알 수 있음
- index(of:) 메서드를 사용하면 해당 요소의 인덱스를 알아낼 수 있음
    - 만약 중복된 요소가 있다면 제일 먼저 발견된 요소의 인덱스를 반환
- append(_:) 메서드를 사용하면 맨 뒤에 요소를 추가할 수 있음
- insert(_:at:) 메서드를 사용하면 중간에 요소를 삽입할 수 있음
- remove(_:) 메서드를 사용하면 요소를 삭제하고 해당 요소를 반환값으로 얻을 수 있음

### 4.4.2 딕셔너리

> 요소들이 순서 없이 key와 value의 쌍으로 구성되는 컬렉션 타입  
> key는 유일하며, value는 유일하지 않음  
> 딕셔너리 내부에 없는 키로 접근해도 오류나지 않고 nil을 반환함

~~~ swift
// 키는 String, 값은 Int 타입인 빈 딕셔너리를 생성
var numberForName: Dictionary<String, Int> = Dictionary<String, Int>()
// 위 선언과 정확히 같은 표현
var numberForName2: [String:Int] = [String:Int]()
// 딕셔너리의 키와 값 타입을 정확히 명시해줬다면 [:]만으로도 빈 딕셔너리를 생성할 수 있다
var numberForName3: [String:Int] = [:]
// 초기값을 주어 생성할 수도 있음
var numberForName4: [String:Int] = ["yagom":100, "chulsoo":200, "jerry":300]

print(numberForName4["yagom"])              // 100을 출력함(but Optional임). key로 value접근 가능
print(numberForName4["yagom", default: 0])  // 100을 출력함(Optional 아님)
print(numberForName4["z3ro", default: 0])   // 0을 출력함(Optional 아님)
~~~

### 4.4.3 세트

> 같은 타입의 데이터를 순서 없이 하나의 묶음으로 저장하는 형태의 컬렉션 타입  
> 세트 내의 값은 모두 유일한 값, 즉 중복된 값이 존재하지 않음  
> -> **순서가 중요하지 않거나 각 요소가 유일한 값이어야 하는 경우에 사용**  
> 해시 가능한 값(Hashable)이 들어와야 함

~~~ swift
var names: Set<String> = Set<String>()  // 빈 세트 생성
var names2: Set<String> = []            // 빈 세트 생성

var names3: Set<String> = ["yagom", "chulsoo", "younghee", "yagom"]

print(names.count)  // 3 - 중복된 값은 허용되지 않아 yagom은 1개만 남음
~~~

- insert(_:) 메서드를 사용해서 요소를 추가할 수있음
- remove(_:) 메서드를 사용하면 요소가 삭제된 후 반환됨

## 4.5 열거형
사용
- 제한된 선택지를 주고 싶을 때
- 정해진 값 외에는 입력받고 싶지 않을 때
- 예상된 입력 값이 한정되어 있을 때

### 4.5.1 기본 열거형

~~~ swift
enum School {
    case primary
    case elementary
    case middle
    case high
    case college
    case university
    case graduate
}
// or
enum School {
    case primary, elementary, middle, high, college, university, graduate
}
~~~

### 4.5.2 원시값

~~~ swift
enum School: String {
    case primary = "유치원"
    case elementary = "초등학교"
    case middle = "중학교"
    case high = "고등학교"
    case college = "대학"
    case university = "대학교"
    case graduate = "대학원"
}

let highestEducationLevel: School = School.university
print("저의 최종학력은 \(highestEducationLevel.rawValue) 졸업입니다.")
~~~

### 4.5.3 연관 값

~~~ swift
enum MainDish {
    case pasta(taste: String)
    case pizza(dough: String, topping: String)
    case chicken(withSauce: Bool)
    case rice
}

var dinner: MainDish = MainDish.pasta(taste: "크림")
dinner = .pizza(dough: "치즈크러스트", topping: "불고기")
dinner = .chicken(withSauce: true)
dinner = .rice
~~~
