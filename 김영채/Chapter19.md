# Chapter 19 : 타입캐스팅  
- 스위프트는 Implicit Type Conversion 을 지원하지 않음 → Type safe 언어

```swift
var value: Double = 3.3
var convertedValue: Int = Int(value_
convertedValue = 5.5    //오류!
```

### 스위프트 타입캐스팅

→ 스위프트의 타입캐스팅은 인스턴스의 타입을 확인하거나 자신을 다른 타입의 인스턴스인양 행세할 수 있는 방법으로 사용 가능

- is, as 연산자로 구현

    → 이 두 연산자로 값의 타입을 확인 or 다른 타입으로 전환(Cast) 가능

### 데이터 타입 확인

- is 연산자를 사용하여 인스턴스가 어떤 클래스의 인스턴스인지 타입 확인 가능
- true or false 반환
- 모든 데이터 타입에 사용 가능

```swift
let coffee: Coffee = Coffee(shot: 1)

let myCoffee: Americano = Americano(shot: 2, iced: false)

print(coffee is Coffee) // true
print(coffee is Americano) //false
```

### 다운캐스팅

- 자식클래스보다 더 상위에 있는 부모클래스의 타입을 자식클래스의 타입으로 캐스팅한다고 해서 다운캐스팅이라 함
- 타입 캐스트 연산자 (Type Cast Operator): as! & as? 2가지가 있음

→ 다운캐스팅은 실패의 여지가 있기 때문에 ? 와 !가 있음.

→ as? 연산자는 실패할 경우 nil 을 반환하지만 as!는 강제 다운캐스팅이니 실패할 경우 런타임 오류 발생

```swift
class MediaItem {
    var name: String
    init(name: String) {
        self.name = name
    }
}

class Movie: MediaItem {
    var director: String
    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String
    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}

let library = [
    Movie(name: "Casablanca", director: "Michael Curtiz"),
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"),
    Movie(name: "Citizen Kane", director: "Orson Welles"),
    Song(name: "The One And Only", artist: "Chesney Hawkes"),
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley")
]

for item in library {
    if let movie = item as? Movie {
        print("Movie: \(movie.name), dir. \(movie.director)")
    } else if let song = item as? Song {
        print("Song: \(song.name), by \(song.artist)")
    }
}

// Movie: Casablanca, dir. Michael Curtiz
// Song: Blue Suede Shoes, by Elvis Presley
// Movie: Citizen Kane, dir. Orson Welles
// Song: The One And Only, by Chesney Hawkes
// Song: Never Gonna Give You Up, by Rick Astley

//library 배열에 있는 인스턴스는 모두 Movie 아니면 Song 으로 다운캐스팅 가능
```

### Any, AnyObject 의 타입캐스팅

- `Any` can represent an instance of any type at all, including function types.
- `AnyObject` can represent an instance of any class type.

```swift
var things = [Any]()

things.append(0)
things.append(0.0)
things.append(42)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })

for thing in things {
    switch thing {
    case 0 as Int:
        print("zero as an Int")
    case 0 as Double:
        print("zero as a Double")
    case let someInt as Int:
        print("an integer value of \(someInt)")
    case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let someString as String:
        print("a string value of \"\(someString)\"")
    case let (x, y) as (Double, Double):
        print("an (x, y) point at \(x), \(y)")
    case let movie as Movie:
        print("a movie called \(movie.name), dir. \(movie.director)")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}

// zero as an Int
// zero as a Double
// an integer value of 42
// a positive double value of 3.14159
// a string value of "hello"
// an (x, y) point at 3.0, 5.0
// a movie called Ghostbusters, dir. Ivan Reitman
// Hello, Michael
```
