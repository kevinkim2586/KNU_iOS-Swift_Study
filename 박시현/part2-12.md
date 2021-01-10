<h1>Chapter 12. 접근제어</h1>

<h2>12.1 접근제어란</h2>

접근제어 - 코드끼리 상호작용을 할 때 파일 간 또는 모듈 간에 접근을 제한할 수 있는 기능

<h3>12.1.1 접근제어의 필요성</h3>

객체지향 프로그래밍에서 캡슐화와 은닉화를 구현하는 이유는 외부에서 보거나 접근하면 안 되는 코드가 있기 때문인데 이떄 접근제어를 이용함

<h3>12.1.2 모듈과 소스파일</h3>

모듈 - 배포할 코드의 묶음 단위 스위프트에서는 import 키워드를 사용함

소스파일 - 하나의 스위프트 소스 코드 파일을 의미

<h2>12.2 접근수준</h2>

* 개방 접근수준 - open , 접근도 - 높음 , 범위 - 모듈 외부까지 , 비고 - 클래스에서만 사용

* 공개 접근수준 - public, 접근도 - 조금 높음, 범위 - 모듈 외부까지, 비고 - x

* 내부 접근수준 - Internal, 접근도 - 보통, 범위 - 모듈 내부, 비고 - x

* 파일외부비공개 접근수준 - fileprivate, 접근도 - 조금 낮음, 범위 - 파일 내부, 비고 - x

* 비공개 접근수준 - private, 접근도 - 낮음, 범위 - 기능 정의 내부, 비고 - x

<h3>12.2.1 공개 접근수준 - public</h3>

public 키워드로 접근수준이 지정된 요소는 어디서든 쓰일 수 있다

```swift
A value type whose instances are either 'true' or 'false'.
public struct Bool {
  /// Default-initialize Boolean value to 'false'.
  public init()
}
```

<h3>12.2.2 개방 접근수준 - open</h3>

클래스에서만 사용이 가능하고, 공개 접근수준과 차이점이 있음

* 개방 접근수준을 제외한 다른 모든 접근수준의 클래스는 그 클래스가 정의된 모듈 안엥서만 상속할 수 있다

* 개방 접근수준을 제외한 다른 모든 접근수준의 클래스 멤버는 해당 멤버가 정의된 모듈 안에서만 재정의 할 수 있다

* 개방 접근수준의 클래스는 그 클래스가 정의된 모듈 밖의 다른 모듈에서도 상속할 수 있다

* 개방 접근수준의 클래스 멤버는 해당 멤버가 정의된 모듈 밖의 다른 모듈에서도 재정의할 수 있다.

```swift
open class NSSstring : NSObject, NSCopying, NSMutableCopying, NSSecureCoding {
  open var length: Int { get }
  open func character(at index: Int) -> unichar
  public init()
  public init?(coder aDecoder: NSCoder)
}
```

<h3>12.2.3 내부 접근수준 - internal</h3>

내부 접근수준은 기본적으로 모든 요소에 암묵적으로 지정하는 기본 접근수준

<h3>12.2.4 파일외부비공개 접근수준 - fileprivate</h3>

파일외부비공개 접근수준으로 지정된 요소는 그 요소가 구현된 소스파일 내부에서만 사용가능

<h3>12.2.5 비공개 접근수준 - private</h3>

비공개 접근수준은 가장 한정적인 범위 , 그 기능을 정의하고 구현한 범위 내에서만 사용가능

<h2>12.3 접근제어 구현</h2>

각각의 접근수준을 요소앞에 지정해주기만 하면 된다 internal은 기본 접근수준이므로 굳이 표기해주지 않아도 된다

```swift
open class OpenClass {
  open var openProperty: Int = 0
  public var publicProperty: Int = 0
  internal var internalProperty: Int = 0
  fileprivate var filePrivateProperty: Int = 0
  private var privateProperty: Int = 0
  
  open func openMethod() {}
  public func publicMethod() {}
  internal func internalMethod() {}
  fileprivate func fileprivateMethod() {}
  private func privateMethod() {}
}
```

<h2>12.4 접근제어 구현 참고사항</h2>

접근수준의 규칙에는 '상위 요소보다 하위 요소가 더 높은 접근수준을 가질 수 없다'이다

```swift
private class AClass {
  // 공개 접근수준을 부여해도 AClass의 접근수준이 비공개 접근수준이므로 이 메서드의 접근수준도 비공개 접근수준으로 취급된다
  public func someMethod() {
    //...
  }
}

// AClass의 접근수준이 비공개 접근수준이므로 공개 접근수준의 함수의 매개변수나 반환 값 타입으로 사용할 수 없다
public func someFunction(a: AClass) -> AClass { // 오류 발생!
  return a
}
```

<h2>12.5 private와 fileprivate</h2>

같은 파일 내부에서 fileprivate 접근수준으로 지정한 요소는 같은 파일 어떤 코드에서도 접근이 가능하다 그러나 private 접근수준으로 지정한 요소는 같은 파일 내부에 다른 타입의 코드가

있더라도 접근이 불가능하다

```swift
public struct SomeType {
  private var privateVariable = 0
  fileprivate var fileprivateVariable = 0
}

// 같은 타입의 익스텐션에서는 private 요소에 접근 가능
extension SomeType {
  public func publicMethod() {
    print("\(self.privateVariable), \(self.fileprivateVariable)")
  }
  
  private func privateMethod() {
    print("\(self.privateVariable), \(self.fileprivateVariable)")
  }
  
  fileprivate func fileprivateMethod() {
    print("\(self.privateVariable), \(self.fileprivateVariable)")
  }
}

struct AnotherType {
  var someInstance: SomeType = SomeType()
  
  mutating func someMethod() {
    // public 접근수준에서는 어디서든 접근 가능
    self.someInstance.publicMethod() // 0, 0
    
    // 같은 파일에 속해 있는 코드이므로 fileprivate 접근수준 요소에 접근 가능
    self.someInstance.fileprivateVaribale = 100
    self.someInstance.fileprivateMethod() // 0, 100
    
    // 다른 타입 내부의 코드이므로 private 요소에 접근 불가! 오류!
    // self.someInstance.privateVariable = 100
    // self.someInstance.privateMethod()
  }
}

var anotherInstance = AnotherType()
anotherInstance.someMethod()
```
<h2>12.6 읽기 전용 구현</h2>

값을 변경할 수 없도록 구현하고 싶을때 -> 설정자(Setter)만 더 낮은 접근수준을 갖도록 제한할 수 있음. 

접근수준 키워드 뒤에 (set)처럼 표현하면 설정자의 접근수준만 더 낮도록 지정해줄 수 있음
