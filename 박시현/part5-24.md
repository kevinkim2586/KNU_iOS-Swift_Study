<h1>Chapter 24. 타입 중첩</h1>

중첩 타입 - 타입 내부에 새로운 타입을 선언해준 것

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

let personJob: Person.job = .jobless
let studentJob: Student.Job = .student

let student: Student = Student(school: .middle)
print(student.job) // student
print(student.school) // middle
```

<h2>24.1 중첩 데이터 타입</h2>

중첩 데이터 타입을 사용할 때는 자신을 둘러싼 타입의 이름을 자신보다 앞에 적어줘야 함


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
 print(basketball.gameInfo) // (time: 40, player: 5)
 
 var sudden: ESports = ESports(gameType: .offline)
 print(sudden.gameInfo) // (location: "제주", pakage: "SA")
 
 let someGameType: Sports.GameType = .football
 let anotherGameType: ESports.GameType = .online
 let errorIfYouWantIt: Sports.GameType = .online //오류발생
 ```
