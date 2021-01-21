# Chapter30 불명확 타입

- 반환 타입에 불명확 타입을 사용하면 반환할 타입의 정확한 타입을 알려주지 않은채로 반환하겠다는 것을 의미

- 뽑기 상품 프로토콜의 정의
```swift
protocol WrappedPrize {
    associatedtype Prize
    
    var wrapColor: String! { get }   // 포장 색상
    var prize: Prize! { get }        // 실제 상품
}
```

- 포장된 상품 프로토콜 정의
```swift
protocol Gundam { }
protocol Pokemon { }

struct WrappedGundam: WrappedPrize {
    var wrapColor: String!
    var prize: Gundam!
}

struct WrappedPokemon: WrappedPrize {
    var wrapColor: String!
    var prize: Pokemon!
}
```

- 뽑기 기계 구조체 정의
```swift
struct PrizeMachine {
	func dispenseRandomPrize()-> WrapperPrize{
		return WrappedGundam()
	}
}
```

some을 붙여서 불명확 타입으로 개선

- 뽑기 기계 구조체 개선
```swift
struct PrizeMachine {
    func dispenseRandomPrize() -> some WrappedPrize {
        return WrappedGundam()
    }
}

let machine: PrizeMachine = PrizeMachine()
let wrappedPrize = machine.dispenseRandomPrize()
```