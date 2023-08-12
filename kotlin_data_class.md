# Data Class

- [참고 블로그](https://velog.io/@haero_kim/Kotlin-%EA%B0%90%EB%8F%99-%EC%8B%A4%ED%99%94-Data-Class-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0)
- Data Class란 : 데이터를 담기위한 클래스, 다양한 메소드를 자동으로 생성해준다
    - 자바 개발자들의 고충과 니즈를 파악하여 자바의 비대한 보일러 플레이트 코드를 줄였다고 한다
- Data Class 특징
    - 기본 생성자에 1개 이상의 파라미터가 있을 것
    - 기본 생성자 파라미터는 val, var로 선언할 것
    - 다른 클래스 상속받을 수 없음 (슈퍼 클래스 가질 수 없음)
        - but, v1.1 이후로 sealed 클래스는 상속 가능하며 인터페이스 구현 가능
    - abstract, open, sealed, inner 등 키워드 붙일 수 없음
    - 자동 생성한 메소드를 오버라이딩할 경우 오버라이드 된 메소드 사용
- Data Class 메소드
    - 사용법
        
        ```kotlin
        // 데이터 클래스 선언
        data class People(
        	val name: String,
        	val age: Int
        )
        
        // 객체 생성하여 사용
        fun main() {
        	val peopleA = People("nameA", 23)
        	val peopleB = People("nameB", 33)
        }
        ```
        
    - toString() 메소드 - 디버깅 시 유용 (필자가 꼽은 베스트 장점)
        
        ```kotlin
        println(peopleA)
        // People(name=nameA, age 23)
        ```
        
    - copy() 메소드 - 특정 필드값만 변경 복사
        
        ```kotlin
        val olderPeopleA = peopleA.copy(age = 33)
        println(olderPeopelA)
        // People(name=nameA, age=33)
        ```
        
    - hashCode() 메소드
        
        ```kotlin
        val peopleA = People("nameA", 23)
        val peopleB = People("nameA", 23)
        
        println(peopleA.hashCode())
        println(peopleB.hashCode())
        
        //2110922579
        //2110922579
        // -> 같은 hashcode 값을 가짐
        // -> 일반클래스는 다른 값을 가짐
        ```
        
    - equals() 메소드
        
        ```kotlin
        println(peopleA == peopleB)
        // true (두 객체 프로퍼티가 같음)
        println(peopleA === peopleB)
        // false (두 객체 메모리 주소가 다름)
        ```
        
    - componentN() 메소드 - 프로퍼티에 번호를 붙여 구조 분해가 가능해짐
        
        ```kotlin
        val peopleA = People("nameA", 23)
        
        // 이 두줄을 아래 한줄로 변경 가능하다
        val name = peopleA.component1()
        val age = peopleB.component2()
        
        // 요렇게 한줄로 변경이 가능
        val (name, age) = peopleA
        
        println("Destructuring Declarations : $name, $age")
        // Destructuring Declarations : nameA, 23
        // Destructuring Declarations는 구조 분해
        ```
        
- 추가내용
    - 자바도 JDK14부터 Record Class라는 것을 도입하면서 코틀린의 Data Class와 동일한 기능을 사용할 수 있게 되었따고 한다.
    - 끗
