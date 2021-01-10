# Chapter9 구조체와 클래스

> 데이터를 용도에 맞게 묶어 표현하고자 할 때 유용  
> 구조체의 인스턴스는 값 타입, 클래스의 인스턴스는 참조 타입

## 9.1 구조체

### 9.1.1 구조체 정의

~~~ swift
struct 구조체이름 {
    프로퍼티와 메서드들
}
~~~

### 9.1.2 구조체 인스턴스의 생성 및 초기화

- 인스턴스를 생성하고 초기화하고자 할 때 기본적으로 생성되는 멤버와이즈 이니셜라이저 사용
- let으로 선언하면 인스턴스 내부 프로퍼티 값 변경 불가
- var으로 선언하면 인스턴스 내부 프로퍼티 값 변경 가능

## 9.2 클래스

### 9.2.1 클래스 정의

~~~ swift
class 클래스이름 {
    프로퍼티와 메서드들
}
~~~

### 9.2.2 클래스 인스턴스의 생성과 초기화

- 인스턴스를 생성하고 초기화하고자 할 때 기본적인 이니셜라이저 사용
- let으로 선언해도 인스턴스 내부 프로퍼티 값 변경 가능

### 9.2.3 클래스  인스턴스의 소멸

- 클래스의 인스턴스는 더는 참조할 필요가 없을 때 메모리에서 해제됨: 소멸
- 소멸되기 직전 deinit이라는 메서드가 호출
- deinit은 클래스당 하나만 구현 가능, 매개변수와 반환 값 가지지않음

~~~ swift
class Person {
    var height: Float = 0.0
    var weight: Float = 0.0

    deinit {
        print("Person 클래스의 인스턴스가 소멸됩니다.")
    }
}

var yagom: Person? = Person()
yagom = nil                     // Person 클래스의 인스턴스가 소멸됩니다. 출력됨
~~~

## 9.3 구조체와 클래스의 차이

- 같은 점
    - 프로퍼티, 메서드, 서브스크립트 정의 가능
    - 초기화 상태 지정 위해 이니셜라이저 정의 가능
    - 새로운 기능 추가를 위해 익스텐션을 통해 확장 가능
    - 프로토콜 사용 가능
- 다른 점
    - 구조체의 인스턴스는 값 타입, 클래스의 인스턴스는 참조 타입
    - 구조체는 상속 불가
    - 타입캐스팅은 클래스의 인스턴스에만 허용
    - Deinitializer는 클래스의 인스턴스에만 활용 가능
    - 참조 횟수 계산은 클래스의 인스턴스에만 적용

### 9.3.1 값 타입과 참조 타입

~~~ swift
struct BasicInformation {
    let name: String
    var age: Int
}

var yagomInfo: BasicInformation = BasicInformation(name: "yagom", age: 99)
var friendInfo: BasicInformation = yagomInfo
friendInfo.age = 999
print("yagom's age: \(yagomInfo.age)")      // 99
print("friend's age: \(friendInfo.age)")    // 999

class Person {
    var height: Float = 0.0
    var weight: Float = 0.0
}

var yagom: Person = Person()
var friend: Person = yagom

friend.height = 134.4

print("yagom's height: \(yagom.height)")    // 134.4
print("friend's height: \(friend.height)")  // 134.4
~~~

식별 연산자(===, !==)

~~~ swift
var yagom: Person = Person()
var friend: Person = yagom
var anotherFriend: Person = Person()

print(yagom === friend)                     // true
print(yagom === anotherFriend)              // false
print(friend !== anotherFriend)             // true
~~~

### 9.3.2 스위프트의 기본 데이터 타입은 모두 구조체

기본 타입(Bool, Int, String, Array, Dictionary, Set 등등)은 모두 구조체로 구현되어 있음  
즉, 값 타입

## 9.4 구조체와 클래스 선택해서 사용하기

**애플의 가이드라인**
- 다음의 경우 구조체 사용 권장
    - 연관된 간단한 값의 집합을 캡슐화하는 것만이 목적일 때
    - 캡슐화한 값을 참조하는 것보다 복사하는 것이 합당할 때
    - 구조체에 저장된 프로퍼티가 값 타입이며 참조하는 것보다 복사하는 것이 합당할 때
    - 다른 타입으로부터 상속받거나 자신을 상속할 필요가 없을 때
