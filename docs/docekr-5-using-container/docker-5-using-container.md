# 도커 컨테이너 사용하기
컨테이너 이미지를 관리하고, 실행, 종료하는 명령어를 사용하여 운영합니다.

1. 컨테이너 이미지 관리 명령어 사용하기

컨테이너 이미지 검색
docker search nginx

컨테이너 이미지 다운로드
docker pull nginx:1.14
docker pull mysql / 뒤에 버전안쓰면 latest 버전을 다운받음
docker pull mysql:8 / mysql과 mysql8은 동일한 이미지이고, tag로 구분한다.

컨테이너 이미지 확인
docker images
docker images --no-trunc / 이미지 ID를 풀네임으로 출력함


3. 컨테이너 실행 및 운영하기

컨테이너 생성
docker create --name webserver nginx:1.14

컨테이너 실행
docker start webserver

컨테이너 확인
docker ps // 실행중인 컨테이너 확인
docker ps -a // 모든 컨테이너 확인

컨테이너 상세 확인
docker inspect webserver
docker inspect --format '{{.NetwrokSettings.IPAddress}}' webserver // 원하는 정보만 출력, 여기서는 IP 정보

단축 명령어 등록
alias cip="docker inspect --format '{{.NetwrokSettings.IPAddress}}'"

컨테이너에서 실행중인 프로세스 확인
docker top webserver

컨테이너 접속
curl 172.17.0.2

컨테이너 로그 확인
docker logs webserver
docker logs -f webserver // 로그 실시간 모니터링

컨테이너에 직접 접속하여 배시 쉘 사용 // 동작중인 컨테이너에 접속하여 웹 페이지를 바꿔보자
docker exec -it webserver /bin/bash
cd /usr/share/nginx/html/
ls // index.html 파일있음, 이걸 수정할 거임
echo "jeongwon's HOMEPAGE" > index.html
exit
curl 172.17.0.2

5. 컨테이너 종료하기

컨테이너 중지
docker stop webserver

컨테이너 삭제
docker rm webserver
※ docker rm -f webserver // 실행중인 컨테이너를 중지하고 삭제함
