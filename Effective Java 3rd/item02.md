# 생성자에 매개변수가 많다면 빌더를 고려하라


## 객체 생성 시 선택적 매개변수가 많은 경우
- 매개변수가 적어도 4개 이상 되는 경우
- 정적 팩터리(item01), 생성자 모두 대응이 어려움

### 방법 1: 점층적 생성자 패턴 (Telescoping constructor pattern)
- 필수 매개변수만 받는 생성자, 필수 매개변수와 선택 파라미터 1개 받는 생성자.. 식으로 작성
- 원하는 매개변수를 모두 포함하는 생성자 중 가장 짧은 것을 골라 호출
#### 단점
- 매개변수가 많아질수록 클라이언트 코드 작성과 읽기 어려움

### 방법 2 : 자바빈즈 패턴 (JavaBeans pattern)
- 매개변수 없는 생성자로 객체 생성 후 setter 메서드로 값을 설정하는 방식
- 점층적 생성자 패턴에 비해 코드 읽기 쉬움
#### 단점
- 객체 하나 생성을 위해 메서드 여러개의 호출이 필요
- 객체가 완전히 생성하기 전까지 일관성이 무너진 상태
    - 매개변수 유효 여부를 생성자 확인만으로 판단 불가
    - 버그 심은 코드와 런타임시 문제되는 코드가 물리적으로 떨어져있어 디버깅 어려움

### 방법 3 : 빌더 패턴 (Builder pattern)
- 파이썬, 스칼라에 있는 명명된 선택적 매개변수(Named optional parameters)를 흉내


```java
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static class Builder {
        // 필수 매개변수
        private final int servingSize;
        private final int servings;

        // 선택 매개변수 - 기본값으로 초기화한다.
        private final int calories      = 0;
        private final int fat           = 0;
        private final int sodium        = 0;
        private final int carbohydrate  = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int val) {
            calories = val;
            return this;
        }
        public Builder fat(int val) {
            fat = val;
            return this;
        }
        public Builder sodium(int val) {
            sodium = val;
            return this;
        }
        public Builder carbohydrate(int val) {
            carbohydrate = val;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize  = builder.servingSize;
        servings     = builder.servings;
        calories     = builder.calories;
        fat          = builder.fat;
        sudium       = builder.sodium;
        carbohydrate = builder.carbohydrate; 
    }
}

NutritionFacts cocaCola = new NutritionFacts.Builder(240, 8)
        .calories(100).sodium(35).carbohydrate(27).build();
```

#### 단점
- 객체 생성을 위해 빌더부터 만들어야하므로 성능에 민감한 상황에서는 문제가 될 수 있음
- 점층적 생성자 패턴보다는 코드가 장황하여 매개변수 4개 이상은 되어야 값어치함