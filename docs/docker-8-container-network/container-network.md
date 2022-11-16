# 컨테이너 간 통신(네트워크)

컨테이너는 실행될 때 docker0 라는 네트워크 인터페이스가 생성됩니다.    
docker0는 네트워크 브릿지를 통해 컨테이너가 외부 통신을 할 수 있도록 네트워크 기능을 제공합니다.   
<br />
<br />
<br />

## 실습 환경 세팅

 Virtual Box
> 우분투 VM 실행
<br />
 
 XShell   
> 도커 우분투 세션 연결
<br />

브릿지 네트워크 확인을 위한 프로그램 설치
> bridge-utils 설치
> ```
> apt install bridge-utils
> ```
<br />


## 1. 컨테이너 네트워크(docker0) 사용하기
docker0 인터페이스를 확인합니다.
> docker0 인터페이스 확인
> ```
> ip addr
> brctl show
> ```
> - ```ip addr``` : docker0 인터페이스를 확인할 수 있다.
> - ```brctl show``` : docker0 가 bridge 인터페이스임을 확인할 수 있다.
<br />

컨테이너 c1 을 실행합니다.
> 컨테이너 c1 실행
> ```
> docker run --name c1 -it busybox
> ip addr
> ```
> - docker0 인터페이스는 ip를 순차적으로 부여하기 때문에 ```172.17.0.2``` IP 주소가 할당된다.
<br />

XShell 세션 복제 후 컨테이너 c2 을 실행합니다.
> 컨테이너 c2 실행
> ```
> docker run --name c2 -it busybox
> ip addr
> ```
> - docker0 인터페이스는 ip를 순차적으로 부여하기 때문에 ```172.17.0.3``` IP 주소가 할당된다.
<br />

XShell 세션 복제 후 컨테이너 web1 을 실행합니다.
> 컨테이너 web1 실행
> ```
> docker run -d -p 80:80 --name web1 nginx
> curl 172.17.0.4
> ```
> - docker0 인터페이스는 ip를 순차적으로 부여하기 때문에 ```172.17.0.4``` IP 주소가 할당된다.
<br />

다음 학습을 위해 컨테이너를 삭제합니다.
> 컨테이너 삭제
> ```
> exit
> docker rm -f c1
> 
> exit
> docker rm -f c2
> 
> docker rm -f web1
> ```
<br />


## 2. 컨테이너 포트 외부로 노출하기
포트 포워딩( ```-p``` )
> ```
> docker run -d -p 80:80 --name web1 nginx
> curl localhost:80
> 
> docker run -d -p 80 --name web2 nginx
> docker ps
> curl localhost:[포트]
>
> docker run -d -P --name web3 nginx
> docker ps
> curl localhost:[포트]
> ```
> - ```-p 80:80```: 호스트의 80 포트를 80 포트로 포워딩합니다.
> - ```-p 80```: 호스트 포트를 생략하게되면 랜덤한 포트를 할당받습니다.
> - ```-P```: 랜덤 포트를 할당하여 Dockerfile 에 정의된 포트 번호로 포워딩합니다.
<br />

다음 학습을 위해 컨테이너를 삭제합니다.
> 컨테이너 삭제
> ```
> docker rm -f web1
> docker rm -f web2
> docker rm -f web3
> ```
<br />

## 3. user-defined 네트워크 구성하기
user-defined 네트워크를 생성합니다.
> 네트워크 생성
> ```
> docker network create --driver bridge --subnet 192.168.100.0/24 --gateway 192.168.100.254 mynet
> docker network ls
> ```
> - ```--driver bridge``` 옵션 생략 시 default 는 bridge 입니다.
> - ```--subnet``` 옵션 생략 시 순서대로 자동 할당합니다.
> - ```--gateway``` 옵션 생략 시 비어있는 순서대로 할당합니다.
<br />

user-defined 네트워크를 사용하는 컨테이너를 생성합니다.
> 컨테이너 생성
> ```
> docker run --name c1 -it --net --ip 192.168.100.100 mynet busybox
> ip addr
> ```
> - ```--ip [IP ADDRESS]```: 고정 IP 할당 시 사용하는 옵션입니다. 이를 생략하면 순차적으로 IP를 할당합니다.
<br />

다음 학습을 위해 컨테이너를 삭제합니다.
> 컨테이너 삭제
> ```
> exit
> docker rm -f c1
> ```
<br />


## 4. 컨테이너 간 통신: wordpress, mysql 컨테이너 서비스 구축하기
MySQL 컨테이너를 실행합니다.
> 컨테이너 실행
> ```
> docker run -d --name mysql -v /dbdata:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=wordpress -e MYSQL_PASSWORD=wordpress mysql:5.7
> ```
<br />

WordPress 컨테이너를 실행합니다.
> 컨테이너 실행
> ```
> docker run -d --name wordpress --link mysql:mysql -e WORDPRESS_DB_PASSWORD=wordpress -p 80:80 wordpress:4
> ```
> - ```--link mysql:mysql```: MySQL 컨테이너와 링크하여 서로 연동는 옵션입니다.
<br />
