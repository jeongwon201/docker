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


## 1. 컨테이너 네트워크 사용하기
docker0 인터페이스를 확인합니다.
> docker0 인터페이스 확인
> ```
> ip addr
> brctl show
> ```
> - ```ip addr``` : docker0 인터페이스를 확인할 수 있다.
> - ```brctl show``` : docker0 가 bridge 인터페이스임을 확인할 수 있다.

컨테이너 포트 외부로 노출하기
user-defined 네트워크 구성하기
컨테이너 간 통신: wordpress, mysql 컨테이너 서비스 구축하기
