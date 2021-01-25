# Chapter24 타입 중첩

> 타입 내부에 타입을 정의하고 구현하는 것  

## 24.1 중첩 데이터 타입

~~~ swift
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
~~~