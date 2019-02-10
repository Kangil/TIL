# private 생성자나 열거 타입으로 싱글턴임을 보증하라

## 싱글턴(Singleton)
- 인스턴스를 오직 하나만 생성할 수 있는 클래스
    - 무상태 객체
    - 설계상 유일해야하는 시스템 컴포넌트
- 사용하는 클라이언트가 테스트하기 어려움
    - 타입이 인터페이스이고 이를 구현한 싱글턴이 아니라면 mock 구현으로 대체할 수 없기 때문

## 싱글턴 만드는 방식
- 생성자를 private으로 감춤
- 인스턴스에 접근할 수 있는 유일한 수단으로 public static 멤버를 마련

### public static final 필드 방식 싱글턴
```java
public class Elvis {
    public static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }

    public void leaveTheBuilding() { ... }
}
```
- 간결하고 싱글턴임이 API에서 명백히 드러남
- 정적 팩터리 방식의 장점이 필요없다면 이방식으로 사용

### 정적 팩터리 방식 싱글턴
```java
pubilc class Elivs {
    private static final Elvis INSTANCE = new Elvis();
    private Elvis() { ... }
    public static Elvis getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { ... }
}
```
- 추후 API 수정없이 싱글턴이 아닌 다른 방식으로도 변경 가능
- 정적 팩터리를 제네릭 싱글턴 팩터리로 만들 수 있음 (item30)
- 팩터리 메서드 참조를 공급자(supplier)로 사용 가능 (item43, item44)
    - Elvis::getInstance를 Supplier<Elvis>로 사용하는 식

### 위 두가지 방식 싱글턴의 문제점
- 직렬화
    - Serializable 구현만으로는 부족
    - 모든 인스턴스 필드를 일시적(transient)이라고 선언하고 readResolve 메서드 제공 필요 (item89)
    - 위의 방식이 아닐 경우 역직렬화할 때마다 새로운 인스턴스 생성됨
    ```java
    // 싱글턴임을 보장해주는 readResolve 메서드
    private Object readResolve() {
        // '진짜' Elvis를 반환하고, 가짜 Elvis는 가비지 컬렉터에 맡긴다.
        return INSTANCE;
    }
    ```
- 권한이 있는 클라이언트가 리플렉션 API(item65)인 AccessibleObject.setAccessible을 사용해 private 생성자 호출 가능
    - 이를 방어하기 위해서는 생성자를 수정하여 두번째 객체 생성시에 예외 던지도록 처리


### 원소 하나인 열거 타입을 선언하는 방식 싱글턴
```java
public enum Elvis {
    INSTANCE;

    public void leaveTheBuilding() { ... }
}
```
- public 필드 방식과 비슷하지만 더 간결
- 추가작업없이 직렬화 가능
- 리플렉션 공격에서도 문제없음
- 대부분 상황에서 가장 좋은 방법
    - 만들려는 싱글턴이 Enum 외의 클래스를 상속해야한다면 사용 불가

