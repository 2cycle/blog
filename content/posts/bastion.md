---
title: "Bastion"
date: 2019-02-26T18:27:39+09:00
Categories: ["aws","security"]
Tags: ["bastion","aws"]
---



# Bastion 적용기



> AWS VPC
>
> 
>
> AWS의 VPC(Virtual Private Cloud) 서비스는 가상의 네트워크 환경을 구성케하는 서비스이다.
>
> 이 서비스를 통해 Private Network와. public Network 환경을 디자인 할 수 있다.



Instance를 생성할 때 Security group로만 접근을 제어하는 방안을 사용하곤 했는데, 실 서비스를 운영하면서 네트워크 접근의 보안을 위해 bastion 구성을 도입했고 일부분 구축한 경험을 정리해보고자 한다.



<img width="715" alt="bastion" src="https://user-images.githubusercontent.com/17693443/53405157-91aaae00-39fa-11e9-9588-7231403501ba.png">



Bastion (번역 - 요새)서비스는 bastion host를 통해 다른 VM에 ssh로 연결하게 하는 보안 방법이기 때문에 public subnet에 위치해야하고 방화벽 및 보안 정책을 적용시켜야한다.



bastion을 통해 ssh 접근 보안 방식은 여러 방법이 있겠지만 한 가지 방안을 정리해보면

1. bastion 서버에서 ssh key를 생성 후 복사
2. 각 운영서버의 .ssh/authorized_keys 파일 내에 bastion public key를 복사
3. 운영서버의 inbound정책을 bastion으로만 설정
4. Bastion 에서 접근을 편하게 하기 위해 hosts를 등록하고, bashrc에서 server alias를 등록



을 통해 bastion서버 설정을 사용할 수 있다.



출처 - http://tenmilesquare.com/using-ssh-through-a-bastion-host-transparently/