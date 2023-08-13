# VO(Value Object)

- [레퍼런스](https://tecoble.techcourse.co.kr/post/2020-06-11-value-object/)
- Value Object : 도메인에서 한 개 또는 그 이상의 속성들을 묶어서 특정 값을 나타내는 **객체**를 의미
- VO도 객체의 일종이고 엔티티와 구별해서 사용한다.
- 엔티티란 무엇인가 - 실제 데이터베이스의 테이블과 1:1로 매핑되는 클래스. ([레퍼런스](https://dev-coco.tistory.com/87))
    - DB의 테이블안에 존재하는 컬럼만을 속성(필드)으로 가져야 함
    - 엔티티와 DTO클래스를 분리하는 이유
        - DTO클래스는 View와 통신하며 자주 변경되므로 클래스 보호를 위해 분리해야 한다네요
        - DTO란 무엇인가
            - 각 계층(Layer)간 데이터 교환을 위한 객체(Java Beans)라고 합니다
                - (예 - View ← (DTO) → Controller ← (DTO) → Service)
            - 로직이 없는 순수한 객체이며, getter/setter 메소드만 갖습니다
    - DTO와 VO의 차이점 -
        - DTO는 데이터 전송만을 위한 객체, VO는 특정한 비즈니스 로직을 가질수있음(?)
        - DTO는 데이터 전달만을 목적으로 하고 VO는 객체 자체를 Value로 사용 가능
    - VO는 equals()와 hashCode()를 제정의해서 각 객체의 동일성을 판별 가능
- 엔티티는 VO와 DTO를 포괄하는 개념(확인필요) → 오.. 이건 뭔가 이견이 많네 ..
- VO는 불변이고 데이터가 들어올때 같은 값인지 확인할 필요가 있으므로
    - equals, hash code 메서드를 재정의한다
    - 불변이어야하므로 setter를 사용하지 않는다
        - 불변해야하는 데이터를 지키지 못하는 코드
        
        ```java
        // 이 코드는 미쳤다리 기억하기 위해서 남겨놓자 << 잘못된 경우 >>
        // Order.java
        public class Order {
          private String restaurant;
          private String food;
          private int quantity;
          
          // Getter, Setter ...
          // equals & hashcode
        }
        
        // Main.java
        public static void main(String[] args) {
        
          Order 첫번째주문 = new Order();
          첫번째주문.setRestaurant("황제떡볶이");
          첫번째주문.setFood("매운떡볶이");
          첫번째주문.setQuantity(2);
          // 첫번째주문 = {restaurant='황제떡볶이', food='매운떡볶이', quantity=2}
        
          Order 두번째주문 = 첫번째주문;
          // 두번째주문 = {restaurant='황제떡볶이', food='매운떡볶이', quantity=2}
        
          두번째주문.setFood("안매운떡볶이");  //** 주문 변경
          두번째주문.setQuantity(3);       //** 주문 변경
          // 첫번째주문 = {restaurant='황제떡볶이', food='안매운떡볶이', quantity=3}
          // 두번째주문 = {restaurant='황제떡볶이', food='안매운떡볶이', quantity=3}
          
        }
        ```
        
        - 잘된 코드 - 주문 변경이 생길경우 생성자를 통해 객체를 새로 생성하고 재할당하여 VO 정체성을 지킬 수 있다
        
        ```java
        public class Order {
          private String restaurant;
          private String food;
          private int quantity;
          
          public Order(String restaurant, String food, int quantity) {
            this.restaurant = restaurant;
            this.food = food;
            this.quantity = quantity;
          }
          
          // only getter..
        }
        
        public static void main(String[] args) {
        
          Order 첫번째주문 = new Order("황제떡볶이", "매운떡볶이", 2);
          // 첫번째주문 = {restaurant='황제떡볶이', food='매운떡볶이', quantity=2}
        
          Order 두번째주문 = new Order("황제떡볶이", "매운떡볶이", 2)
          // 두번째주문 = {restaurant='황제떡볶이', food='매운떡볶이', quantity=2}
        
          두번째주문 = new Order("황제떡볶이", "안매운떡볶이", 3)  //** 주문 변경
          // 첫번째주문 = {restaurant='황제떡볶이', food='매운떡볶이', quantity=2}
          // 두번째주문 = {restaurant='황제떡볶이', food='안매운떡볶이', quantity=3}
          
        }
        ```
        
- 와 한문장 이해하기가 이렇게 빡셀줄이야 .. 이 글도 나중에 다시 읽어보자 ([블로그](https://kth990303.tistory.com/288), [블로그2](https://medium.com/@nicolopigna/value-objects-like-a-pro-f1bfc1548c72))
- 그렇다면 코틀린 VO는 뭘까 (var, val, [참고글](https://kotlinworld.com/173))
    - var : 가변 변수 (읽기, 쓰기 허용)
    - val : 불변 변수 (읽기만 허용)
- 자바에서 DTO라고 불리는 게 Data Class와 같다고 볼 수 있다고 한다. 단, VO와는 다르다. VO는 read only 특성만을 가지기 때문이다.
    - 데이터 클래스의 컴파일러는 기본 생성자에 선언된 모든 속성에서 다음 멤버를 자동으로 파생한다
        - equals() / hashCode() 쌍
        - User(name=John, age=42) 형태의 toString()
        - componentN()은 선언 순서대로 속성에 해당
        - copy()
