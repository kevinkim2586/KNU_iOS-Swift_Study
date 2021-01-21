# Chapter 24 : 중첩 타입
- 타입 내부에 타입을 정의하고 구현할 때 사용
- 타입 내부에 새로운 타입을 선언해주는 것 = 중첩 타입 (Nested Types)

```swift
class Person{
	enum Job{
		case jobless, programmer, student
	}
	var job: Job = .jobless
}

let personJob: Person.Job = .jobless
```

→ 중첩 데이터 타입을 사용할 때는 자신을 둘러싼 타입의 이름을 자신보다 앞에 적어줘야 한다!

ex. Person.Job

그냥 Job 로는 접근 불가능

When to use Nested Types?

```swift
struct Sports {
    
    enum GameType {
        case football, basketball
    }
    
    var gameType: GameType
    
    struct GameInfo {
        var time: Int
        var player: Int
    }
    
    var gameInfo: GameInfo {
        switch self.gameType {
        case .basketball:
            return GameInfo(time: 40, player: 5)
        case .football:
            return GameInfo(time: 90, player: 11)
        }
    }
}

struct ESports {
    
    enum GameType {
        case online, offline
    }
    
    var gameType: GameType
    
    struct GameInfo {
        var location: String
        var pakage: String
    }
    
    var gameInfo: GameInfo {
        switch self.gameType {
        case .online:
            return GameInfo(location: "www.liveonline.co.kr", pakage: "LoL")
        case .offline:
            return GameInfo(location: "제주", pakage: "SA")
        }
    }
}
```

→ Sports 와 ESports 구조체 내부에 모두 GameType 과 GameInfo 라는 중첩타입 구조체가 선언되어 있다. 구조체의 이름은 같을지라도, 내부의 프로퍼티나 메서드는 다르다. 즉, 외부에 선언해서 공용으로 사용하기에는 부적합하다는 것이다. 

→ 목적에 따라 타입을 중첩
