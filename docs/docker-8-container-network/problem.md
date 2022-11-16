# 컨테이너 간 통신(네트워크) | 연습 문제
컨테이너 간 통신(네트워크) 실습 후 연습 문제입니다.   
<br />
<br />

## 1. 다음의 컨테이너를 빌드하시오.
> genid.sh
> ```
> #!/bin/bash
> mkdir -p /webdata
> while true
> do
>   /usr/bin/rig | /usr/bin/boxes -d boy >
> /webdata/index.html
>   sleep 5
> done
> ```
> <br />
>
> Dockerfile
> ```
> FROM ubuntu:18.04
> RUN apt-get update; apt-get -y install rig boxes
> ADD genid.sh /bin/genid.sh
> RUN chmod +x /bin/genid.sh
> ENTRYPOINT ["/bin/genid.sh"]
> ```
<br />

## 2. 빌드한 컨테이너를 이용해 multi-tier 컨테이너를 구축하시오.
genid는 웹 문서를 생성하고, nginx는 고객에게 서비스하는 형식으로 운영한다.
- genid 에서 생성한 index.html 은 volume을 통해 nignx의 웹 컨텐츠로 공유되어야한다.
- nginx 웹 서버는 80 포트를 통해 genid 가 생성한 html 문서를 고객에게 서비스한다.

