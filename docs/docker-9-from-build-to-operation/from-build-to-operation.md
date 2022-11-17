# 빌드에서 운영까지

여러 컨테이너를 일괄적으로 정의하고 실행하는 툴인 **docker-compose**를 이용하여 컨테이너화 된 애플리케이션을 통합 관리해야 합니다.
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


## 1. docker-compose 설치하기
docker-compose를 설치합니다.
> docker-compose 설치
> ```
> apt-get update
> apt-get install docker-compose-plugin
> ```
> <br />
>
> docker-compose 설치 확인
> ```
> docker compose version
> ```
<br />


## 2. 컨테이너 빌드에서 운영까지
- 웹 사이트에 접속할 때마다 방문 횟수를 출력하는 애플리케이션을 운영합니다.
<br />

서비스 디렉토리를 생성합니다.
> 디렉토리 생성
> ```
> mkdir composetest
> cd composetest
> ```
<br />

서비스 애플리케이션 app.py 파일을 생성합니다.
> app.py 생성
> ```
> vi app.py
> ```
> <br />
> 
> app.py
> ```
> import time
>
> import redis
> from flask import Flask
> 
> app = Flask(__name__)
> cache = redis.Redis(host='redis', port=6379)
> 
> def get_hit_count():
>    retries = 5
>    while True:
>        try:
>            return cache.incr('hits')
>        except redis.exceptions.ConnectionError as exc:
>            if retries == 0:
>                raise exc
>            retries -= 1
>            time.sleep(0.5)
>
> @app.route('/')
> def hello():
>     count = get_hit_count()
>     return 'Hello World! I have been seen {} times.\n'.format(count)
> ```
<br />

라이브러리 설치를 위한 requirements.txt 파일을 생성합니다
> requirements.txt 생성
> ```
> vi requirements.txt
> ```
> <br />
> requirements.txt
> ```
> flask
> redis
> ```
<br />

Dockerfile 생성합니다.
> Dockerfile 생성
> ```
> vi Dockerfile
> ```
> <br />
>
> Dockerfile
> ```
> # syntax=docker/dockerfile:1
> FROM python:3.7-alpine
> WORKDIR /code
> ENV FLASK_APP=app.py
> ENV FLASK_RUN_HOST=0.0.0.0
> RUN apk add --no-cache gcc musl-dev linux-headers
> COPY requirements.txt requirements.txt
> RUN pip install -r requirements.txt
> EXPOSE 5000
> COPY . .
> CMD ["flask", "run"]
> ```
<br />

docker-compose.yml 파일을 생성합니다.
> docker-compose.yml 생성
> ```
> vi docker-compose.yml
> ```
> <br />
>
> docker-compose.yml
> ```
> version: "3.9"
> services:
>   web:
>     build: .
>     ports:
>       - "8000:5000"
>     volumes:
>       - .:/code
>     environment:
>       FLASK_DEBUG: True
>   redis:
>     image: "redis:alpine"
> ```
<br />

docker-compose로 통합 실행합니다.
> docker-compose 통합 실행
> ```
> docker compose up
> ```

실행된 애플리케이션에 접속합니다.
> XShell 세션 복제
> <br />
>
> 접속
> ```
> curl 172.18.0.3:5000
> ```
> - docker compose up 커맨드 실행 시 할당받은 IP가 출력됩니다. 출력된 IP로 접속합니다.
<br />

docker-compose로 생성된 컨테이너를 삭제합니다
> docker-compose로 생성된 컨테이너 삭제
> ```
> docker compose down
> ```
<br />

MySQL 데이터베이스를 사용하는 Wordpress 운영하기
