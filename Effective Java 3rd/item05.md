# 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라

## 사용하는 자원에 따라 동작이 달라지는 클래스
- 여러 자원 인스턴스를 지원해야하며 클라이언트가 원하는 자원을 사용해야 하는 경우
- 정적 유틸리티 클래스(item04), 싱글턴 클래스 (item03) 부적합
    - 여러 자원 사용 불가
    - 자원 교체하는 메서드를 추가할 수 있으나 오류 발생 쉬우며 멀티스레드 환경에서 사용 불가
- 인스턴스 생성시에 필요한 자원을 넘겨주는 방식이 필요

### 의존 객체 주입 방식
```java
public class SpellChecker {
    private final Lexicon dictionary;

    public SpellChecker(Lexicon dictionary) {
        this.dictionary = Objects.requireNonNull(dictionary);
    }

    public boolean isValid(String word) { ... }
    public List<String> suggestions(String typo) { ... }
}
```
- 불변(item17)을 보장하여 여러 클라이언트가 공유 가능
- 생성자, 정적 팩터리(item01), 빌더(item02) 모두에 똑같이 응용 가능
- 유연성과 테스트 용이성을 높여주나 큰 프로젝트는 코드가 복잡해짐
    - 의존 객체 주입 프레임워크를 이용하여 해소 (ex. Spring)

### 의존 객체 주입 방식 변형
- 생성자에 자원 팩터리를 넘겨주는 방식
    - 팩터리란 호출시 특정 타입 인스턴스를 반복해 만들어주는 객체 (팩터리 메서드 패턴의 구현)
- Supplier<T> 인터페이스를 입력으로 받는 경우 한정적 와일드카드 타입 (bounded wildcard type, item31)을 사용해 타입 매개변수 제한이 필요
```java
Mosaic create(Supplier<? extends Tile> tileFactory) { ... }
```
