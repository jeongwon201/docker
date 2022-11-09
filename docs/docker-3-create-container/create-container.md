# 도커 컨테이너 만들어보기

## 실습 환경 세팅

 Virtual Box
> 우분투 VM 실행
<br />
 
 XShell   
> 도커 우분투 세션 연결
<br />




1. node js 애플리케이션 컨테이너 만들기 : hellojs

폴더 생성
mkdir hellojs
cd hellojs


node js 애플리케이션 소스코드 생성
cat > hello.js

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



도커 파일 생성
vi dockerfile

FROM node:14
COPY hello.js /
CMD ["node", "/hello.js"]


컨테이너 이미지 빌드
docker build -t hellojs:latest .


빌드된 이미지 확인
docker images






2. 우분투 기반의 웹 서버 컨테이너 만들기

폴더 생성
mkdir webserver
cd webserver


도커파일 생성
vim Dockerfile

FROM ubuntu:18.04
LABEL maintainer="Jeongwon"
# install apache
RUN apt-get update \
  && apt-get install -y apache2
RUN echo "TEST WEB" > /var/www/html/index.html
EXPOSE 80
CMD ["/usr/sbin/apache2ctl", "-DFOREGROUND"]


컨테이너 이미지 빌드
docker build -t webserver:v1 .


생성된 컨테이너 이미지 확인
docker images


컨테이너 실행/우분투서버
docker run -d -p 80:80 --name web webserver:v1


컨테이너 상태 확인
docker ps


웹서버 접속 / TEST WEB 나오면 성공
curl localhost:80


컨테이너 삭제 : 러닝 중인 컨테이너라 -f 옵션 붙여야됨
docker rm -f web


컨테이너 실행/nodejs 서버
docker run -d -p 8080:8080 --name web hellojs


컨테이너 상태 확인
docker ps


웹서버 접속 / Container Hostname: ********* 나오면 성공
curl localhost:8080


컨테이너 삭제 : 러닝 중인 컨테이너라 -f 옵션 붙여야됨
docker rm -f web







4. 만들어놓은 컨테이너 배포하기

도커 허브 로그인 / 이메일 뺴고 아이디만 입력하셈
docker login


도커 이름(태그/tag) 바꾸기
docker tag webserver:v1 jeongwon201/webserver:v1
docker tag hellojs:latest jeongwon201/hellojs:latest


도커 허브에 푸시
docker push jeongwon201/webserver:v1
docker push jeongwon201/hellojs:latest

도커 허브에 배포가 되었는지 
