---
title: "LinuxCommand_2"
date: 2019-01-23T18:29:03+09:00
Categories: ["linux"]
Tags: ["linuxCommand","crontab"]
---

## crontab



현재 실행중인 crontab list

```shell
# crontab -l
```



Crontab 편집

```shell
# crontab -e
```

다음과 같이 입력하면 vi처럼 편집할 수 있는 화면이 생긴다.

여기서 필요한 job들을 추가한 후 :wq로 저장하면 자동으로 crontab 에 job이 추가된다.



### job설정

##### 주기 결정

```shell
 *		*		*		*		*
(분)   (시간)    (일)	 (월)	(요일)
```

분, 시간, 일 ,월, 요일 순이며 요일의 경우 0과 7이 일요일이며 1부터 월요일, 6이 토요일이다.

##### 주기 예제

```shell
* * * * *
매분 실행

0 0 * * *
매일 자정 실행

0,30 * * 1-5
월요일부터 금요일까지 매시간 0분, 30분마다 실행

*/10 * * * * 
매 10분 마다 실행

*/10 22,23 5-10 * * 
5일에서 10일까지 22시, 23시 매 10분마다 실행
```



##### job example

``` shell
* * * * * /root/test/test.sh
test.sh를 매 분마다 실행
```



##### logging

``` shell
* * * * * /root/test/test.sh > /var/log/test/test.sh.log 2>&1
test.sh.log에 파일이 매분마다 경신됨

* * * * * /root/test/test.sh >> /var/log/test/test.sh.log 2>&1
로그 누적해서 쌓음

* * * * * /root/test/test.sh > /dev/null 2>&1
로그 안쌓음

 2>&1이란?
 n >&m : 표준 출력과 표준 에러를 서로 바꾸는 것이다.
 0: 표준 입력 , 1: 표준 출력, 2: 표준에러를 의미한다.
 즉 2>&1이란 표준 에러를 표준 출력이 전달되는 곳으로 전달! 이라는 의미이다. (표준 입력의 경우 <&0)
 
 조금 더 들어가면 /dev/null 은 /dev/null로 출력하므로 출력이 안보이기에 로그를 남기지 않는 것입니다.

```







참고 

jdm.kr/blog/2

www.adminschoice.com/crontab-quick-reference















