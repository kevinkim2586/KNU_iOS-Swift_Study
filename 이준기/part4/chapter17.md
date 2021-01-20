# Chapter17 서브스크립트

> 클래스, 구조체, 열거형, 컬렉션, 리스트, 시쿼스 등 타입의 요소에 접근하는 단축 문법<br>

- 인덱스를 통해서 값을 설정하거나 가져올수 있음
- 한타입에 여러개의 서브스크립트를 정의


## 17.1 서브스크립트 문법

- subscript 키워드를 사용하여 정의한다.
- 인스턴스 매서드와 비슷하게 매개변수의 개수 타입 반환 타입을 지정 및 읽기 전용으로 구현 가능

```swift
subscript(index: Int) -> Int{
	get{
		//적절한 서브스크립트 결괏값 반환	
	}
	set(newValue){
		//적절한 설정자 역할 수행 
	}
}
```

- 읽기 전용 서브스크립트를 구현할때는 get만 사용함

```swift
subscript(index: Int) -> Int{
	get{
		//적절한 서브스크립트 결괏값 반환	
	}
	set(index: Int ) -> Int {
		//적절한 서브스크립트 결괏값 반
	}
}
```

## 17.2 서브스크립트 구현

```swift
struct Student {
    var name: String
    var number: Int
}

class School {
    
    var number: Int = 0
    var students: [Student] = [Student]()
    
    func addStudent(name: String) {
        let student: Student = Student(name: name, number: self.number)
        self.students.append(student)
        self.number += 1
    }
    
    func addStudents(names: String...) {
        for name in names {
            self.addStudent(name: name)
        }
    }
    
    subscript(index: Int) -> Student? {
        if index < self.number {
            return self.students[index]  
        }
        return nil
    }
}

let highSchool: School = School()
highSchool.addStudents(names: "MiJeong","JuHyun", "JiYoung", "SeongUk", "MoonDuk")

let aStudent: Student? = highSchool[1]
print("\(aStudent?.number) \(aStudent?.name)")    // Optional(1) Optional("JuHyun")
```

## 17.3 복수 서브스크립트

- 하나의 타입이 여러개의 서브스크립트를 가질수 있

```swift
struct Student {
    var name: String
    var number: Int
}

class School {
    
    var number: Int = 0
    var students: [Student] = [Student]()
    
    func addStudent(name: String) {
        let student: Student = Student(name: name, number: self.number)
        self.students.append(student)
        self.number += 1
    }
    
    func addStudents(names: String...) {
        for name in names {
            self.addStudent(name: name)
        }
    }
    
    subscript(index: Int) -> Student? {		// 첫 번째 서브스크립트
        get {
            if index < self.number {
                return self.students[index]  
            }
            return nil
        }
        
        set {
            guard var newStudent: Student = newValue else {
                return
            }
            
            var number: Int = index
            
            if index > self.number {
                number = self.number
                self.number += 1
            }
            
            newStudent.number = number
            self.students[number] = newStudent
        }
    }
    
    subscript(name: String) -> Int? {		// 두 번째 서브스크립트
        get {
            return self.students.filter{ $0.name == name }.first?.number
        }
        
        set {
            guard var number: Int = newValue else {
                return
            }
            
            if number > self.number {
                number = self.number
                self.number += 1
            }
            

            let newStudent: Student = Student(name: name, number: number)
            self.students[number] = newStudent
        }
    }
    
    subscript(name: String, number: Int) -> Student? {	// 세 번째 서브스크립트
        return self.students.filter{ $0.name == name && $0.number == number }.first
    }
}

let highSchool: School = School()
highSchool.addStudents(names: "MiJeong","JuHyun", "JiYoung", "SeongUk", "MoonDuk")

let aStudent: Student? = highSchool[1]
print("\(aStudent?.number) \(aStudent?.name)")    // Optional(1) Optional(“JuHyun”)

print(highSchool["MiJeong"]) // Optional(0)
print(highSchool["DongJin"]) // nil

highSchool[0] = Student(name: "HongEui", number: 0)
highSchool["MangGu"] = 1

print(highSchool["JuHyun"])      // nil
print(highSchool["MangGu"])      // Optional(1)
print(highSchool["SeongUk", 3])  // Optional(Student(name: "SeongUk", number: 3))
print(highSchool["HeeJin", 3])   // nil
```

## 17.4 타입 서브스크립트

- 인스턴스에서 사용할수 있는 서브스크립트
- subscript 키워드 앞에 static 키워드를 붙여줌

```swift
enum School: Int {
    case elementary = 1, middle, high, university
    
    static subscript(level: Int) -> School? {
        return Self(rawValue: level)
        // return School(rawValue: level)과 같은 표현입니다
    }
}

let school: School? = School[2]
print(school)    // School.middle
```