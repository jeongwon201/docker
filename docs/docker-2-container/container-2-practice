# 도커 컨테이너 - 실습


2. 컨테이너 이미지 다운로드 후 image layer 보기

3. 컨테이너 실행하고 확인해보기

[준비]
가상머신 키기

XShell로 도커 우분투 세션 연결 후 루트 계정 로그인
su -

XShell 세션 복제(마우스 오른쪽 클릭 후 세션 복제)하면 guru 계정으로 세션이 복제됨

도커호스트에서 도커데몬이 잘 실행중인지 확인(root 세션)
systemctl status docker
active(running) 초록색으로 잘 뜨는지 확인

1. Docker Hub에서 컨테이너 이미지 목록 검색(guru 세션)
  docker search nginx

2. 컨테이너 이미지 다운로드 후 image layer 보기
  다운로드 받기전 확인할 사항(root 계정)
    cd /var/lib/docker/
    ls -l
    
  overlay2에 컨테이너 이미지가 들어있음
    cd overlay2
    ls -l
  
  l 하나만 뜨면 컨테이너 이미지가 하나도 없는 상태임, 도커 커맨드로 확인해보자(guru 계정)
    docker images
    
  이제 nginx 컨테이너 이미지 다운로드 하겠음(guru 세션)
    docker pull nginx
  6개의 uuid가 뜨는데 nginx는 6개의 레이어로 구성되어있다는 뜻
  
  이제 overlay2에 깔려있는지 확인해보겠음(root 세션)
    cd /var/lib/docker/overlay2
    ls -l
  
  도커 이미지 잘 깔렸는지 확인(guru 세션)
    docker image ls 또는 docker images
  
  이제 도커로 실행하겠음
  이름 web, 백그라운드 모드, 80번 포트로 nginx 컨테이너 이미지 실행
    docker run --name web -d -p 80:80 nginx
  
  id 하나 뜨는데 이게 컨테이너 id 임
  
  도커 컨테이너가 잘 실행되고 있나 확인해보겠음
    docker ps
  
  이제 실행중인 nginx 서버에 접속해보겠음
    curl localhost:80
  
  이제 실행중인 도커 컨테이너 종료하겠음
    docker stop web
  
  80포트 접속해보면 안될거임
    curl localhost:80
  
  이제 도커 컨테이너를 지우겠음, 컨테이너가 지워진거지 컨테이너 이미지가 지워진건 아님
    docker rm web
  
  이제 컨테이너 이미지를 지우겠음
    docker rmi nginx
  
  루트세션으로가서 이미지 지워졌는지 확인해보자
    cd /var/lib/docker/overlay2
    ls -l
