# Chapter15 맵, 필터, 리듀스

> 스위프트에서 유용한 대표적인 고차함수  
> *고차함수: 매개변수로 함수를 갖는 함수

## 15.1 맵

> Sequence, Collection 프로토콜을 따르는 타입과 옵셔널에서 사용 가능  
> 컨테이너가 갖고 있던 각각의 값을 매개변수를 통해 받은 함수에 적용한 후 다시 컨테이너에 포장하여 반환  

~~~ swift
let numbers: [Int] = [0, 1, 2, 3, 4]

let numbersInString: [String] = numbers.map { (number: Int) -> String in
    return String(number)
}

print(numbersInString)          // ["0", "1", "2", "3", "4"]
~~~

## 15.2 필터

> 컨테이너 내부의 값을 걸러서 추출해줌  
> filter 함수의 매개변수로 전달되는 함수의 반환 타입은 Bool이다(true: 포함, false: 미포함)  

~~~ swift
let numbers: [Int] = [0, 1, 2, 3, 4, 5]

let oddNumbers: [Int] = numbers.filter( { (number: Int) -> Bool in
    return number % 2 == 1
})

print(oddNumbers)              // [1, 3, 5]
~~~

## 15.3 리듀스

첫번째 형태의 reduce 사용
~~~ swift
// 첫 번째 형태인 reduce(_:_:) 메서드의 사용

let numbers: [Int] = [1, 2, 3]

// 초깃값이 1이고 정수 배열의 모든 값을 더한다
var sumFromOne: Int = numbers.reduce(1) { (result: Int, next: Int) -> Int in
    print("\(result) + \(next)")
    // 1 + 1
    // 2 + 2
    // 4 + 3
    return result + next
}

print(sumFromOne)      // 7
~~~

두번째 형태의 reduce 사용
~~~ swift
// 두 번째 형태인 reduce(into:_:) 메서드의 사용

let numbers: [Int] = [1, 2, 3]

// 초깃값이 1이고 정수 배열의 모든 값을 더한다
// 첫 번째 리듀스 형태와 달리 클로저의 값을 반환하지 않고 내부에서
// 직접 이전 값을 변경한다는 점이 다르다
var sumFromOne: Int = numbers.reduce(into: 1) { (result: inout Int, next: Int) in
    print("\(result) + \(next)")
    // 1 + 1
    // 2 + 2
    // 4 + 3
    result += next
}

print(sumFromOne)      // 7
~~~

## 15.4 맵, 필터, 리듀스의 활용

**다시 본 후 수정추가하기**
