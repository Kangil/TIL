# public 클래스에서는 public 필드가 아닌 접근자 메서드를 사용하라
- public 클래스는 접근자를 제공
    - 이를 통해 내부 구현을 얼마든지 유연하게 변경 가능
    - public 필드로 외부에 노출하지 말것
    - 불변객체라도 필드 읽을때 부수 작업을 할 수 없는 부작용 존재
- package-private 클래스나 private 중첩 클래스
    - 접근자 아닌 데이터 필드 노출로도 가능

