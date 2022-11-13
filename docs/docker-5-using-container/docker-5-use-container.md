# 도커 컨테이너 사용하기
컨테이너 이미지를 관리하고, 실행, 종료하는 명령어를 사용하여 운영합니다.
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

## 1. 컨테이너 이미지 관리하기

컨테이너 이미지를 Docker Hub에서 검색합니다.
> 컨테이너 이미지 검색   
> ```
> docker search nginx
> ```
<br />

컨테이너 이미지를 다운로드합니다.
> 컨테이너 이미지 다운로드   
> ```
> docker pull nginx:1.14  
> docker pull mysql 
> docker pull mysql:8
> ```   
> - 이미지 이름 뒤에 버전이 없으면, 최신 버전을 다운로드합니다.
> - mysql과 mysql 8은 동일한 컨테이너 이미지이고, tag로 구분합니다.
<br />

다운로드받은 컨테이너 이미지를 확인합니다.
> 컨테이너 이미지 확인
> ```
> docker images
> docker images --no-trunc
> ```
> - --no-trunc 옵션은 이미지 ID를 풀네임으로 출력합니다.
<br />

## 2. 컨테이너 실행 및 운영하기
다운로드받은 이미지를 통해 컨테이너를 생성합니다.
> 컨테이너 생성
> ```
> docker create --name webserver nginx:1.14
> ```
<br />

생성한 컨테이너를 실행합니다.
> 컨테이너 실행
> ```
> docker start webserver
> ```
<br />

컨테이너를 확인합니다.
> 컨테이너 확인
> ```
> docker ps
> docker ps -a
> ```
> - 옵션이 없으면, 실행중인 컨테이너 목록을 출력합니다.
> - -a 옵션은 모든 컨테이너 목록을 출력합니다.
<br />

컨테이너의 자세한 정보를 확인합니다.
> 컨테이너 상세 확인
> ```
> docker inspect webserver
> docker inspect --format '{{.NetwrokSettings.IPAddress}}' webserver
> ```
> - --format 옵션은 원하는 정보만 출력합니다.
> - 위 옵션에서는 컨테이너의 IP 주소만 출력하게 됩니다.
<br />

※ alias 명령어
- alias 명령어는 단축 명령어입니다.
> alias 등록
> ```
> alias cip="docker inspect --format '{{.NetwrokSettings.IPAddress}}'"
> cip webserver
> ```
> - 길이가 긴 ```docker inspect --format '{{.NetwrokSettings.IPAddress}}'``` 커맨드를 cip라는 단축 명령어로 등록합니다.
<br />

Running 상태인 컨테이너 내부에 실행 중인 프로세스를 확인합니다.
> 컨테이너에서 실행중인 프로세스 확인
> ```
> docker top webserver
> ```
<br />

컨테이너에 접속합니다.
> 컨테이너 접속
> ```
> curl 172.17.0.2
> ```
<br />

컨테이너에서 생성되는 로그를 확인합니다.
> 컨테이너 로그 확인
> ```
> docker logs webserver
> docker logs -f webserver
> ```
> - -f 옵션은 로그를 실시간으로 모니터링할 때 사용하는 옵션입니다.
<br />

동작 중인 컨테이너에 접속하여 index 파일을 수정합니다.
- 동작 중인 컨테이너의 파일을 변경할 수 있습니다.
- 이를 위해 컨테이너의 쉘을 사용합니다.
> 컨테이너에 직접 접속 후 배시 쉘 사용
> ```
> docker exec -it webserver /bin/bash
> cd /usr/share/nginx/html/
> ls
> echo "My HOMEPAGE" > index.html
> exit
> curl 172.17.0.2
> ```
<br />

## 3. 컨테이너 종료하기
컨테이너를 중지합니다.
> 컨테이너 중지
> ```
> docker stop webserver
> ```
<br />

컨테이너를 삭제합니다.
> 컨테이너 삭제
> ```
> docker rm webserver
> ```
> - -f 옵션은 실행 중인 컨테이너를 중지하고 삭제할 때 사용하는 커맨드입니다. ex) ```docker rm -f webserver```
