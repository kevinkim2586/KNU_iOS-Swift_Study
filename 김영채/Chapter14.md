# Chapter 14 : 옵셔널 체이닝과 빠른 종료
- Optional Chaining and Early Exit

### 옵셔널 체이닝

→ 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드 등을 가져오거나 호출할 때 상요할 수 있는 과정

- nil이 반환될 가능성이 있으므로 옵셔널 체이닝의 반환된 값은 항상 옵셔널

```swift
let roomNum: Int? = yagom.address?.building?.room?.number
//nil
//number까지 가보고 값이 있으면 return 값, 아니면 return nil

let roomNum1: Int = yagom.address!.building!.room!.number
//런타임 오류
//만약 address 에 값이 할당되어 있지 않으면 거기서 이미 오류 발생
```

→ 꼬리에 꼬리를 무는 형식

```swift
let yagom: Person = Person(name: "yagom")

if let roomNumber: Int = yagom.address?.building?.room?.number{
	print(roomNumber)
}
else{
	print("no room num")
}
```

→ 옵셔널 체이닝을 통해 한 단계뿐만 아니라 여러 단계로 복잡하게 중첩된 옵셔널 프로퍼티나 메서드 등에 매번 nil 체크를 하지 않아도 손쉽게 접근 가능

옵셔널 체이닝을 통한 값 할당 시도

```swift

yagom.address?.building?.room?.number = 505
print(yagom.address?.building?.room?.number)
//nil
```

→ 애초에 address 프로퍼티가 nil 이면 맨 끝의 number 에는 당연히 할당이 안 됨. nil.number 란 존재 X

옵셔널 체이닝을 통한 값 할당

```swift
yagom.address = Address(province: "서울",city: "잠실", street: "잠실대로", building: nil
, detailAddress: nil)

yagom.address?.building = Building(name: "곰굴")
yagom.address?.building?.room = Room(number: 0)
yagom.address?.building?.room?.number = 505
//야곰아 주소 입력했어? 빌딩은? 방은? 다 있어? 그럼 number 에 505 할당할게~ 대강 이런 식?
```

→ 이런 경우 값 할당 가능

### 옵셔널 체이닝을 통한 메서드 호출

- 호출 방법은 프로퍼티 호출과 동일
- 메서드의 반환 타입이 옵셔널이라면 이 또한 옵셔널 체인에서 사용 가능

```swift
yagom.address?.fullAddress()?isEmpty

...
func fullAddress()->String?{
	..
	if...
	if...
	else{
		return nil
	}
}
```

### 옵셔널 체이닝을 통한 서브스크립트 호출

- Subscript 는 딕셔너리나 배열에서 많이 사용
- 옵셔널의 서브스크립트를 사용하고자 할 때 대괄호 앞에 ? 를 표시

```swift
let array: [Int]?: [1,2,3]
array?[1]  // 2

var dictionary: [String: [Int]]? = [String: [Int]]()
dictionary?["numberArray"] = array
dictionary?["numberArray"]?[2]  // 3
```

### 빠른 종료 (Early Exit)

- guard 키워드
- bool 타입의 값으로 동작하는 기능 (if와 유사)
- guard 뒤에 따라붙는 코드의 실행결과가 true 일 때 코드가 계속 실행
- if문과 다르게 항상 else 구문이 뒤에 따라와야 함
- else구문이 실행되면 else 구문의 블록 내부에는 꼭 자신보다 상위의 코드 블록을 종료하는 코드가 들어가게 된다

Why use? → if코드보다 훨씬 간결하고 읽기 좋게 구성 가능

```swift
//if구문
for i in 0...3{
	if i==2{
		print(i)
	}
	else{
		continue
	}
}

//guard 구문
for i in 0...3{
	guard i==2 else{
		continue      //이 블록 내부가 else문이라 보면 됨
	}  
	print(i)
}
```

guard 구문이 사용될 수 없는 경우

→ 자신을 감싸는 코드 블록, 즉 return, break, continue, throw 등의 제어문 전환 명령어를 쓸 수 없는 상황이라면 사용이 불가능

```swift
let first = 3
let second = 5

guard first >  second else{
	//여기에 들어올 제어문 전환 명령이 없으니 오류!
}

```
