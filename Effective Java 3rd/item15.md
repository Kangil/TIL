# 클래스와 멤버의 접근 권한을 최소화하라

## 정보 은닉(캡슐화의 이점)
- 컴포넌트 병렬 개발로 개발 속도를 높임
- 컴포넌트 별로 파악, 디버깅가능하여 교체 부담도 적어 관리비용을 낮춤
- 성능 최적화에 도움 (다른 컴포넌트에 영향없이 특정 컴포넌트만 최적화 가능)
- 독자적으로 동작할 수 있는 컴포넌트는 재사용성을 높임
- 개별 컴포넌트 동작을 검증할 수 있어 큰 시스템 제작 난이도를 낮춤

## 정보 은닉의 핵심
- 접근 제한자를 제대로 활용
- 모든 클래스와 멤버의 접근성은 가능한한 좁게 설정
- 패키지 외부에서 사용할 일이 없다면 package-private으로 선언
- 한 클래스에서만 사용하는 package-private 톱레벨 클래스나 인터페이스는 사용하는 클래스 안의 private static으로 중첩하여 사용 (item24)

## 멤버(필드, 메서드, 중첩 클래스, 중첩 인터페이스)에 부여할 수 있는 접근 수준
- 공개 API를 설계한 후, 그 외의 모든 멤버는 private으로 처리
- 오직 같은 패키지의 다른 클래스가 접근해야하는 경우에 한해 package-private으로 설정
- 권한을 풀어주는 일을 자주 하게된다면 컴포넌트를 더 분리
- Serialize 구현한 클래스는 공개 API가 될 수 있음 (item86, item87) 

### private
- 멤버를 선언한 톱레벨 클래스에서만 접근 가능

### package-private
- 멤버가 소속된 패키지 안의 모든 클래스에서 접근 가능
- 접근 제한자를 명시하지 않은 경우 적용되는 수준 (단, 인터페이스는 public)

### protected
- package-private 접근 범위를 포함, 하위 클래스에서도 접근 가능

### public
- 모든 곳에서 접근 가능

## 테스트 목적으로 접근 범위를 넓히는 경우
- pakcage-private정도 까지는 괜찮지만 그 이상은 공개 API가 되므로 주의
- 테스트코드를 같은 패키지안에 두는 방식으로 해결


## public 클래스의 설계
- public 클래스의 인스턴스 필드는 되도록 public 이 아니도록 설계 (item16)
    - final이 아닌 필드를 public으로 선언하면 해당 필드 관련 항목은 불변식으로 보장X
    - 이 경우 필드 수정시 다른 작업을 할 수 없으므로 스레드 안전하지 않음
- 유일한 예외는 public static final 필드를 상수로 쓰는 경우
    - 관례상 대문자에 단어간 _ 문자로 구분 (item68)
    - 반드시 기본 타입 값이나 불변 객체를 참조해야함 (item17)
- public static final 배열필드나 접근자 제공해서는 안됨
    - 길이가 0아 아닌 배열은 모두 변경 가능하므로

```java
// 보안 허점 존재
public static final Thing[] VALUES = { ... };

// 해결책1 - public 불변리스트
private static final Thing[] PRIVATE_VALUES = { ... };
public static ifnal List<Thing> VALUES =
    Collections.unmodifiableList(Arrays.as(PRIVATE_VALUES));

// 해결책2 - 방어적 복사
private static final Thing[] PRIVATE_VALUES = { ... };
public static final Thing[] values() {
    return PRIVATE_VALUES.clone();
}
```

## 자바9 모듈 시스템
- 모듈은 패키지의 모음으로 protected, public 제한자에 영향을 미침
- 사용이 쉽지 않으므로 꼭 필요한게 아니라면 나중을 기약