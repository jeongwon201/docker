# 도커 컨테이너 만들어보기
Dockerfile을 이용하여 컨테이너를 빌드해보고 Docker Hub에 배포합니다.
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

## 1. 컨테이너 만들기
2 가지 컨테이너를 만듭니다.   
- Node.js 애플리케이션 컨테이너
- 우분투 기반 웹 서버 컨테이너
<hr />


### [Nods.js 애플리케이션]
컨테이너 이미지를 빌드하기 위해 작업 폴더를 생성하여 해당 폴더로 이동합니다.
> 폴더 생성
> - mkdir hellojs
> - cd hellojs
<br />

실행할 Node.js 소스코드를 생성합니다.
> js 파일 생성
> - vi hello.js
> <br />
> 
> 아래 코드 입력
```
const http = require('http');
const os = require('os');   
console.log("Test server starting...");   
     
var handler = function(request, response) {   
    console.log("Received request from" + request.connection.remoteAddress);   
    response.writeHead(200);   
    response.end("Container Hostname: " + os.hostname() + "\n");   
};   
 
var www = http.createServer(handler);   
www.listen(8080);
```
<br />

컨테이너 이미지 빌드에 필요한 Dokerfile을 생성합니다.
> Dockerfile 생성
> - vim dockerfile
> <br />
> 
> 아래 코드 입력
```
FROM node:14
COPY hello.js /
CMD ["node", "/hello.js"]
```
<br />

컨테이너 이미지를 빌드합니다.
> 컨테이너 이미지 빌드
> - docker build -t hellojs:latest .
<br />

빌드된 컨테이너 이미지를 확인합니다.
> 컨테이너 이미지 확인
> - docker images
<br />
<hr />
<br />

### [우분투 기반 웹 서버]
컨테이너 이미지를 빌드하기 위해 작업 폴더를 생성하여 해당 폴더로 이동합니다.
> 폴더 생성
> - mkdir webserver
> - cd webserver
<br/>

컨테이너 이미지 빌드에 필요한 Dokerfile을 생성합니다.
> 도커파일 생성
> - vim Dockerfile
```
FROM ubuntu:18.04
LABEL maintainer="입력하고싶은 값을 입력하세요."
# install apache
RUN apt-get update \
  && apt-get install -y apache2
RUN echo "TEST WEB" > /var/www/html/index.html
EXPOSE 80
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]
```
<br />

컨테이너 이미지를 빌드합니다.
> 컨테이너 이미지 빌드
> - docker build -t webserver:v1 .


빌드된 컨테이너 이미지를 확인합니다.
> 컨테이너 이미지 확인
> - docker images
<br />

## 2. 컨테이너 실행하기
빌드한 2 가지의 컨테이너 이미지를 실행합니다.
- Node.js 애플리케이션 컨테이너
- 우분투 기반 웹 서버 컨테이너
<hr />

### [Node.js 애플리케이션 컨테이너]

컨테이너를 실행합니다.
> 컨테이너 실행
> - docker run -d -p 8080:8080 --name web hellojs
<br />

실행한 컨테이너의 상태를 확인합니다.
> 컨테이너 상태 확인
> - docker ps
<br />

실행 중인 컨테이너에 접속합니다.
> 접속
> - curl localhost:8080
```
Container Hostname: *********
```
<br />

컨테이너를 삭제합니다.
- 실행 중인 컨테이너를 삭제할 때는 -f 옵션을 추가해야합니다.
> 컨테이너 삭제
> - docker rm -f web
<br />
<hr />
<br />

### [우분투 기반 웹 서버 컨테이너]

컨테이너를 실행합니다.
> 컨테이너 실행
> - docker run -d -p 80:80 --name web webserver:v1
<br />

실행한 컨테이너의 상태를 확인합니다.
> 컨테이너 상태 확인
> - docker ps
<br />


실행 중인 컨테이너에 접속합니다.
> 접속
> - curl localhost:80
```
TEST WEB
```
<br />


컨테이너를 삭제합니다.
- 실행 중인 컨테이너를 삭제할 때는 -f 옵션을 추가해야합니다.
> 컨테이너 삭제
> - docker rm -f web
<br />

## 3. 컨테이너 배포하기

도커에 로그인 합니다.
> 도커 로그인
> - docker login

※ 이메일 계정이라면, 주소는 생략합니다.
<br />

컨테이너의 이름(태그/tag)을 Docker Hub 배포 형식으로 변경합니다.
> 컨테이너 이름(태그/tag) 변경
> - docker tag webserver:v1 도커ID/webserver:v1
> - docker tag hellojs:latest 도커ID/hellojs:latest
<br />

Docker Hub에 배포합니다.
> Docker Hub에 Push
> - docker push 도커ID/webserver:v1
> - docker push 도커ID/hellojs:latest
<br />

Docker Hub에 접속하여 배포가 잘 되었는 지 확인합니다.
> https://hub.docker.com/
