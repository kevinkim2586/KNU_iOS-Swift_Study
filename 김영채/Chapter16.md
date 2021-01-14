# Chapter 16 : 모나드 (Monda)
모나드: 값이 있을 수도 있고 없을 수도 있는 컨텍스트를 가지는 함수 객체 타입

### 컨텍스트 (Context)

- Context = 'Contents 를 담은 그 무엇인가'

    ex) 물컵에 물이 담겨있으면 물은 Content, 물컵은 Context

    → 값을 넣을 수 있는 "상자"

- 2라는 숫자 옵셔널이 있으면, 컨텍스트 안에 2라는 콘텐츠가 들어가 있음.

    → 컨텍스트는 존재하지만 내부에 값이 없을 수도 있음

```swift
func addThree(num: Int) -> Int{
	return num + 3
}

addThree(2)
//정상 실행

addThree(Optional(2))
//오류 발생
//순수한 값이 아닌 옵셔널이라는 컨텍스트로 둘러싸여 전달되었기 때문 
```

### 함수 객체 (Functor)

함수 객체란 맵을 적용할 수 있는 컨테이너 타입.

```swift
Optional(2).map(addThree)
//Optional(5)
```

- Array, Dictionary, Set 등등 많은 컬렉션 타입이 함수 객체임

```swift
var myNumber: Int? = 2
print(myNumber + 3)
//error -> Int? 와 Int 는 엄연히 다른 타입
//myNumber 는 컨텍스트임. 거기다가 바로 Int 더하기는 안 됨
//그래서 이걸 꺼내는 작업 필요 -> map

var myNumber: Int? = 2
print(myNumber.map { $0 ++ 3 }
//Optional(5) 
```

→ map 이라는게 요소 하나하나를 보면서 context 내에 값이 있는지 없는지 확인함. 있으면 요소를 꺼냄.

→ ** 근데 왜 프린트가 Optional(5) 로 되냐? map은 꺼낸 이 값을 context 에 넣어서 돌려주기 때문이다.

- 함수 객체란 맵을 적용할 수 있는 컨테이너 타입 → Optional 에 map 을 적용함. 그럼 Optional == 컨테이너, so 옵셔널은 함수객체임

### 모나드 (Monad)

- 함수객체 중에서 자신의 컨텍스트와 같은 컨텍스트의 형태로 맵핑할 수 있는 함수객체를 닫힌 함수객체 (Endofunctor)라고 함.

    → 모나드는 닫힌 함수객체

- 모나드는 값이 있을 수도 있고 없을 수도 있는 컨텍스트를 가지는 함수 객체 타입

모나드는 값이 있을수도 있고, 없을 수도 있는 컨텍스트(맥락)을 가지는 map을 적용 할 수 있는 타입이라는 것

ex) 옵셔널은 정의 자체가 값이 있을 수도, 없을 수도 있는 타입이며, map 까지 적용 가능한 함수 객체. So, Optional == Monad

** Optional 은 함수객체이면서 모다드이다.

---

추가 이해 필요
