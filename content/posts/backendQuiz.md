---
title: "BackendQuiz"
date: 2019-12-25T01:07:16+09:00
Categories: ["Quiz","Backend"]
Tags: []

---

# 면접 대비 10가지 질문 (1 ~ 10)

- <span style="color:red">수정버전</span>

### 1)  JVM 에 대해서 설명해줄 수 있는가?

<span style="color:red">수정답변</span> : JVM은 클래스 로더, 가비지 컬렉터, 실행 엔진으로 구성되어 있으며 Runtime Data Areas의 메모리과 함께하며 java 프로그램을 구동합니다.
JVM 관련 프로그래밍을 해본 적은 없지만 자바 성능 테스트를 위해 잠깐 써봤던 Scouter 프로그램에서 힙 메모리 사용량이나 CPU사용량등을 보며 구조에 대해 간단히 학습했던 경험이 있습니다.

----

먼저 JRE는 Java API와  JVM으로 구성되어 있으며 JVM은 Java Virtual Machine으로 Java byte code를 실행하는 주체이다.

(Byte code를 기계어로 바꿔 실행)

JVM은 크게 4가지로 나뉠 수 있다. (Class Loader, Execution Engine, Garbage Collector, Runtime Data Area)

<img src="https://t1.daumcdn.net/cfile/tistory/9967B7495C6165F715" width="60%">



1. <span style="color:red">**Class Loader**</span>
   Java 컴파일러가 Source Code(.java)를 빌드하면 Byte code(.class) 파일이 생성된다. 각 운영체제의 JVM의 Class loader가 이 바이트 코드를 <span style="color:Orange">Runtime Data Areas</span>에 로딩하여 프로그램을 구동한다.
   Class Loader의 로딩은 <span style="color:Orange">Runtime</span>에 일어나는데 클래스에 처음 접근할 때 일어난다. 이를 이용해 Lazy Loading singleton이 구현되기도 한다.(클래스 접근시가 아니라 getInstance시 Class Loading이 발생하므로 초기화를 늦추고 멀티쓰레드 환경에 안전한 동기화 이슈를 JVM에서 해결할 수 있음) <span style="color:Orange">Class Loading은 Thread safe하다.</span>

2. <span style="color:red">**Execution Engine**</span>
   Byte code를 실행할 때 기계어로 변경해 명령어 단위로 실행, 1Byte의 명령어(OpCode)와 피연산자(OpeRand)로 구분된다.

   Execution Engine은 2 가지 방식으로 명령어를 수행한다.

   - Interpreter : 명령을 순차적으로 읽으며 실행한다. 파일 이름을 인자로 받고 main함수를 포함한다.
   - JIT (Just In Time) Compiler : Interpreter의 느린 단점을 보완하고자 나온 컴파일러이다. 유사한 Byte code를 매번 재컴파일 하지 않고, 캐싱해둔 후 컴파일 총 시간을 단축한다.
     - Hotspot Compiler(Oracle) : 프로파일링을 통해 Hotspot(높은 우선순위)을 찾아 네이티브 코드로 컴파일 한다. 컴파일된 메소드의 바이트 코드가 더이상 필요 없을 경우 Hotspot virtual Machine은 캐시에서 네이티브 코드를 삭제하고 Interpreter모드로 실행한다.

3. <span style="color:red">**Garbage Collector**</span>
   Heap memory 영역에 생성된 객체들 중 <span style="color:orange">참조되지 않은 객체를 탐색 후 제거</span>하는 역할을 한다. 

4. <span style="color:red">**Runtime Data Areas**</span>
   OS로 부터 할당받은 JVM의 메모리 영역, 자바 어플리케이션 실행에 필요한 데이터가 담겨있다. 총 5개의 영역으로 나뉜다.

   - **Method Area** : Class Loader가 적재한 클래스(또는 인터페이스)에 대한 메타데이터 정보가 저장된다.  Non-heap영역으로 Permenant영역에 저장된다. JVM 옵션 중 PermSize (Permanent Generation의 크기)를 지정할 때 고려해야 할 요소이다. 5가지 정보를 저장한다.
     1) <span style="color:Blue">Type Information</span> : Interface 여부, 패키지 명을 포함한 Type이름, Type 접근제어자 ,연관된 Interface 리스트
     2) <span style="color:Blue">Runtime Constant Pool</span> : Type, Field, Method로의 모든 레퍼런스 저장, JVM은 Runtime Constant Pool을 통해 실제 메모리 상의 주소를 찾아 참조한다.
     3) <span style="color:Blue">Field Information</span> : Field Type, Field 접근 제어자
     4) <span style="color:Blue">Method Information</span> : Constructor를 포함한 모든 메타데이터를 저장, Method의 이름, 파라미터 수와 Type, Return Type, 접근 제어자 등
     5) <span style="color:Blue">Class Variable</span> : static 키워드로 선언된 변수, 기본형이 아닌 static변수의 실제 인스턴스는 heap 에 저장

   - **Heap Area** : new 연산자로 생성된 객체를 저장하는 공간이다. 참조 변수나 필드 미존재지 GC의 대상이 된다

   - **Stack Area** : Thread 마다 별개의 Frame으로 저장하며 아래 정보들을 저장한다.
     1) <span style="color:Blue">Loacl Variable Area</span> : 지역변수, 매개변수, 메소드를 호출한 주소 등 Method 수행중 발생한 임시 데이터를 저장한다. 4 Byte단위로 저장되며 int, float 등 4 byte 기본형은 1개의 셀(bool포함), double등의 8 byte 기본형은 두게의 셀을 차지한다.
     2) <span style="color:Blue">OpeRand Stack</span> : method의  workspace이다. 어떤 명령을 어떤 피연산자로 수행할지 나타낸다.
     3) <span style="color:Blue">Frame Data</span> : Constant Pool Resolution, Method return, Exception Dispatch 등을 포함한다. 또한 참조된 Exception테이블을 가지고 있어 Exception이 발생하면 JVM은 이 테이블을 참고하여 어떤 Exception을 처리할지 결정한다.

   - **PC Register** : Thread가 생성될 때마다 생성되는 영역(Program Counter)으로 Thread가 현재 실행하고 있는 주소와 명령을 저장한다. OS는 PC(Program Counter) 를 참고하여 스케쥴링시 해당 Thread의 다음 명령을 수행할수 있게 한다.

   - **Native Method Stack** : 자바 외 언어로 작성된 네이티브 코드를 저장하기 위한 영역 (주로 c/c++)

     

   <img src=https://t1.daumcdn.net/cfile/tistory/997E983C5C85F29110 width="70%">

   

   Thread생성을 기준으로 Heap과 Method는 모든 Thread가 공유하고 
   Stack, PC Register, Native Method Stack은 각각 Thread마다 생성되고 공유되지 않는다.

   > 참고
   > <https://jeong-pro.tistory.com/148> 
   >
   > <https://lazymankook.tistory.com/79>



### 2) Java 동기와 비동기/블로킹과 논블로킹

<span style="color:red">수정답변</span> : 동기 비동기는 요청과 결과가 동시에 발생하는 것인지, 동시에 발생하지 않는 것인지의 차이로 구분할 수 있습니다. 블로킹과 논 블로킹의 경우 제어권의 위치에 따라 구분되어 지는 것으로 이해하고 있습니다.

비동기 프로그래밍은 Kakao 비드 솔루션을 개발할 때 고려했던 개발방법 중 하나였습니다. 동시요청이 굉장히 많지만 DB block까지 고려했을 때 동기적 처리는 성능 및 사용성 이슈가 될 수 있다고 생각하여 AWS SQS를 도입하여 비동기 요청을 받고 순차 처리를 진행한 경험이 있습니다.

-----

**동기** : 요청과 결과가 동시애 발생하는 것으로 요청하면 시간이 얼마가 걸리든 요청한 자리에 결과가 나와야함

- 작업 처리 단위를 동시에 맞춤 (Transaction)

**비동기** : 요렁과 결과가 동시에 일어나지 않는다.

- 함수의 결과를 요청한 쪽에서 처리하지 않는 것

------

**블로킹** : 수행 결과가 끝날 때까지 제어권을 갖고 있는 것

**논블로킹** : 제어권을 반납



### 3) Java Singleton을 사용하는 이유

<span style="color:red">수정답변</span> : singleton 은 애플리케이션 시작 시 최초 한번 메모리에 할당된 인스턴스만 사용하는 방법을 의미합니다. Spring의 기반은 singleton을 기반으로 구현되고 있다는 것으로 싱글톤을 사용하고 있다고 인식하고 있습니다. 하지만 싱글톤을 직접 구현하여 개발한 경험은 대학시절 DB접속 인스턴스 관리를 위해 조금 사용해본 경험이 있지만 조금 부족하다고 생각합니다.

singleton이란 애플리케이션 시작될 때 최초 한번만 메모리를 할당하고 (static) 그 메모리에 인스턴스를 만들어 사용

- 메모리 낭비를 방지
- 데이터 공유 - 전역 인스턴스로 사용됨
- 인스턴스가 한 개임을 보증

멀티 쓰레드 환경에서 쓰레드 safe하게 사용하기 위해선 
Initialization on demand holder idiom (holder초기화) > 클래스 안에 클래스를 두어 JVM의 Class Loader  매커니즘과 Class가 로드되는 시점을 이용하는 방법을 사용 (일반적인 Singleton Pattern)



### 4) 데이터베이스에서 index구조

<span style="color:red">수정답변</span> : 인덱스는 키 컬럼순으로 정렬된 B+tree 구조로 되어있고 Range scan을 통해 값을 찾는 방식으로 동작합니다.
내부 레거시 솔루션의 데이터 로직은 프로시저를 통해 이뤄지고 있습니다. 새로운 보고서나 데이터 형식이 필요할 경우 프로시저 내에 많은 쿼리문들이 추가 되는데 그 때 쿼리 실행 계획을 보며 인덱스를 타고 있는지 또는 어떤 인덱스를 타야 하는지 등을 고려하여 개발한 경험이 있습니다.

----

인덱스는 키 컬럼순으로 정렬되어 있기때문에 범위 내애서 특정 값을 찾는 방식으로 작동(Range Scan)
인덱스는 B+tree로 트리구조를 이루고 있다. 
Root block, Branch block, Leaf block이 있고 B+tree는 **Left Blok의 깊이가 모두 동일하게 균형** 을 이루고 있다.
leaf간엔 양방향 링크가 걸려있어 내림차순, 오름차순 검색이 쉽고 Branch block을 기준으로 왼쪽 오른쪽을 찾아 검색한다.



### 5) SOLID법칙이란?

<span style="color:red">수정답변</span> : SOLID 방법에는 객체는 단일 책임을 가져야 한다는 단일 책임 원칙, 확장에 개방적이고 수정은 폐쇄적이어야 한다는 interface와 관련된 개방 폐쇄 원칙, 자식 클래스는 부모 클래스를 대체할 수 있다는 원칙인 리스코프 원칙, 인터페이스의 단일 책임 원칙을 중시하는 인터페이스 분리 원칙, 추상화 인터페이스 사용을 통해 변경을 용이하게 해야한다는 의존성 역전 법칙이 있습니다.

----



객체지향을 공부하며 모든 원칙에 대해 고민하고 지켜가려고 노력하고 있습니다. 특히, 일급 컬렉션을 활용해 개발하거나 인터페이스를 통해 의존성을 분리하고 중복 코드를 줄이는 것을 지키고 있습니다. 또한 상속을 사용하지 않음으로써 객체 분리와 단일 책임 원칙에 대해서 고려하여 개발해왔고 앞으로도 노력해나가겠습니다.

- **SRP(Single Responsible Principle) - 단일 책임 원칙**
  객체는 오직 하나의 책임만 가져야 한다. 클래스 목적을 명확히 함으로써 기능을 명확히 분리할 수 있다.
- **OCP(Open-Closed Principle) - 개방-폐쇄 원칙**
  객체는 확장에 대해서는 개방적이고 수정에 대해서는 폐쇄적이여야 한다는 원칙 (interface사용)
- **LSP(Liskov Substitution Principle) - 리스코프 치환 원칙**
  자식클래스는 부모클래스를 대체할 수 있다. is-a관계에 주의
- **ISP(Interface Segregation Principle) - 인터페이스 분리 원칙**
  인터페이스에 단일 책임 원칙을 주어 작게 나눈다. 
- **DIP(Dependency Inversion Principle) - 의존성 역전 원칙**
  고수준 클래스는 저수준 클래스에 의존해서는 안되며 고수준 인터페이스는 보통 저수준 클래스에서 추상화된 인터페이스만을 바라보게 구현해야함.(변경에 용이)



### 6) Java lambda란?

<span style="color:red">수정답변</span> : 함수를 변수처럼 사용할 수 있는 개념입니다.

람다를 사용하면서 굉장히 많은 장점을 얻었습니다. 가령 코드라인 수가 많이 줄었고, 스트림과 연동하여 파이프라인 처리시에도 의도가 정확한 코드를 손쉽게 작성할 수 있는 장점이 있었습니다. 

함수를 변수처럼 사용할 수 있는 개념
즉, 메서드를 하나의 expression으로 표현한 것. 메서드를 람다식으로 표현하면 이름과 반환값이 없어지므로 익명함수라고 함.

- 장점
  코드 라인 수가 줄어든다. 병렬적으로 코딩이 가능하다. 버그가 적은 코드를 작성할 수 있다.(의도가 명확해져서)
- 클로져가 지원되지 않는다. **



### 7) 직렬화란?

<span style="color:red">수정답변</span> : 직렬화란 객체나 데이터를 여러 환경에서도 잘 처리하기 위해  byte화 시키는 것이라고 할 수 있습니다. 

최근엔 springboot를 썼기 때문에 json 직렬화를 주로 사용하여 직접 serializable을 붙여 사용했던 경험은 잘 기억나지 않습니다. 하지만 채팅 프로그램에서 소켓통신시 특수값 처리나 데이터 처리를 위해서 직렬화를 사용해야 한다는 내용을 학부시절 그리고 책에서 본 기억이 있습니다.

객체들의 데이터를 연속적인 데이터로 변형하여 Stream에서 처리할 수 있도록 전송 가능한 형태로 만드는 것
직렬화가 되면 자동으로 SerializeUID(역직렬화에 사용)가 부여 되는데 다중 배포가 발생하고 객체가 계속 바뀌는 환경에선 유지되기(InvalidClassException) 힘드므로 UID를 직접 선언하여 사용.



### 8) 리플렉션이란?

<span style="color:red">수정답변</span> : 애플리케이션의 런타임 동작을 검사하거나 수정하는 것입니다.

DB에서 암호화된 데이터를 admin사이트에서 확인해야할 때, 복호화를 위해 리플렉션을 간단히 활용하여 복호화했던 경험이 있습니다. 데이터 출력시 복호화 로직에서 특정 필드의 데이터를 복호화하여 수정하는 로직을 리플렉션을 통해 간단히 구현하였습니다. 다만 보안적 이슈로 더이상 디비 내에서 암호화하는 방식을 사용하지 않아 재대로 운영까지 해보진 못하였습니다.

----

Java리플렉션의 경우 JVM에서 실행되는 애플리케이션의 런타임 동작을 검사하거나 수정하는 것입니다.  Class로 접근하여 사용.



### 9) Spring filter와 Interceptor차이

<span style="color:red">수정답변</span> : 필터와 인터셉터는 실행 시점이 좀 다릅니다. 핸들러로 가기전에 인터셉터가 처리되고 필터의 경우는 디스패쳐 서블릿 아판에서 정보를 처리합니다. 
프로젝트를 진행할 때 인증 처리시 크로스 도메인 처리를 위해 필터를 활용한 경험이 있습니다. 또한, 인터셉터의 경우 리다이렉트가 특정한 로직에 맞춰 처리되어야 할 때 enum과 함께 사용하여 페이지 리디렉션 로직을 처리했던 경험이 있습니다.

----

실행 시점이 다르다.
Filter는 Dispatcher Servlet 앞단에서 정보를 처리한다면 Interceptor는 selvlet에서 handelr(Controller)로 가기 전에 정보를 처리한다.
인증, 권한 등은 대게 Interceptor에서 처리하지만 보안, 인코딩 같은 전역적인 일은 filter에서 처리한다.



### 10) Garbage Collector를 설명

<span style="color:red">수정답변</span> :  GC를 통한 경험이 거의 없기에 활용 예제를 얘기할 수 있는 공부를 조금 더 해보도록 하겠습니다. 

-----

Garbage Collector는 Heap메모리에서 참조되지 않는 객체를 식별하여 메모리에서 삭제함.

Garbage Collector 순서

1. Marking 
   메모리중 어떤 부분이 사용되지 않고 있는지 체크하는 단계
2. Normal Deletion
   Marking으로 찾아낸 비참조 객체를 삭제하는 단계, 삭제 후 빈 공간은 Memory Alloctator가 참조하고 있어 빈 공간을 쉽게 찾아냄
3. Deletion with Compacting
   효율적인 메모리 관리를 위해 Compaction을 실행할 수 있고 메모리 할당이 더 쉬워진다.

**Hotspot JVM에서의 구조와 특징**

1. Young Generation - Eden, Survivor0, Survivor1 영역으로 나뉜다.
   객체가 Eden에서 생성되고 Eden이 꽉차면 **Minor GC가 발생**, 살아남은 객체가 Survivor로 이동, 객체가 살아남을 때 마다 Age가 증가하여 old Generation으로 이동할 수 있다.
2. Old Generation - Young에서 넘어온 객체들이 Old영역을 꽉채우면 **Major GC가 발생**, 속도가 느리다(Young을 제외한 모든 객체를 탐색해야 하므로)
3. Permanent Generation - Class와 메서트 메타데이터가 저장되는 공간
   공간이 모자랄 경우 Full GC(Major GC + Heap 영역 전체)안에서 GC가 발생한다.
