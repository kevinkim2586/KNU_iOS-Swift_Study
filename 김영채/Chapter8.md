# Chapter 8 : 옵셔널
- 안전성을 문법으로 담보하는 기능
- 값이 있을 수도, 없을 수도 있음을 나타내는 표현

→ 변수/상수 등에 꼭 값이 있다는 것을 보장할 수 없다. 값이 nil일 수도 있다.

- 옵셔널과 옵셔널이 아닌 값은 철저히 다른 타입으로 인식하기 때문에 컴파일 오류 방지 가능

** 옵셔널 변수/상수가 아니면 nil 값 할당 불가능. 즉 옵셔널 (? 추가) 이어야지만 nil 값 할당 가능

- C언어와는 다르게 0을 null이라 생각하지 않음. 0도 엄연한 값임
- nil은 옵셔널로 선언된 곳에서만 사용 가능

```swift
var myName : String? = "kevin"

myName = nil

print(myName) 
//nil
```

- 함수 매개변수가 옵셔널일 때는 "아, 이 매개변수에는 값이 없어도 되는구나"라는 것을 바로 알아야 함

- 옵셔널은 값을 갖는 케이스와 그렇지 못한 케이스 두 가지로 정의되어 있음 (enum)

    → nil 일때는 none case, 값이 있으면 some case.

    → 옵셔널에 값이 있으면 some 과 연관된 값인 Wrapped 에 값이 할당된다

### 옵셔널 추출

**강제 추출**:

- 옵셔널 강제 추출(Forced Unwrapping)
- 가장 위험하지만 가장 간단한 방법 (런타임 오류 발생 가능성)
- ! 를 붙여주면 값 강제 추출 후 반환 → 강제 추출 시 값이 없다면 런타임 오류

```swift
var myName: String? = "kevin"

//옵셔널이 아닌 변수에는 옵셔널 값이 들어갈 수 없음. 추출해서 할당해야 함
var name: String = myName!

myName = nil
name = myName!
//오류

if myName!=nil{
	print("name is \(myName!)")
}else{
	print("name == nil")
}
```

**옵셔널 바인딩:**

- Optional Binding
- 옵셔널에 값이 있는지 확인할 때 사용
- if, while 문과 결합해서 사용

```swift
var myName: String? = "yagom"

// 옵셔널 바인딩을 통한 임시 상수 할당
if let name = myName{
	print("name is \(name)")
}
else{
	print("nil")
}
//name is yagom

// 옵셔널 바인딩을 통한 임시 변수 할당
if var name = myName{
	name = "kevin"
	print("my name is \(name)")
}
else{
	print("nil")
}

//my name is kevin 
```

→ name 은 임시 상수/변수임. 코드 블록 외부에서 사용 불가능

여러 개의 옵셔널 값 추출 시:

```swift
var myName: String? = "yagom"
var yourName: String? = nil

if let name = myName, let friend = yourName{
	print("\(name), \(friend)")
}
//friend 에 바인딩이 되지 않으므로 오류

yourName = "eric"

if let name = myName, let friend = yourName{
	print("\(name), \(friend)")
}
//yagom eric
```

**암시적 추출 옵셔널**

- Implicitly Unwrapped Optionals
- 옵셔널 바인딩으로 매번 값을 추출하기 귀찮거나 오류가 발생하지 않을 것 같다는 확신이 들 때 사용
- 원래 옵셔널 표시 시 ? 를 사용, but 여기서는 ! 사용

```swift
var myName: String! = "yagom"
print(myName)
//yagom
myName = nil

if let name = myName{
	print("\(name)")
}else{
	print("nil")
}
```

→ 추천하는 방법은 아님
