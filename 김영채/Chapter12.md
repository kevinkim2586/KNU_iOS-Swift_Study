
# 접근제어 (Access Control)

- 은닉화
- 파일 간, 또는 모듈 간 접근을 제한할 수 있는 기능
- 모듈(Module) → 배초할 코드의 묶음 단위

    ex) 하나의 Framework, Library, Application 등 ... import 키워드를 통해 불러오는 것

### 접근 수준 (Access Level)

- 접근제어는 접근수준 키워드를 통해 구현 가능
- 클래스, 구조체, 열거형, 프로퍼티, 메서드, 이니셜라이저, 모두에 접근 수준 지정 가능

Keyword:

1. open
2. public
3. internal
4. fileprivate
5. private

### 공개 접근수준 - Public

- 어디서든 쓰일 수 있음
- 주로 Framework 에서 외부와 연결될 Interface 를 구현하는데 많이 쓰임

### 개방 접근수준 - Open

- public 이상으로 높은 접근수준
- 클래스와 클래스의 멤버에서만 사용 가능
- Open class 는 그 클래스가 정의된 모듈 밖의 다른 모듈에서도 사용 및 재정의 가능

### 내부 접근수준 - Internal

- 기본적으로 모든 요소에 암묵적으로 지정하는 기본 접근수준 (따로 표기 안 해도 됨)
- 소스파일이 속해 있는 모듈 어디에서든 쓰일 수 있음
- 외부 모듈에서는 접근 불가

### 파일외부비공개 접근수준 - fileprivate

- 그 요소가 구현된 소스파일 내부에서만 사용 가능

### 비공개 접근수준 - private

- 가장 한정적인 범위
- 그 기능을 정의하고 구현한 범위 내에서만 사용 가능
- 같은 소스파일 안에 구현한 다른 타입이나 기능에서도 사용 불가

note: public, open 으로 갈수록 "높은 접근수준"을 말하며, fileprivate, private 으로 내려갈수록 "낮은 접근수준"이라 말한다

### 접근제어 구현 참고사항

- 규칙 : "상위 요소보다 하위 요소가 더 높은 접근 수준을 가질 수 없다"

ex) private 으로 설정한 구조체 내부의 프로퍼티로 open, public 설정 불가능

```swift
private class AClass{

	//AClass 가 private이기 때문에 someMethod 도 private으로 취급 
	public func someMethod(){
	...
	}
}
```

- 튜플의 내부 요소 타입 또한 튜플의 접근수준보다 같거나 높아야 함. 더 낮으면 안 됨

```swift
internal class InternalClass { }
private struct PrivateStruct { }

public var publicTuple: (first: InternalClass, second: PrivateStruct) 
												= (InternalClass(), PrivcateStruct())
//InternalClass 와 PrivateStruct 의 Access Level 이 public 보다 낮기 때문에 사용 불가

private var privateTuple: (first: InternalClass, second: PirvateStruct)
												= (InternalClass(), PrivateStruct())
//Access Level 이 같거나 높기 때문에 사용 가능
```

### Private & fileprivate

**private:**

- 같은 파일 내부에 다른 타입의 코드도 접근 불가
- extension 코드가 같은 파일에 존재하는 경우에 접근 가능

**fileprivate:**

- 같은 파일 어떤 코드에서도 접근 가능
