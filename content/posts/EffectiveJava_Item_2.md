---
title: "EffectiveJava_Item_2"
date: 2019-01-09
Categories : ["effectivejava"]
Tags : ["java","effectivejava","item3"]
---



## Effective Java 3/e Chapter 2.

### Item 2. 생성자 매개변수가 많다면 빌드를 고려하라



- 점층적 생성자 패턴 (telescoping constructor pattern)
  - 필수 매개변수 생성자 + 선택 매개변수의 갯수에 맞는 생성자를 늘려가는 방식
  - 매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다.

``` Ex
public class NutritionFacts {
    priavte fianl int test1;
    priavte fianl int test2;
    priavte fianl int test3;
    priavte fianl int test4;
    
    public NutritionFacts(int test1, int test2){
        this(test1,test2,0);
    }
    public NutritionFacts(int test1, int test2, int test3){
        this(test1, test2, test3, 0);
    }
    
    ...
    
}

```



- 자바빈즈 패턴 (JavaBeans pattern)
  - 매개변수가 없는 생성자로 객체를 만든 후, 세터(setter) 메서드를 호출하여 값 설정
  - 객체가 완성되기 전까지 일관성이 무너진 상태에 놓임
  - 클래스를 불면으로 만들 수 없음

``` Ex
NutritionFacts cocaCola = nw NutritionFacts();
cocaCola.setTest1(1);
cocaCola.setTest2(2);
cocaCola.setTest3(3);
cocaCola.setTest4(4);
```



- 빌더 패턴 (Builder pattern)
  - 필수 매개변수만으로 생성자를 호출해 빌더 객체를 얻고, 빌더 객체가 제공하는 세터 메서드로 선택 매개변수들 설정
  - 쓰기 쉽고, 읽기 쉽다.
  - 계층적으로 설계된 클래스와 함께 쓰기에 좋다.

``` Nu
public class NutritionFacts {
    //필수
    private final int servingSize;
    private final int servings;
    //옵션
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    private NutritionFacts(Builder builder) {
        this.servingSize = builder.servingSize;
        this.servings = builder.servings;
        this.calories = builder.calories;
        this.fat = builder.fat;
        this.sodium = builder.sodium;
        this.carbohydrate = builder.carbohydrate;
    }

    public static class Builder {
        private final int servingSize;
        private final int servings;

        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int calories) {
            this.calories = calories;
            return this;
        }

        public Builder fat(int fat) {
            this.fat = fat;
            return this;
        }


        public Builder sodium(int sodium) {
            this.sodium = sodium;
            return this;
        }

        public Builder carbohydrate(int carbohydrate) {
            this.carbohydrate = carbohydrate;
            return this;
        }


        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

}
```



``` Basic.java
public class Item02Basic {
    public static void main(String[] args) {
        NutritionFacts cocaCola = new NutritionFacts.Builder(240,8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27)
                .build();
        log.info("cocaCola : {}", cocaCola);

        NutritionFacts2 cocaCola2 = NutritionFacts2.builder()
                .servingSize(240)
                .servings(8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27)
                .build();
        log.info("cocaCola2 : {}", cocaCola2);
    }
}
```



예제 소스 출처 : https://github.com/bnpdukim/effective-java-3e/tree/master/src/main/java/study/effective/ch02/item02/basic





