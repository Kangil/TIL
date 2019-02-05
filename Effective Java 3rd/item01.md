# 생성자 대신 정적 팩터리 메서드를 고려하라

```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```
- 디자인 패턴의 Factory Method와는 다름

## 장점
1. 이름을 가질 수 있어 좀더 명확
2. 호출될 때마다 인스턴스를 새로 생성하지 않아도 됨
    - 불변 클래스는 미리 만들거나 재활용 가능
    - 생성 비용이 큰 경우 성능상 이점
3. 반환 타입의 하위 타입 객체를 반환 가능
    - 인터페이스를 반환 타입으로 사용하여 유연성 확보
    - 인터페이스 기반 프레임워크의 핵심기술
4. 입력 매개변수에 따라 다른 클래스 객체 반환 가능
    - ex. 원소 수에 따라 다르게 반환되는 EnumSet
5. 작성 시점에서 반환할 객체의 클래스가 없어도 가능
    - ex. JDBC

## 단점
1. 정적 팩터리 메서드만 제공하면 하위 클래스 생성 불가
    - private 생성자를 사용하는 경우
    - 상속보다는 컴포지션을 사용(item18)하고, 불변 타입(item17)으로 만들려면 되려 장점
2. 찾기 어려움
    - 생성자처럼 API문서상에서 명확하지 않으므로 널리 알려진 규약을 따라 짓는 식으로 완화

## 널리 알려진 규약
- from : 매개변수 하나 받아 해당 타입 인스턴스를 반환하는 형변환 메서드
```java
Date d = Date.from(instant);
```

- of : 여러 매개변수 받아 적합한 타입 인스턴스를 반환하는 집계 메서드
```java
Set<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING);
```

- valueOf : from, of의 자세한 버전
```java
BigInteger prime = BigInteger.valueOf(Integer.MAX_VALUE);
```

- instance 혹은 getInstance: 매개변수로 명시한 인스턴스를 반환, 같은 인스턴스임은 보장X
```java
StackWalker luke = StackWalker.getInstance(options);
```

- create 혹은 newInstance : instance 혹은 getInstance와 같지만, 매번 새로운 인스턴스 반환을 보장
```java
Object newArray = Array.newInstance(classObject, arrayLen);
```

- getType : getInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 정의할때 사용
```java
FileStore fs = files.getFileStore(path);
```

- newType : newInstance와 같으나 생성할 클래스가 아닌 다른 클래스에 정의할때 사용
```java
BufferedReader br = Files.newBufferedReader(path);
```

- type : getType, newType 의 간결버전
```java
List<Complaint> litany = Collections.list(legacyLitany);
```