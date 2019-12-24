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
2. Docker 환경에서. Cent os , ubuntu 설치해보기



# Chapter 2.



<img width="747" alt="2019-03-02 2 43 22" src="https://user-images.githubusercontent.com/17693443/53677840-b011f180-3cf9-11e9-9cb3-dad1d63e5d6d.png">

- nginx 디렉토리 구조에서 ngiinx/html 폴더는 nginx config에 root path로 설정되어있다.
- Nginx.conf에서 http 프로토콜 설정이 되어있음

```sh
 server {
        listen       80 default_server;
        listen       [::]:80 default_server;
        server_name  _;
        root         /usr/share/nginx/html/Fastcampus-web-deploy;

        # Load configuration files for the default server block.
        include /etc/nginx/default.d/*.conf;

        location / {
        }

        error_page 404 /404.html;
            location = /40x.html {
        }

        error_page 500 502 503 504 /50x.html;
            location = /50x.html {
        }
    }
```

- location에서 요청마다 root 위치를 바꿔줄 수 있음

### 그 nginx 중요 사항

- Worker_processes auto
  - 몇 개의 쓰레드를 활용할 것인가.
  - auto 로 설정하면 해당 머신의 cpu core수 만큼 자동 설정됨
- Worker_connection 1024 : 
  - Worker thread당 최대 몇 개의 커넥션을 처리할 것인지 정의
  - 최대 처리량 = worker_process * worker_connections





## Load balancer

부하분산 처리. (Health-check)

- 각 인스턴스들에게 주기적으로 정상 작동 확인 요청 (ELB설정 시 **상태검사구성**에서 세팅)
  Ex) /get index.html
- 200 응답시 정상으로 이해
- 그 외 Http status code(ex.500)을 받으면 장애로 인식
  - 정상 인스턴스에만 작업 할당
- 내부 로드 밸런서 생성 - **프라이빗 IP**(Elastic IP가 아닌)가 부여되므로 내부 **ELB**(WAS Layer 단위 - 외부에서 연결되면 안됨 , non dmz zone)사용시에 체크
- 교차 영역 로드 밸런싱 활성화 - AZ , idc단위로 만들어짐 (최상단 L4는 따로 있음)
- 연결 드레이닝 활성화 - 오토 스케일링시 대기시간.



## Auto scaling (이 필요하다면? 아키텍쳐를 고민해봐.....)

1. Auto scaling 시작구성 - AMI를 통해 EC2 스펙을 설정
2. Auto scaling group - 해당 설정된 EC2스펙을 어디에 어떻게 붙일 것인가를 결정.
3. 서브넷 설정을 통해 Elastic IP 주소 할당
4. 상태검사 유형에서 ELB가 여러 대일 경우 ELB를 체크
5. 대상 그룹의 경우 API path별로 ELB가 따로 있을 때 사용

- 알림 설정
  - 경보 생성에서 지표 도달 시 필요한 정보를 알림처리.





##### consul

- Service discovery / health check.
  ex) properties ( redis.ip : redis.service.consul )
- clustering.
- Idc , clouds 사용시 multi idc , multi colocation 동작. 
- Hashcorp valut도 함께 사용함.





# Chapter 3.

- WAS LB 등록
- Proxy server 
  - Container 에선 Traefik 사용
  -  TCP or UDP 전용 서버는 envoy / HAProxy 사용.
- 배포방식
  - Canari - 두 대의 서버를 운용하면서 (신규, 기존 - 안정화) 가중치를 바꿔가면서 배포
  - Blue Green
  - Graceful shutdown - 모든 요청을 완료 후 shutdown 하는 배포.
- DB!!! 는 가장 안정적이여야한다.
  - 클러스터링은 물론 좋지만 관리가 어렵다.
    - Master db를 replica로 만들어서 사용 < 무조건 on promise 로만 사용함 (강사님 case)
  - RDS의 경우 백업시간과 유지시간이 겹치지 않게 생성해야 한다.



# Chapter 4.

- Internet Gateway
  - EC2 ip를 가변 IP로 발급
  - 원하는 서브넷을 외부 통신이 가능하게 할 수 있다.
- 큰 틀로 Region > SUbnet > AZ



>ngnix > 프로세스 정보를 따로 모르기 때문에 매번 ps -ef를 통해 꺼야함
>
>Service nginx start > service스크립트에서 관리 가능
>
>Systemctl nginx start > ngnix권장 스탭을 자동으로 실행해줌.
> Process 3단계 진행됨





# Chapter 5.

- dockerfile 에서 daemon off 명령어를 주는 이유
  - 컨테이너에서 백그라운드로 돌리면 컨테이너 os의 데몬(service, systemctl 등)이 관리하게 됨
  - 즉, 도커 데몬은 컨테이너 os로 돌리면 확인을 못하므로 daemon off를 통해 도커 데몬(프로세서 매니저가 알 수 있도록  foreground로 실행해줘야 함.)이 관리하도록 함.
  - 그래서 도커에선 대부분 foreground로 실행되게 함.
- Dockerfile 에서 WORKDIR 명령은 하기 명령어를 실행해줌
  - mkdir {directory}
  - cd {directory}
- .dockerimgnore 파일 만들어놓으면 .gitignore와 같은 역할을함.



# Chapter 6.

- config에서 mode와 placement 중 우선 순위는 placement이다.
  - Placement에서 필터링 순위를 정해주는 방식으로 진행해야 확장성이 생긴다.
  - 



# Chpater 7.

- Application load balancer
  - Port / path / domain(sub & domain)