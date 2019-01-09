---
title: "EffectiveJava_Item_5"
date: 2019-01-09
Categories : ["effectivejava"]
Tags : ["java","effectivejava","item5"]
---



## Effective Java 3/e Chapter 2.

### Item 5. 자원을 직접 명시하지 말고 의존 객체 주입을 사용하라



- 인스턴스를 생성할 때 생성자에게 필요한 자원을 넘겨주는 방식
  - 불변을 보장하여 여러 클라이언트가 공유하여 사용 가능
  - 의존 객체 주입은 생성자, 정적 팩터리, 빌더 모두 응용 가능
- 생성자에 자원 팩터리를 넘겨주는 방식
  - 팩터리 메서드 패턴 (Factory Method pattern 구현)
  - Java8 Supplier<T>

```java
Mosaic.java

import java.util.function.Supplier;
import java.util.stream.IntStream;

@Slf4j
public class Mosaic {
    private final Tile tile;

    private Mosaic(Tile tile) {
        this.tile = tile;
    }

    public void print() {
        IntStream.range(1,10)
                .forEach( i -> {
                    IntStream.range(1,i).forEach(i2->System.out.print(tile.tile()));
                    System.out.println();
                } );
    }

    public static Mosaic create(Supplier<? extends Tile> tileFactory ) {
        return new Mosaic(tileFactory.get());
    }
}
```

``` java
Tile.java

public class Tile {
    private final String tile;
    public Tile(String tile) {
        this.tile = tile;
    }
    public String tile() {
        return tile;
    }
}
```

``` java
Item05.java

import java.util.function.Supplier;

public class Item05 {
    public static void main(String[] args) {
        Mosaic aMosaic1 = Mosaic.create(new Supplier<Tile>() {
            @Override
            public Tile get() {
                return new Tile("a");
            }
        });
        Mosaic aMosaic = Mosaic.create(() -> new Tile("a"));
        aMosaic.print();

        Mosaic bMosaic = Mosaic.create(() -> new Tile("b"));
        bMosaic.print();
    }
}
```



의존 객체 주입은 유연성, 재사용성, 테스트 용이성을 개선해준다.



예제코드 출처 : https://github.com/bnpdukim/effective-java-3e/blob/master/src/main/java/study/effective/ch02/item05/Tile.java



 