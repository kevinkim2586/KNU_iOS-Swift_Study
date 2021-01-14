# Chapter14 옵셔널 체이닝과 빠른 종료

## 14.1 옵셔널 체이닝

> 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정  
> 옵셔널 체이닝의 반환된 값은 항상 옵셔널(강제추출(!)을 하지 않았다면)  

~~~ swift
class Room {                    // 호실
    var number: Int             // 호실 번호
    
    init(number: Int) {
        self.number = number
    }
}

class Building {                // 건물
    var name: String            // 건물 이름
    var room: Room?             // 호실 정보
    
    init(name: String) {
        self.name = name
    }
}

struct Address {                // 주소
    var province: String        // 광역시/도
    var city: String            // 시/군/구
    var street: String          // 도로명
    var building: Building?     // 건물
    var detailAddress: String?  // 건물 외 상세 주소
}

class Person {                   // 사람
    var name: String             // 이름
    var address: Address?        // 주소
    
    init(name: String) {
        self.name = name
    }
}
~~~

옵셔널 체이닝
~~~ swift
let jin: Person = Person(name: "jin")

let jinRoomViaOptionalChaining: Int? = jin.address?.building?.room?.number      // nil
~~~

옵셔널 체이닝 & 옵셔널 바인딩
~~~ swift
if let roomNumber: Int = jin.address?.building?.room?.number {
    print(roomNumber)
} else {
    print("Cannot find room number")
}
~~~

옵셔널 체이닝을 통한 값 할당 시도
~~~ swift
jin.address?.building?.room?.number = 505
print(jin.address?.building?.room?.number)                                      // nil
~~~

옵셔널 체이닝을 통한 값 할당
~~~ swift
jin.address = Address(province: "대구광역시", city: "북구", street: "대현로", building: nil, detailAddress: nil)
jin.address?.building = Building(name: "jinHouse")
jin.address?.building?.room = Room(number: 0)
jin.address?.building?.room?.number = 505

print(jin.address?.building?.room?.number)                                      // Optional(505)
~~~

## 14.2 빠른 종료

> guard else 키워드 사용  

~~~ swift
guard Bool_타입_값 else {
    예외사항_실행문
    제어문_전환_명령어
}
~~~

응용
~~~ swift
func enterClub(name: String?, age: Int?) {
    guard let name: String = name, let age: Int = age, age > 19, !name.isEmpty else {
        print("You are too young to enter to club!!!")
        return
    }
    print("Welcome \(name)!")
}

enterClub(name: "jin", age: 27)                 // Welcome jin!
enterClub(name: "goding", age: 18)              // You are too young to enter to club!!!
~~~

