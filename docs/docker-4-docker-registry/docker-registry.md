# 도커 레지스트리
도커 Private Registry를 운영합니다.

1.hub.docker.com에 컨테이너 업로드 및 다운로드(public registry)

httpd 키워드를 담고있는 컨테이너 이미지를 public registry로 부터 가져옴
docker search httpd

httpd 컨테이너 이미지 다운로드
docker pull httpd:latest

이미지 확인
docker images

도커 로그인
docker login

컨테이너 이미지 태그 변경
docker tag httpd:latest DOCKER_ID/httpd:latest

도커 내 도커 리포지토리에 컨테이너 이미지 업로드
docker push jeongwon201/httpd:latest



2. private registry 운영하기(로컬 저장소로 운영하는 것을 학습)

private registry quick start(regisry 컨테이너 이미지 다운 후 실행까지 자동으로 해줌)
docker run -d -p 5000:5000 --restart always --name registry registry:2

이제 httpd 컨테이너 이미지를 개인 저장소에 넣어줄거임 그 전에 tag 변경
docker tag httpd:latest localhost:5000/httpd:latest

이미지 확인
docker images

이미지 푸시
docker push localhost:5000/httpd:latest

로컬 저장소로 이동하기 전 root 계정 로그인
su -

컨테이너 이름 확인
cd /var/lib/docker/volumes
ls

잘 저장되었는지 확인
cd 컨테이너이름/_data/docker/registry/v2/repositories
ls
