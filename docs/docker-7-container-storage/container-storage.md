# 컨테이너 스토리지

컨테이너에 적제된 데이터는 컨테이너를 삭제할 때 모두 사라집니다.   
이를 방지하기 위해 도커 호스트에 컨테이너 볼륨을 구성해 영구적으로 데이터를 저장할 수 있습니다.   
또한 컨테이너 간 볼륨의 데이터를 공유할 수 있습니다.
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


## 1. MySQL 데이터 영구 보존하기
Mysql 컨테이너를 실행합니다.
> 컨테이너 실행
> ```
> docker run -d --name db -v /dbdata:/var/lib/mysql -e MYSQL_PASSWORD=pass mysql:latest
> ```
> - ``` -v /dbdata:/var/lib/mysql ```  볼륨 마운트 시 사용하는 옵션입니다.
<br />

컨테이너에 접속하여 MySQL Database를 생성합니다.
> 컨테이너 접속
> ```
> docker exec -t db /bin/bash
> ```
<br />

MySQL에 루트 계정으로 접속합니다.
> MySQL 루트 로그인
> ```
> mysql -u root -ppass
> ```
<br />

Database를 생성합니다.
> Database 생성
> ```
> create database jeongwon;
> ```
<br />

Database가 잘 생성되었는지 확인합니다.
> Database 확인
> ```
> show databases;
> ```
<br />


웹데이터 readonly 서비스로 지원하기
컨테이너 간 데이터 공유하기
