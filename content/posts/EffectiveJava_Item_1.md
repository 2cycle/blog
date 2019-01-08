---
title: "Effective JAVA 정리 chapter / item 1 "
date: 2019-01-08
Categories: ["effectivejava"]
Tags: ["java", "item1", "effectivejava", "study"]
---

## Effective Java 3/e  Chapter 2.

### Item 1. 생성자 대신 정적 팩터리 메서드를 고려하라

#### 장점

- 이름을 가질 수 있다. 

  + 생성자에 넘기는 매개변수와 생성자 자체만으로는 반환된 객체의 특성을 제대로 설명하지 못한다.

  + 정적 팩터리는 이름만 잘 지으면 반환될 객체의 특성을 쉽게 표시할 수 있다.

  ``` Ex 
  BinInteger(int , int , Random) vs BigInteger.probablePrime( int, Random)
  ```

- 호출될 때마다 인스턴스를 새로 생성하지 않아도 된다. 

  + 불변 클래스는 인스턴스를 미리 만들어 놓거나 새로 생성한 인스턴스를 캐싱하여 재활용하는 식으로 불필요한 객체 생성을 피할 수 있다.

  ``` Ex 
  Boolean.valueOf(boolean)
  ```

- 반환 타입의 하위 타입 객체를 반환할 수 있는 능력이 있다.

  + 자바 8 이전에는 interface 에 companion class(인스턴스화 불가 동반 클레스)가 필요하였다. (Ex Collection)

  + 자바 8부터는 interface 내에 정적 메서드를 가질 수 있어 default method 활용 가능

- 입력 매개변수에 따라 매번 다른 클래스의 객체를 반환할 수 있다.

   ``` Ex
   EnumSet 클래스는 원소(매개변수) 갯수에 따라
   64개 이하 - RegularEnumSet 인스턴스(long변수 하나로 관리),
   65개 이상 - JumboEnumSet의 인스턴스(long배열로 관리) 를 반환한다.
   ```

- 정적 팩터리 메서드를 작성하는 시점에는 반환할 객체의 클래스가 존재하지 않아도 된다.

  - 서비스 제공자 프레임워크(service provider framework)를 만드는 근간 (ex JDBC)

  ``` Ex
  서비스 제공자 프레임워크에서 provider는 서비스 구현체. 이 구현체들을 따라 클라이언트에서 제공하는 역할을 프레임워크가 통제하야, 클라이언트를 구현체로부터 분리
  ```

#### 단점

- 상속을 하려면 public이나 protected 생성자가 필요하니 정적 팩터리 메서드만 제공하면 하위 클래스를 만들 수 없다.
  - 컴포지션을 사용하도록 유도
  - 생성자의 경우 보통 private으로 만듬
- 정적 팩터리 메서드는 프로그래머가 찾기 어렵다.
  - 자바독이 알아서 처리할 수 없으므로 API정리를 잘 해놔야하고 규약에 따라 메서드 이름을 정리
    - from
    - of
    - valueOf
    - instance
    - create
    - getType
    - newType
    - type



생성자를 사용시에 무분별하게 public 생성자를 사용하였는데, 이 책의 시작이었던 Item1을 읽으면서 조금 부끄러웠다.

Item 90까지 읽으면 부끄럽지 않은 코딩을 할 수 있을까.

