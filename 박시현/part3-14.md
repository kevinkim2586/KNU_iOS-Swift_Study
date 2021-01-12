<h1>Chapter 14. 옵셔널 체이닝과 빠른 종료</h1>

<h2>14.1 옵셔널 체이닝</h2>

옵셔널 체이닝 - 옵셔널에 속해 있는 nil일지도 모르는 프로퍼티, 메서드, 서브스크립션 등을 가져오거나 호출할 때 사용할 수 있는 일련의 과정

옵셔널이 nil이라면 프로퍼티, 메서드, 서브스크립트 등은 nil을 반환함

```swift
class Room { // 호실
  var number: Int
  
  init(number: Int) {
    self.number = number
  }
}

class Building {
  var name: String
  var room: Room?
  
  init(name: String) {
    self.name = name
  }
}

struct Address { // 주소
  var province: String // 광역시/도
  var city: String // 시/군/구
  var street: String // 도로명
  var building: Building? // 건물
  va rdetailAddress: String? // 건물 외 상세 주소
}

class Person { // 사람
  var name: String // 이름
  var address: Address? // 주소
  
  init(name: String) {
    self.name = name  
  }
}

let yagom: Person = Person(name: "yagom") // yagom 이라는 사람의 인스턴스를 생성

// 옵셔널 체이닝 문법

let yagomRoomViaOptionalChaining: Int? = yagom.address?.building?.room?.number // nil

let yagomRoomViaOptionalUnwraping: Int = yagom.address!.building!.room!.number // 오류 발생!
```

옵셔널 바인딩의 사용

```swift
let yagom: Person = Person(name: "yagom")

var roomNumber: Int? = nil

if let yagomAddress: Address = yagom.address {
  if let yagomBuilding: Building = yagomAddress.building {
    if let yagomRoom: Room = yagomBuilding.room {
      roomNumber = yagomRoom.number
    }
  }
}

if let number: Int = roomNumber {
  print(number)
} else {
  print("Can not find room number")
}
```
옵셔널 체이닝의 사용

```swift
let yagom: Person = Person(name: "yagom")

if let roomNumber: Int = yagom.address?.building?.room?.number {
  print(roomNumber)
} else {
  print("Can not find room number")
}
```
<h2>14.2 빠른 종료</h2>
