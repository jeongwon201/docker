# 컨테이너 살펴보기

도커 허브에서 컨테이너 이미지를 다운로드받아 컨테이너를 실행합니다.   
<br />

## 실습 환경 세팅

 Virtual Box
> 우분투 VM 실행
<br />
 
 XShell   
> 도커 우분투 세션 연결 후 루트 계정 로그인
> - su -
<br />
 
 XShell 세션 복제
> 위에서 생성한 세션에 마우스 오른쪽 클릭 후 세션 복제
<br />
 
 도커 데몬 상태 확인
> 루트 세션에서 실행
> - systemctl status docker
<br />

## 1. 컨테이너 이미지 검색
도커 허브(Docker Hub)에 업로드된 컨테이너 이미지를 검색합니다.
> 사용자 세션에서 실행
> - docker search nginx
<br />

## 2. 컨테이너 이미지 다운로드
컨테이너 이미지가 다운로드되는 경로로 이동하여 리스트를 확인합니다.
- 이미지 레이어를 모두 확인할 수 있습니다.
> 루트 세션에서 실행
> - cd /var/lib/docker/overlay2
> - ls -l
<br />

다운로드한 컨테이너 이미지는 다음 커맨드로 확인할 수 있습니다.
> 다운로드한 컨테이너 이미지 확인(사용자 계정)
> - docker images
<br />

컨테이너 이미지를 다운로드합니다.
- 컨테이너 이미지의 다운로드 과정을 레이어 단위로 확인할 수 있습니다.
> 사용자 세션에서 실행
> - docker pull nginx
<br />

다운로드받은 컨테이너 이미지를 확인합니다.
> 루트 세션에서 실행
> - cd /var/lib/docker/overlay2
> - ls -l
<br />

> 사용자 세션에서 실행
> - docker images
<br />

## 3. 컨테이너 실행
다운로드 받은 컨테이너 이미지를 실행합니다.
> 사용자 세션에서 실행
> - docker run --name web -d -p 80:80 nginx

##### ※ 실행 시 출력되는 UUID가 컨테이너의 ID 입니다.   
<br />

컨테이너의 실행 상태를 확인합니다.
> - docker ps
<br />

실행 중인 nginx 서버에 접속합니다.
> - curl localhost:80
<br />  

## 4. 컨테이너 종료
실행 중인 컨테이너를 종료합니다.
> - docker stop web
<br />

정상적으로 종료되었는지 확인합니다.
> - docker ps
<br />

## 5. 컨테이너, 컨테이너 이미지 삭제
컨테이너를 삭제합니다.
> - docker rm web

##### ※ 컨테이너는 삭제되었지만 컨테이너 이미지는 삭제되지 않습니다.
<br />

컨테이너가 삭제되었는지 확인합니다.
> - docker ps
<br />

컨테이너 이미지를 삭제합니다.
> - docker rmi nginx
<br />

컨테이너 이미지가 삭제되었는지 확인합니다.
> 사용자 세션에서 실행
> - docker images
<br />

> 루트 세션에서 실행
> - cd /var/lib/docker/overlay2
> - ls -l
