# Chapter8 옵셔녈

>안정성을 담보하는 기능<br>
>선택적인 즉, 값이 있을수도, 없을 수도 있음을 나타내는 표

## 8.1 옵셔널 사용

- \* 옵셔널 변수/상수가 아니면 nil 값 할당 불가능.\*

```swift
var myName : String = "yagom"
myName = nil // 오류!

var myName : String? = "yagom"
print(myName) // yagom
myName = nil 
print(myName) //nil
```

- 함수 매개변수가 옵셔널일 때는 해당하는 매개변수에는 값이 없어도 되는 것을 알아야함

## 8.2 옵셔널 추출

- \*옵셔널의 값을 옵셔널이 아닌 값으로 추출\*

### 8.2.1 강제 추출

- \*가장 위험한 방법\*

```swift
var myName: String? = "yagom"

//옵셔널이 아닌 변수에는 옵셔널 값이 들어갈 수 없음. 추출해서 할당해 주어야 함
var yagom: String = myName!

myName = nil
yagom = myName! // 런타임 오류

// if 구문 등 조건문을 이용해서 조금 더 안전하게 처리해볼 수 다.
if myName!=nil{
	print("name is \(myName!)")
}else{
	print("name == nil")
}
// myName == nil
```

### 8.2.2 옵셔널 바인딩

- 옵셔널에 값이 있는지 확인할 때 사용

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
	name = "wizplan" // 변수이므로 내부에서 변경이 가능합니다.
	print("my name is \(name)")
}
else{
	print("myName == nil")
}

//my name is wizplan
```

- name 은 임시 상수/변수, 즉 구문 밖에선 못 씀

### 8.2.3 암시적 추출 옵셔널

- 옵셔널에 값이 있는지 확인할 때 사용함
- 옵셔녈에서 추출한 값을 일정 블록 안에서 사용할 수 있는 상수나 변수로 할해서 옵셔널이 아닌 형태로 사용할수 있게 해줌

```swift
var myName: String! = "yagom"

// 옵셔널 바인딩을 통한 임시 변수 할당
if let name = myName{
	print("my name is \(name)")
}else{
	print("myName == nil")
}
// My name is 

// 옵셔널 바인딩을 통한 임시 변수 할당
if var name = myName{
	name = "wizplan" // 변수이므로 내부에서 변경이 가능합니다.
	print("my name is \(name)")
}else{
	print("myName == nil")
}
// My name is wizplan
```