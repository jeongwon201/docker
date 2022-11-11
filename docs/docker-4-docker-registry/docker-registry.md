# 도커 레지스트리
도커 Public · Private Registry를 운영합니다.
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

## 1.Public Registry
Docker Hub에 업로드된 컨테이너 이미지를 다운로드 받고, 도커 Repository에 업로드합니다.
<br />

httpd 키워드로 Docker Hub에 컨테이너 이미지를 검색합니다.
> httpd 검색
> - docker search httpd
<br />

검색 리스트를 참고하면서, httpd 컨테이너 이미지를 다운로드합니다.
> httpd 다운로드
> - docker pull httpd:latest
<br />

이미지가 잘 다운로드 되었는지 확인합니다.
> 이미지 확인
> - docker images
<br />

도커에 로그인합니다.
> 도커 로그인
> - docker login
<br />

업로드를 위해 다운로드한 컨테이너 이미지의 태그를 변경합니다.
> 컨테이너 이미지 태그 변경
> - docker tag httpd:latest DOCKER_ID/httpd:latest
<br />

내 도커 계정의 리포지토리에 컨테이너 이미지를 업로드합니다.
> 컨테이너 이미지 업로드
> - docker push DOCKER_ID/httpd:latest
<br />

## 2. Private Registry
호스트 PC의 내부(로컬)에 독립적인 Repository를 구성하여 운영합니다.
<br />

컨테이너 이미지 다운로드부터 컨테이너 실행까지 자동으로 구성해주는 Quick Start 명령어를 입력합니다.
> Quick Start
> - docker run -d -p 5000:5000 --restart always --name registry registry:2
<br />

Public Registry 학습 과정에서 다운로드받은 httpd 컨테이너 이미지를 개인 저장소에 업로드하기 전, 태그를 변경합니다.
> 컨테이너 이미지 태그 변경
> - docker tag httpd:latest localhost:5000/httpd:latest
<br />

잘 변경되었는지 확인합니다.
> 이미지 확인
> - docker images
<br />

컨테이너 이미지를 Private Registry에 업로드합니다.
> 이미지 업로드
> - docker push localhost:5000/httpd:latest
<br />

Private Registry에 잘 저장되었는지 확인합니다.
> 루트 계정 접속
> - su -
>    
> - cd /var/lib/docker/volumes
> - ls   

※ 긴 이름의 디렉토리가 하나 생성되어 있는데, 이는 컨테이너 이름이며 해당 디렉토리로 이동합니다.

> - cd 컨테이너이름/_data/docker/registry/v2/repositories
> - ls
<br />
httpd 컨테이너 이미지가 잘 업로드된 것을 확인할 수 있습니다.
