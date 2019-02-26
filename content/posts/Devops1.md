---
title: "Devops1"
date: 2019-02-23T15:27:42+09:00
Categories: ["Devops","AWS"]
Tags: ["devops"]
draft: true
---



## Devops Chapter1.

> 강의 진행 방식
>
> \- 모든 과정은 실습 -> 개념 설명으로 진행
>
> \- 질문은 페이스북 그룹에 달아서 질문 주기
>
> \- 어떻게 해보는게 중요하지 어떤걸로 해보는게 중요한게 아니라 해보는 것에 집중해서 한다.
>
> ​    \- 다른 방식으로 해보는건 할 줄 알면 금방이다
>
> ​    \- cli가 아니라 웹 콘솔을 이용



서버 선택시

개인 학습용 - unbuntu

서버 사용 용도 - centos
	 사용이 안정적이다.



웹서버 - 요청에 대해서 응답을 해주는 역할

	- HTTP transaction을 처리하고 정적인 파일(static - html, css etc)을 요청한 대상에게 보내주는 역할

* 별다른 이슈가 없을 시 윈도우 말고는 nginx 웹서버를 쓰자.



ps 대신 yum install htop을 사용해보자!

was(dmz존)와 webserver (non - dmz)

container의 장점이 확실하지만 dbms는 아직 적용하기 어려움



Docker - 가벼운 VM이다.

- 하나의 프로그램인데 운영체제, 소스코드, 환경 등이 패키징되어 있다.
- 컨테이너를 실행하기 위한 하나의 플랫폼

- VM환경 (ec2)
  - 하이퍼바이저 - 가상화된 Guest OS를 관리, host 운영체제 위에 있다. 
  - vm환경 안에서는 guest os 단위로 움직이기 때문에 인프라와 같은 업무를 진행하는 작업이 많아져 overhead가 발생할 수 있다.
- os에서 컨테이너를 돌리기 위한 최소 부분만 빌드되어 올라간다. 즉, container에서 guest os의 기능을 가져다가 사용하기 때문에 가볍다. (host os 기능을 가져다가 씀)
  - 하지만 문제점 : host os가 break되면 이슈가 생김.
- 컨테이너 단위가 너무 작기 때문에 이 단위로 CI/CD를 사용하기 편함
- 컨테이너 오케스트레이션 도구가 필요한 이유는 



과제 - 

1. vagrant 로 vm 환경 구성 - cent os , ubuntu
2. Docker 환경에서. Cent os , ubuntu 설치해보