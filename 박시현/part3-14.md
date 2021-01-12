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

옵셔널 체이닝을 통한 값 할당
```swift
yagom.address = Address(province: "충청북도", city: "청주시 청원구", street:
  "충청대로", building: nil, detailAddress: nil)
  yagom.address?.building = Building(name: "곰굴")
  yagom.address?.building?.room = Room(number: 0)
  yagom.address?.building?.room?.number = 505
  
  print(yagom.address?.building?.room?.number) // Optional(505)
```
옵셔널 체이닝을 통한 메서드 호출

```swift
yagom.address?.fullAddress()?.isEmpty //false
yagom.address?.printAddress()
```

옵셔널 체이닝을 통한 서브스크립트 호출
```swift
let optionalArray: [Int]? = [1, 2, 3]
optionalArray?[1] // 2

var optionalDictionary: [String: [Int]]? = [String: [Int]]()
optionalDictionary?["numberArray"] = optionalArray
optionalDictionary?["numberArray"]?[2] // 3
```

<h2>14.2 빠른 종료</h2>

핵심 키워드는 guard , if 구문과 유사하게 Bool 타입의 값으로 동작한다, if 구문과 다르게 else 구문이 항상 뒤에 따라와야 한다

guard 구문의 옵셔널 바인딩 활용

```swift

func greet(_ person: [String: String]) {
  guard let name: String = person["name"] else {
     return
  }
  
  print("Hello" \(name)!")
  
  guard let location: String = person["location"] else {
    print("I hope the weather is nice near you")
    return
  }
  
  print("I hope the weather is nice in \(location)")
 }
 
 var eprsonInfo: [String: String] = [String: String]()
 personInfo["name"] = "Jenny"
 
 greet(personInfo)
 // Hello Jenny!
 // I hope the weather is nice near you
 
 personInfo["location"] = "Korea"
 
 greet(personInfo)
 // Hello Jenny!
 // I hope the weather is nice in Korea
 
```
