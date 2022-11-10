# 도커 컨테이너 만들어보기 | 연습 문제
도커 컨테이너 만들어보기 실습 후 연습 문제입니다.   
<br />
<br />

## 문제 1.
주어진 Script를 실행하는 컨테이너를 빌드하시오.   
- 컨테이너 이름 : fortune:20.02
- Dockerfile의 내용
  - base image: debian
  - 컨테이너에 아래의 webpage.sh 파일 복사
    > #!/bin/bash   
    > mkdir /htdocs   
    > while:   
    > do   
    > 　/usr/games/fortune > /htdocs/index.html   
    > 　sleep 10   
    > done   
- 컨테이너에 fortune 애플리케이션 설치 : apt-get update && apt-get install fortune
<br />

컨테이너 실행 시 저장한 webpage.sh가 실행되도록 구성하시오.
