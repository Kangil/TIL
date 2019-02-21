# 변경 가능성을 최소화하라

## 자바 라이브러리의 불변 클래스
- String
- 기본 타입의 박싱된 클래스
- BigInteger
- BigDecimal

## 불변 클래스 규칙
- 상태변경하는 메서드(변경자)를 제공하지 말것
- 클래스 확장 방지
    - final 클래스 사용
- 모든 필드 final 선언
    - 스레드 안전하기 위해서도 필요
- 모든 필드 private
    - public final로도 불변이 가능하나 향후 내부 표현을 바꾸기 위해 private 사용 (item15, item16)
- 자신만 내부 가변 컴포넌트 접근하도록 처리
    - 생성자, 접근자, readObject 메서드 (item88) 모두에서 방어적 복수 수행
