함수

기본 형태의 함수 정의와 사용

1. 함수선언의 기본형태
```swift
func 함수이름(매개변수1이름:매개변수1타입, 매개변수2이름:매개변수2타입...)->반환타입 {
        /*함수 구현부*/
    return 반환값
}

//예)
//sum이라는 이름을 가지고
//a와 b라는 Int 타입의 매개변수를 가지며
//Int 타입의 값을 반환하는 함수
func sum(a:Int, b:Int)->Int{
    return a + b
}
```
2. 반환 값이 없는 함수

```swift
func 함수이름(매개변수1이름:매개변수1타입, 매개변수2이름:매개변수2타입...)->Void {
        /*함수 구현부*/
    return
}

//예)
func printMyName(name:String)->Void{
    print(name)
}

//반환 값이 없는 경우, 반환 타입(Void)을 생략해 줄 수 있다
func printYourName(name:String){
    print(name)
}
```

3. 매개변수가 없는 함수
```swift
func 함수이름()->반환타입{
    /*함수 구현부*/
    return 반환값
}

//예)
func maximumIntegerValue()->Int{
    return Int.max
}
```

4. 매개변수와 반환값이 없는 함수
```swfit
func 함수이름()->Void{
    /*함수 구현부*/
    return
}

//함수 구현이 짧은 경우
//가독성을 해치지 않는 범위에서
//줄바꿈을 하지 않고 한 줄에 표현해도 무관하다
func hello()->Void{print("hello")}

//반환 값이 없는 경우, 반환타입(Void)은 생략가능하다
func 함수이름(){
    /*함수 구현부*/
    return
}
func sayHelloWorld() {
    print("Hello, world!")
}
sayHelloWorld() // Hello,world!

func sayHello(from myName: String, to name: String){
    print("Hello \(name)! I'm \(myName)")
}
sayHello(from: "yagom", to: "Minjeong")// Hello Mijeong! I'm yagom

func bye(){print("bye")}
```

5. 함수의 호출
```swift
sum(a: 3, b: 5)//8
printMyName(name: "sihyun")//sihyun

printYourName(name: "haha")//haha

maximumIntegerValue()//Int의 최댓값

hello()//hello

bye()//bye

func hello(name: String) -> String {
    return "Hello \(name)!"
}

let helloJenny: String = hello(name: "Jenny")
print(helloJenny) // Hello jenny!

func introduce(name: String) -> String {
    // return "제 이름은" + name + "입니다" return 생략가능
}
let introduceJenny: String = introduce(name: "Jenny")
print(introduceJenny) // 제 이름은 Jenny입니다
```

매개변수가 여러 개인 함수의 정의와 사용
```swift
func sayHello(myName: String, yourName: String) -> String {
    return "Hello \(yourName)! I'm  \(myName)"
}

print(sayHello(myName: "yagom", yourName: "Jenny")) //Hello Jenny! I'm yagom
```

매개변수 이름과 전달인자 레이블을 가지는 함수 정의와 사용

```swift
func sayHello(from myName:String, to name: String) -> String {
    return "Hello \(name)! I'm \(myName)"
}
print(sayHello(from: "yagom", to: "Jenny")) // Hello Jenny! I'm yagom
```

가변 매개변수
* 전달 받을 값의 개수를 알기 어려울 때 사용한다.
* 가변 매개변수는 함수당 하나만 가질 수 있다.
* 기본값이 있는 매개변수와 같이 가변 매개변수 역시 매개변수 목록 중 뒤쪽에 위치하는 것이 좋습니다.

```swift
//func 함수이름(매개변수1이름: 매개변수1타입, 전달인자 레이블 매개변수2이름: 매개변수2타입...)->반환타입{
//  /*함수 구현부*/
//  return
//}

func sayHelloToFriends(me: String, friends: String...)->String{
    return "Hello \(friends)! I'm \(me)!"
}
print(sayHelloToFriends(me: "sihyun", friends: "hana", "eric", "wing"))
// Hello ["hana", "eric", "wing"]! I'm sihyun!

print(sayHelloToFriends(me: "sihyun"))
// Hello []! I'm sihyun!
//*반환값이 없는 함수, 매개변수 기본 값, 전달인자 레이블, 가변 매개변수 등 모두 섞어서 사용 가능하다.
```

가변 매개변수를 가지는 함수의 정의와 사용
```swfit
func sayHelloToFriends(me: String, friends names: String...) -> String {
    var result: String = ""
    
    for friend in names {
        result += "Hello \(friend)" + " "
    }
    
    result += "I'm " + me + "!"
    return result
}

print(sayHelloToFriends)(me: "yagom", friends: "Johansson", "Jay", "Wizplan")) // Hello Johansson! Hello Jay! Hello Wizplan! I'm yagom!

print(sayHelloToFriends(me: "yagom")) // I'm yagom!
```

데이터 타입으로서의 함수
```swift
func sayHello(name: String, times: Int) -> String){
    //...
} // sayHello의 함수타입은 (String, Int) -> String

func sayHelloToFriends(me: String, names: String...) -> String {
    //...
} // sayHelloToFriends 함수의 타입은 (String, String...) - > String

func sayHelloWorld() {
    //...
} // sayHelloWorld 함수의 타입은 (Void) -> Void
```

함수 타입의 사용
```swift
typealias CalculateTwoInts = (Int, Int) -> Int

func addTwoInts(_ a: Int, _ b: Int) -> Int {
    return a + b
}

func multiplyTwoInts(_ a: Int, _ b: Int) -> Int {
    return a * b
}

var mathFunction: CalculateTwoInts = addTwoInts
위 표현과 정확히 같은 표현
var mathFunction: (Int, Int) -> Int = addTwoInts
print(mathFunction(2, 5)) // 2 + 5 = 7

mathFunction = multiplyTwoInts
print(mathFunction(2, 5)) // 2 * 5 = 10

원점으로 이동하기 위한 함수
typealias MoveFunc = (Int) -> Int

func goRight(_ currentPosition: Int) -> Int {
    return currentPosition + 1
}

func goLeft(_ currentPosition: Int) -> Int {
    return currentPosition - 1
}

func functionForMove(_ shouldGoLeft: Bool) -> MoveFunc {
    return shouldGoLeft ? goLeft : goRight
}

var position: Int = 3

let moveToZero: MoveFunc = functionForMove(position > 0)
print("원점으로 갑시다.")

while position != 0 {
    print("\(position)...")
    position = moveToZero(position)
}

print("원점 도착!")
```

중첩함수를 사용했을 때
```swift
typealias MoveFunc = (Int) -> Int

func functionForMove (_ shouldGoLeft: Bool) -> MoveFunc {
    func goRight(_ currentPostion: Int) -> Int {
        return currentPosition + 1
    }
    
    func goLeft(_ currentPosition: Int) -> Int {
        return currentPosition - 1
    }
    
    return shouldGoLeft ? goLeft : goRight
}

var position: Int = -4

let moveToZero: MoveFunc = functionForMove(position > 0)

while position != 0 {
    print("\(position)...")
    position = moveToZero(position)
}

print("원점 도착!")
```
