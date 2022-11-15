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
> - ``` /dbdata: ``` 는 도커 호스트의 경로입니다.   
>  이를 생략하게 되면 ``` /var/lib/docker/volumes/UUID/_data ``` 경로로 볼륨 마운트를 하게 됩니다.
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

MySQL과 컨테이너의 접속을 종료합니다.
> ``` exit ``` 명령어를 두 번 실행하여 우분투로 돌아옵니다.
<br />

컨테이너를 삭제합니다.
> 컨테이너 삭제
> ```
> docker rm -f db
> ```
<br />

삭제된 컨테이너의 데이터가 영구 저장이 잘 되었는지 확인합니다.
> 영구 저장 확인
> ```
> cd /dadata
> ls
> ```
<br />

생성된 도커 볼륨을 확인합니다.
> 도커 볼륨 확인
> ```
> docker volume ls
> ```
<br />

생성된 도커 볼륨을 삭제합니다.
> 도커 볼륨 삭제
> ```
> docker volume rm [VOLUME NAME]
> ```
<br />

## 2. 웹데이터 readonly 서비스로 컨테이너에 지원하기
작업 폴더를 생성합니다.
> 작업 폴더 생성
> ```
> mkdir /webdata
> cd /webdata
> ```
<br />

간단한 HTML 파일을 생성합니다.
> HTML 파일 생성
> ```
> echo "<h1>Jeongwon's Docker Practice</h1>" > index.html
> ```
<br />

웹 서버 컨테이너를 실행합니다.
> 컨테이너 실행
> ```
> docker run -d --name web -p 80:80 -v /webdata:/usr/share/nginx/html:ro nginx:1.14
> ```
<br />

Nginx 서버에 접속합니다.
> 서버 접속
> ```
> curl localhost:80
> ```
<br />

볼륨에 있는 index.html을 수정합니다.
> 볼륨으로 이동
> ```
> cd /webdata
> ```
> <br />
>
> HTML 파일 수정
> ```
> vi index.html
> <h1>Jeongwon's Docker Practice - Changed</h1>
> ```
<br />

Nginx 서버에 접속합니다.
> 서버 접속
> ```
> curl localhost:80
> ```
<br />

## 3. 컨테이너 간 데이터 공유하기
