# Chapter 17 : 서브스크립트
- Subscript
- 클래스, 구조체, 열거형에는 컬렉션, 리스트, 시퀀스 등 타입의 요소에 접근하는 단축 문법인 서브스크립트 정의 가능
- 즉 이런 개별 요소에 접근할 수 있는 지름길을 제공하는 것
- 인덱스를 통해 값 설정 및 가져오기 가능
- someArray[index], someDictionary[key] → 이렇게 표현하는 것이  Subscript
- subscript 키워드 사용 필요

```swift
subscript(index: Int) -> Int{
	get{
		//적절한 서브스크립트 결과값 반환	
	}
	set(newValue){
		//적절한 설정자 역할 수행
		//newValue는 subscript 의 반환 타입과 통일해야 함 
	}
}
```

- 읽기 전용 서브스크립트는 get 메서드만 있음

When to use? → 자신이 가지는 시퀀스나 컬렉션, 리스트 등의 요소를 반환하고 설정할 때 주로 사용

```swift
struct TimesTable { 
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index 
        // 서브스크립트에 입력하는 정수만큼 곱하는 것.
        // set 없이 읽기 전용
    }
}
let threeTimesTable = TimesTable(multiplier: 3) // 구구단 3단
print("six times three is \(threeTimesTable[6])") // 서브스크립트에 [6] 입력됬으니 3*6 해서 18출력

```

### 복수 서브스크립트

- 하나의 타입이 여러 개의 서브스크립트 가질 수 있음

```swift
subscript(index: Int)->Student?{
	get{
	}
	set{
	}
}

//읽기전용
subscript(index: String)->Int?{
	get{
	}
}

```

→ [ ] 안에 Int 값을 주냐 String 값을 주냐에 따라 다른 subscript 가 실행됨
