# Chapter17 서브스크립트

> 인덱스를 통해 값을 설정하거나 가져올 수 있게 만드는 것  
> 오버로드 가능  

## 17.1 서브스크립트 문법

~~~ swift
subscript(item: Int) -> String {
    get {
        // 적절한 서브스크립트 결과값 반환
    }
    set {
        // 적절한 설정자 역할 수행
        // newValue: 설정자의 암시적 전달인자. 여기서는 String형임
    }
}
~~~

읽기 전용 서브스크립트 정의 문법
~~~ swift
subscript(index: Int) -> Int {
    get {
        // 적절한 서브스크립트 결과값 반환
    }
}
// or
subscript(index: Int) -> Int {
    // 적절한 서브스크립트 결과값 반환
}
~~~

## 17.2 서브스크립트 구현

17.3절에서 함께 설명

## 17.3 복수 서브스크립트

~~~ swift
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
    
    subscript(index: Int) -> Student? {        // 첫 번째 서브스크립트
        // get은 print(school[4])와 같이 인덱스로 값을 접근할 때 사용
        get {
            if index < self.number {
                return self.students[index]
            }
            return nil
        }
        // set은 school[4] = newStudent와 같이 인덱스로 값을 수정할 때 사용
        set {
            guard var newStudent: Student = newValue else {
                return
            }
            
            guard index < self.number else {
                self.addStudent(name: newStudent.name)
                return
            }
            
            newStudent.number = index
            self.students[index] = newStudent
        }
    }
    
    subscript(name: String) -> Int? {        // 두 번째 서브스크립트
        get {
            return self.students.filter{ $0.name == name }.first?.number
        }
        
        set {
            guard let number: Int = newValue else {
                return
            }
            
            guard number < self.number else {
                self.addStudent(name: name)
                return
            }

            self.students[number].name = name
        }
    }
    
    subscript(name: String, number: Int) -> Student? {    // 세 번째 서브스크립트
        return self.students.filter{ $0.name == name && $0.number == number }.first
    }
}

let highSchool: School = School()
highSchool.addStudents(names: "MiJeong","JuHyun", "JiYoung", "SeongUk", "MoonDuk")

// 4번째 자리에 "yuju" 추가
highSchool[4] = Student(name: "yuju", number: 11)
// 잘 들어갔는지 확인하기
let aStudent: Student? = highSchool[4]
print("\(aStudent?.number) \(aStudent?.name)")              // Optional(4) Optional("yuju")

// "MiJeong"을 1번째 자리에 추가
highSchool["MiJeong"] = 1
// 잘 들어갔는지 확인하기
print(highSchool.students.map{ (student) -> String in
    return student.name
}) // ["MiJeong", "MiJeong", "JiYoung", "SeongUk", "yuju"]

// *print 비교
print(highSchool.students) // [__lldb_expr_48.Student(name: "MiJeong", number: 0), __lldb_expr_48.Student(name: "MiJeong", number: 1), __lldb_expr_48.Student(name: "JiYoung", number: 2), __lldb_expr_48.Student(name: "SeongUk", number: 3), __lldb_expr_48.Student(name: "yuju", number: 4)]
~~~

## 17.4 타입 서브스크립트

> static 또는 class(class내부에서 사용) 키워드를 사용  

~~~ swift
enum School: Int {
    case elementary = 1, middle, high, university
    
    static subscript(level: Int) -> School? {
        return Self(rawValue: level)
        // return School(rawValue: level)과 같은 표현입니다
    }
}

let school: School? = School[2]
print(school)    // School.middle
~~~