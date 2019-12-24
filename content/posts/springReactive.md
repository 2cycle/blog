---
title: "SpringReactive"
date: 2019-03-13T19:23:24+09:00
Categories: ["Spring","SrpingReactive"]
Tags: ["Spring"]
draft: true
---



# 스프링 리액티브 프로그래밍

> 스프링리액티브 - 우아한 Tech 세미나 후기 
>
> 토비(이일민)님 강의



1. 스프링 리액티브 프로그래밍이란 무엇인가?
2. 스프링 리액티브 프로그래밍은 어디에 필요한가? 쓰레드풀 헬!!
3. 스프링 리액티브 프로그래밍은 어떻게 하는가?





- What is reactive programming?

  - Reactive?

    이벤트 기반으로 응답 또는 동작이 진행되는 것.
    웹 요청을 받으면 처리 후 응답하는 것

  - Erik Meijer - reactive 창시자
    리액티브 4가지 속성



## 스프링 중심으로 생각하는 리액티브 프로그래밍

1. 인프라 스트럭처에 대한 도전
   - 인프라 리소스의 효율적인 운용
2. 프로그래밍 모델의 전환
   - 보이지 않는 리소스 문제 해결을 위한 코드 작성 방식의 변화
   - 불편하고, 지저분하고, 어려울 수 있다.
3. [종합] 가용성과 응답성을 향상시켜 효율성을 얻는 것
   - 일반적으로 성능이 좋다고 여길 순 없다!



* 논 블로킹
* 이벤트드리븐
* 작은 수의 쓰레드
* 배압



스프링 5에서는 Reactive 와 servlet 으로 구분되지만, MVC(servlet)방식에서 reactive(3가지)를 적용할 수 있고 이 방식은 전체 리액티브 개발 방식의 70%를 차지할 수 있다.





#### 비동기와 논블록킹 IO

> 서버자원을 효율적으로 사용하자.



IntRange, Flux 



------------

2부

## Reactive Streams와 Reacctor

### Reactive Streams

> 논블록킹 백프레셔를 이용하는 비동기 데이터 스트림 처리 표준
>
> 비동기 논블록킹 + 데이터 스트리밍 + 백프레셔

+ Publisher 는 Subscriber의 요청에 따라 제한없는 일련의 데이터 제공
  + Publisher 데이터 제공, Subscriber 데이터 구독하여 처리



#### Reactor API (Reactive Streams 구현)





## 리엑티브를 쓸 때 기억할 것

- 비동기 - 논블록킹 서비스를 만드는 것이 목적
- Publisher,Subscriber를 직접 만드는 경우 거의 없음
- Subscribe()는 스프링 MVC, WebFlux가 담당
  - 코드내 직접 쓸 일 없음.
- 비동기 - 논블록킹  Publisher 생성은 리액티브 라이브러리가 담당



