---
title: "ssl파일 만료일자 모니터링"
date: 2019-01-23T18:49:00+09:00
Categories: ["linux"]
Tags: ["ssl_monitoring","ssl","ssl_expire"]
---

## SSL 만료 확인

Let's encrypt를 쓰면 자동 연장이 되지만 3개월마다 갱신이 되는 것에 대한 불안감과 1년 내로 사라질 서버들이기에...

간단히 ssl 인증이 만료되는 것을 방지하기위해 간단히 shell script를 만들어서 crontab에 등록해놓았다.

스크립트는 아주 간단한데

1. 먼저 ssl 위치를 확인하고 인증 만료 시간을 확인한다.

   ```shell
   $ openssl x509 -in /etc/httpd/conf/ssl/인증서.crt
   ```

2. 오늘 날짜를 가지고와서 notAfter 타임과 비교하여 차이를 구한다.

3. 조건 일자 이상인 경우 알림을 알린다. (필자의 경우 curl을 통해 post콜을 하였다.)



 ```shell
#!/bin/bash

data=`echo | openssl x509 -in /etc/httpd/conf/ssl/log.cresendo.net.crt -noout -dates | grep notAfter | sed -e 's#notAfter=##'`

#echo "${data}"

diffdays=300
ssldate=`date -d "${data}" '+%s'`
nowdate=`date '+%s'`
diff="$((${ssldate}-${nowdate}))"
ex_days="$((${diff}/3600/24))"
#echo "${diff}"
#echo "${ex_days}"

if [ ${diff} -lt $((${diffdays}*24*3600)) ]
then
    if [ ${diff} -gt 0 ]
    then
        curl -H "Content-Type: application/json" \
			-d '{"title":"WARNING!! LOG SERVER  SSL WILL BE EXPIRE", "message":"The LOG SERVER certificate will expried in '"${ex_days}"' days", "sendTo" : ["test@test.com"] }' \
-k EMAIL_API_SERVER_URL
    fi
fi

 ```

