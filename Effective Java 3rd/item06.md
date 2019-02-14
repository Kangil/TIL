# 불필요한 객체 생성을 피하라

## 같은 기능의 객체를 매번 생성하기보다는 재사용이 바람직
- 불변객체(item17)는 언제든 재사용 가능
- 가변객체라도 중간에 변경되지 않는다면 재사용 가능
```java
// Bad : 실행시마다 새 인스턴스 생성
String s = new String("test"); 
// Good : 가상 머신 내 같은 문자열리터럴을 사용하는 코드가 모두 같은 객체 사용
String s = "test";
```

## 정적 팩터리 메서드(item01)를 제공된다면 생성자보다 이를 활용
```java
// Bad : Java 9 deprecated API
Boolean b = Boolean("true");
// Good
Boolean b = Boolean.valueOf("true");
```

## 생성 비용이 아주 비싼 객체는 캐싱하여 재사용
```java
// Bad
static boolean isRomanNumeral(String s) {
    return s.matches("^(?=.)M*(C[MD]|D?C{0,3})"
            + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
}
```
- String.matches 내부에서 만드는 Pattern 인스턴스의 생성 비용이 높음
    - Pattern은 정규에 해당하는 유한 상태 머신(finite state machine)을 만들기 때문
    - 한번 쓰고 버려져서 가비지 컬렉션 대상
- 성능 개선을 위해 불변 Pattern 인스턴스를 클래스 초기화 과정에서 생성해 캐싱하고 재사용

```java
// Good : 기존보다 6.5배 빠름
public class RomanNumerals {
    private static final Pattern ROMAN = Pattern.compile(
            "^(?=.)M*(C[MD]|D?C{0,3})"
            + "(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$");
    
    static boolean isRomanNumerals(String s) {
        return ROMAN.matcher(s).matches();
    }
}
```
- isRomanNumeral 처음 호출시 필드를 초기화하는 지연 초기화도 가능(item83)
    - 코드를 복잡하게 만들고 성능은 크게 개선되지 않을 때가 많아 비추천(item67)

## 오토박싱을 피하는것이 바람직
- 오토박싱은 성능적으로 손해 (item61)

```java
private static long sum() {
    Long sum = 0L;
    for (long i = 0; i <= Integer.MAX_VALUE; i++)
        sum += i; // 오토박싱이 일어나는 부분

    return sum;
}
```
- 위 코드에서 sum을 Long에서 long으로만 변경하여도 성능이 10배 빨라짐

## 기타
- 객체 생성 비용이 비싸니 이를 피하자는 이야기가 아님
    - JVM에서 객체 생성 회수는 큰 부담X
    - 일반적으로 객체 생성은 프로그램의 명확성, 간결성, 기능을 위해서는 권장
- 데이터베이스 연결풀은 권장하지만 자체 객체 풀은 만들지 말것
    - 잘만들기 힘들며 최근 JVM 가비지 컬렉터 최적화로 보통 직접 만들 객체 풀보다 가벼운 객체 다루는게 훨씬 빠름
- 방어적인 복사가 필요한 상황에서는 재사용하지 말것
    - 이는 버그와 보안 구멍으로 연결됨
    - 불필요한 객체 생성은 단지 코드 형태와 성능에만 영향
