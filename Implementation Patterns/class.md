# 클래스
- 관련성 있는 데이터와 로직을 함께 사용하기 위한 용도 (응집도를 올리기 위한?)


# 단순한 상위클래스 이름
- 명확한 의미 전달이 중요하며 가급적 짧게 한 단어로
- 몇개의 단어보다 은유가 좋을 때도 존재 (ex. 드로잉 프로그램의 Figure)


# 한정적 하위클래스 이름
- 간결성보다는 표현성을 선택
- 보통은 상위 클래스 이름 + 수식어를 붙이나 예외도 존재


# 추상 인터페이스
- 구현보다는 인터페이스에 맞춰 코딩
- 구현을 별도로 분리


# 인터페이스
- 인터페이스 사용시 구현의 변경은 쉽지만 인터페이스 자체 변경은 어려움
- 구상 클래스 이름의 간결함을 위해 보통 I를 붙여서 이름을 지음 (ex. IFile)


# 추상 클래스
- 인터페이스는 수정에 취약
    - 기존 설계 향상이 필요하면 버전 인터페이스 사용
- 추상 클래스의 경우 새로운 연산 추가 가능
- 인터페이스와 추상 클래스를 동시 사용도 가능


# 버전 인터페이스
- 인터페이스 변경이 필요한 경우 해당 인터페이스를 상속받아 새로운 인터페이스 생성
- 새로운 동작은 instanceof 연산자로 확인하고 downcast 하여 사용
- 대체 인터페이스가 늘어남은 설계 변경이 필요함 시점을 나타냄

```Java
interface Command {
    void run();
}

interface ReversibleCommand extends Command {
    void undo();
}

Command recent = ...;
if (recent instanceof ReversibleCommand) {
    ReversibleCommand downcasted = (ReversibleCommand) recent;
    downcasted.undo();
}
```


# 값 객체
- 프로그래밍의 두 가지 스타일
    - 프로시저 인터페이스 (객체의 상태 조작)
        - 호출 순서가 중요할수 있으며 묵시적 인터페이스 변경에 취약
    - 수학적 표현법 (함수형 스타일)
        - 상태 변경이 아닌 새로운 값 생성 (값 객체)
        - 호출 순서에 영향받지 않음
- 값객체의 경우 생성자에서만 값을 설정, 새로운 객체를 반환하며 반환된 객체의 저장은 연산을 요청한 쪽에

```Java
bounds.translateBy(10, 20); // 변경가능한 Rectangle 객체
bounds = bounds.translateBy(10, 20); // 값 스타일 Rectangle 객체
```

# 특화
- 연산 간의 유사점과 차이점을 부각시키는 방향으로 코드를 짜면 유지보수에 용이

# 하위클래스
- 상속은 공통된 구현을 갖는 경우 사용하며 분류를 위해서는 사용X
- 상위클래스의 각 메소드는 잘게 쪼개서 개별로 오버라이드 가능하게 구현
    - 상위클래스 메소드가 큰 경우 복사하여 수정하면되나 이 경우 암묵적 의존관계
- 하위클래스, 조건문, 위임을 각 경우에 맞게 사용
    - 변화하는 로직을 표현하기위해서는 하위클래스보다 조건문, 위임 사용

# 구현자
- 객체지향적 프로그램은 선택을 표현 하기 위해 다형적 메시지 사용
- 다형적 메세지는 여러 가지 변형 수용 가능
- 객체와 메세지를 통해 의도(데이터 전송)와 구현을 분리하여 프로그래머의 의도를 명확히 표현
- 이 방식으로 미래에 발생할 확장을 수용할 수 있음

# 내부 클래스
- 낮은 비용으로 클래스 사용의 이점을 가질수 있는 방법
- 감싼 클래스(enclosing class)의 데이터에 접근 가능

```java
public class InnerClassExample {
    private String field;

    public class Inner {
        public String example() {
            return field; // 감싼 클래스 인스턴스의 필드 사용
        }
    }

    @Test public void passed() {
        field = "abc";
        Inner bar = new Inner();
        assertEquals("abc", bar.example());
    }
}
```

- 내부 클래스는 인자없는 생성자 가질 수 없음 (???)
    - 리플렉션으로 인스턴스 만드는 경우 문제점 내포
- 생성 클래스의 인스턴스와 분리된 내부 클래스는를 쓰려면 static 으로 선언해 사용

# 인스턴스별 행위
- 클래스의 인스턴스는 모두 같은 로직을 공유하나 다르게도 가능
- 연산 도중 로직이 변화하는 경우 코드 이해하기 어려워짐
    - 데이터 흐름을 추적해야할 필요가 있으므로
    - 인스턴스 생성 후에는 되도록 인스턴스 행동을 변화시키지 않는 편이 좋음

# 조건문
- 모든 로직이 한 클래스 안에 있어 읽기 편리
- 분기가 많을수록 안정성이 떨어지고 버그 가능성도 증가
- 조건부 로직이 중복되거나 분기문의 결과에 따라 로직이 달라질 경우
    - 하위클래스나 위임을 사용하여 메시지로 변경

# 위임
- 프로그램 수행 도중 동작 변경이 필요한 경우 사용
- 로직이 각 클래스에 분산되어 코드읽기 어려움
- 인스턴스별 행위 지원 이외에 코드 공유에도 사용 가능
- 위임자 클래스(delegator)를 위임 메소드(delegated method)에 인자로 넘겨주는 방식으로 응용 가능
    - #1 처럼 위임 메소드에 위임자 클래스를 전달하면 여러종류의 editor클래스에서 이 메소드 사용 가능
    - #1과 같이 유연성이 필요하지 않은 경우 #2 처럼 필드를 통해 참조하는 편이 간편

```java
// #1
GraphicEditor
public void mouseDown() {
    tool.mouseDown(this);
}

RectagleTool
public void mouseDown(GraphicEditor editor) {
    editor.add(new RectangleFigure());
}
```

```java
// #2
GraphicEditor
public void mouseDown() {
    tool.mouseDown();
}

RectangleTool
private GraphicEditor editor;
public RectangleTool(GraphicEditor editor) {
    this.editor = editor;
}

public void mouseDown() {
    editor.add(new RectangleFigure());
}
```

# 플러그인 선택자
- 한 두개의 메소드에서만 인스턴스별 행동이 필요하고 모든 로직이 한개 클래스에 있어도 되는 경우 메소드 이름을 필드에 저장해두고 리플렉션을 통해 호출
    - 기법을 알고 있는 사람만 이해 가능
    - 사용에 따르는 비용이 크므로 일부 문제에 한해서만 제한적으로 사용!

```java
// junit
String name;
public void runTest() throws Exception {
    class[] noArguments = new Class[0];
    Method method = getClass().getMethod(name, noArguments);
    method.invloke(this, new Object([0]));
}
```

# 익명 내부 클래스 
- 한 곳에서만 사용되는 클래스를 생성하여 일부 메소드 오버라이드후 지역적으로 사용
    - API가 매우 간단한 경우 (run()만 가지고 있는 Runnable 인터페이스)
    - 상위클래스가 대부분 구현을 담당하여 내부 클래스 구현이 쉬운 경우
- 가독성을 위해 내부 클래스는 최대한 짧게
- 별도 테스트 어려워서 복잡한 로직에 적합하지 않으며 이름이 없어 이를 통해 의도 전달 불가

# 라이브러리 클래스
- 어떤 객체에도 적합하지 않은 기능을 모아둔 클래스
- 인스턴스가 불가하게 정적 메소드로 구현한 클래스
- 로직이 많이질 경우 객체로 변환이 필요