# 인스턴스화를 막으려거든 private 생성자를 사용하라

## 정적 메서드와 정적 필드만을 가진 클래스
- 기본 타입 값이나 배열 관련 메서드를 모아두는 용도
    - java.lang.Math
    - java.util.Arrays
- 특정 인터페이스를 구현하는 객체 생성해주는 정적 메서드를 모아두는 용도
    - java.util.Collections (java 8부터는 인터페이스에 해당 메서드 선언 가능)
- final 클래스 관련한 메서드를 모아두는 용도
    - 상속해서 하위 클래스에 넣는게 불가능 하기 때문

### 인스턴스화 막기
- 인스턴스화가 불필요하므로 막을 필요가 있음
- 생성자를 생략할 경우 컴파일러가 자동으로 public 생성자 생성하므로 주의
- 추상 클래스로 만드는 것으로는 방지 불가
    - 상속해서 쓰라는 뜻으로 오해할 가능성 존재 (item19)
- private 생성자를 추가하여 인스턴스화 방지

```java
public class UtilityClass {
    // 기본 생성자가 만들어지는 것을 막는다(인스턴스화 방지용).
    private UtilityClass() {
        throw new AssertionError();
    }
    ...
}
```
- 생성자가 존재하지만 호출할 수 없으니 명확하게 주석 달아두길 추천
- 해당 방법으로 상속도 막을 수 있음