---
title: "LinuxCommand_1"
date: 2019-01-23T17:41:10+09:00
Categories: ["linux"]
Tags: ["linuxcommand"]
---

요 근래 회사에서 주로 진행한 업무는 check_mk 모니터링 및 SSL 만료확인용 shell script 를 만드는 것이었다.

리눅스 사용시 몇 개의 익숙한 명령문만 사용하였지만, 이번에 사용한 여러 명령문을 정리해보려고 한다.

## 리눅스 커널 확인

#### getconf

  *getcong utility shall write to the standard output the value of the variable spcified by the system_var operand*

즉 시스템 환경변수를 확인할 수 있는 명령문이다.

> **-v** [path_var, pathname, system_var]
>
> 아마도 구성된 변수를 결정하는 specific한 사양이나 버전을 표시하는 것 같다.



```shell
# getconf LONG_BIT
 > 32 or 64 
 각 os의 비트 값을 출력한다.
```



### arch

```shell
# arch
 > i386 or i686 or x86_64
 i363,i686의 경우 32비트이다. 2015 macpro mid 기준 i386
```



### uname

```shell
# uname -m
 > x86_64
 -a : 모든 정보
 -s : systeom OS 정보
 -n : 호스트 이름 정보
```



참고

linux.die.net/man/1/getconf

Https://zetawiki.com/wiki/ 





​	 



