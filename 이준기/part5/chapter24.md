# Chapter24 타입 중첩

- 타입 내부에 타입을 정의하고 구현할 때 사용
- 타입 내부에 새로운 타입을 선언해주는 것을 중첩 타입이라고 한다.

## 24.1 중첩 데이터 타입

- 함수를 중첩해 사용 했듯이 클래스 내부에 새 클래스, 클래스 내부에 새 구조체, 구조체 내부에 새 열거형 등의 타입을 중첩해서 타입 내부에 새 타입을 정의할 수 있음

- 중첩 데이터 타입 구현

```swift
class Person {
    enum Job {
        case jobless, programmer, student
    }
    
    var job: Job = .jobless
}

class Student: Person {
    
    enum School {
        case elementary, middle, high
    }
    
    var school: School
    
    init(school: School) {
        self.school = school
        super.init()
        self.job = .student
    }
}


let personJob: Person.Job = .jobless
let studentJob: Student.Job = .student

let student: Student = Student(school: .middle)
print(student.job)      // student
print(student.school)   // middle
```

- 같은 이름의 중첩 데이터 타입 구현

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


var basketball: Sports = Sports(gameType: .basketball)
print(basketball.gameInfo)  // (time: 40, player: 5)

var sudden: ESports = ESports(gameType: .offline)
print(sudden.gameInfo)      // (location: "제주", pakage: "SA")

let someGameType: Sports.GameType = .football
let anotherGameType: ESports.GameType = .online

/*
let errorIfYouWantIt: Sports.GameType = .online // 오류 발생
*/
```