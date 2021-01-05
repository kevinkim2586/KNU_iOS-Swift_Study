# Chapter 5 : 연산자
### 삼항 조건 연산자

```swift
var valueA: Int = 3
var valueB: Int = 5
var biggerValue: Int = valueA > valueB ? valueA : valueB
```

### 범위 연산자

- A...B → A와 B 포함
- A..<B → A부터 B미만까지
- A... → A 이상의 수
- ...A → A 이하의 수
- ..< A → A 미만의 수

# Chapter 6 : 흐름 제어
if 구문 관련 : 

** 스위프트에서는 if 구문은 조건의 값이 꼭 **Bool 타입**이어야 함

→ C언어처럼 0, 1 안 되나?

- if 키워드 뒤에 따라오는 조건수식의 소괄호는 선택 사항임

```swift
let first : Int = 5
let second : Int = 10

if first < second {
	...
} 
else if (first > second){
	...
}
```

### Switch 구문:

- C언어와는 다르게 break 키워드 사용은 선택 사항. 즉, case 내부의 코드를 모두 실행하면 break 없이도 switch 구문 종료
- 스위프트에서 switch 구문 조건에는 다양한 값이 들어갈 수 있음

    → 다만 비교 값은 입력 값과 타입이 같아야 함

```swift
let integerValue : Int = 5

switch intergerValue{
	case 0:
		print("0")
	case 1...10:
		print("1~10")
		fallthrough //case를 연속 실행할 때 사용
	default:
		print("---")
}

```

- case "Jenny", "Joker", "Nova": 처럼 여러 개의 항목을 한 번에 case로 지정은 가능. 그렇지만 case 를 연달아 쓰는 것은 불가능

ex.

```swift
let stringValue : String = "Liam Neeson"

switch stringValue{
	case "yagom":
		print("yagom")
	case "Jenny":       //비어 있으니 오류!
	case "Joker":       //비어 있으니 오류!
}
//C언어처럼 구현하려면 fallthrough 키워드 사용

```

- 튜플도 사용 가능!

```swift
typealias NameAge = (name: String, age: Int)

let tupleValue: NameAge("yagom", 99)

switch tupleValue{
	case ("yagom", 99):
		print("correct")
	default:
		print("wrong")
}
```

- Where 키워드 사용

```swift
let 직급: String = "사원"
let 연차: Int = 1
let 인턴인가: Bool = false

switch 직급{
	case "시원" where 연차 < 2 && 인턴 == false:
		print("True")
}
```

### 반복문

- for-in 구문

```swift
for i in 0...5{

	if i.isMultiple(of: 2){
		print(i)
		continue
	}
}
```

 

- i 와 같은 시퀀스에 해당하는 값이 필요 없다면? 와일드 카드 식별자 (_) 사용

```swift
for _ in 1..3{
	result += 10
}
```

- 기본 컬렉션 타입도 사용 가능

```swift
//Dictionary
let friends : [String : Int] = ["Jay", 24, "Joe", 23]

for tuple in friends{
	print(tuple)
}

// ("Joe", 23)
// ("Jay", 24)

for (key,value) in friends {
	print("\(key), \(value)")
}
```

### While 구문

```swift
var names : String = ["Joker", "Jenny", "Nova"]

while names.isEmpty == false{
	print("Good bye \(names.removeFirst())")
}
```

### repeat-while 구문

- do-while 구문과 크게 다르지 않음
- repeat 블록의 코드를 최초 1회 실행을 무조건 함

```swift
var names : String = ["Joker", "Jenny", "Nova"]

repeat{
	print("Good bye \(names.removeFirst())")
}while names.isEmpty == false
```
