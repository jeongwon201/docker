# 컨테이너 리소스 관리하기
기본적으로 컨테이너는 호스트 하드웨어 리소스의 사용 제한을 받지 않습니다.   
그러므로 컨테이너가 필요로 하는 만큼의 리소스만을 할당해주어야합니다.
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

부하 테스트 프로그램(Stress) 준비
- 부하 테스트 프로그램을 컨테이너에 올려서 사용합니다.

> 루트 계정 로그인
> ```
> su -
> ```
<br />

작업 폴더를 생성합니다.
> 작업 폴더 생성
> ```
> mkdir build
> cd build
> ```
<br />

컨테이너 이미지를 빌드할 Dockerfile을 생성합니다.
> Dockerfile 생성
> ```
> vim Dockerfile
>
> [Dockerfile]
> FROM debian   
> MAINTAINER jeongwon   
> RUN apt-get update; apt-get install stress -y   
> CMD ["/bin/sh", "-c", "stress -c 2"]
> ```
<br />

컨테이너 이미지를 빌드합니다.
> ```
> docker build -t stress .
> ```
<br />

## 1. 컨테이너 리소스 제한
메모리 리소스를 제한하여 컨테이너 실행 후, 부하 테스트를 진행합니다.
> 메모리 리소스 제한 및 부하 테스트
> ```
> docker run -m 100m --memory-swap 100m stress:latest stress --vm 1 --vm-bytes 90m -t 5s
> ```
> - 총 메모리 할당량과 스왑 메모리가 동일하면, 스왑 메모리는 사용하지 않습니다.   
> <br />
> 
> ```
> docker run -m 100m --memory-swap 100m stress:latest stress --vm 1 --vm-bytes 150m -t 5s
> ```
> - 총 메모리 할당량을 100m을 초과하여 부하 테스트를 진행하므로, 오류가 발생합니다.
> <br />
> 
> ```
> docker run -m 100m stress:latest stress --vm 1 --vm-bytes 90m -t 5s
> ```
> - 총 스왑 메모리 생략 시 할당량의 두 배를 스왑 메모리로 사용하여 총 200m의 메모리를 사용할 수 있습니다.
> <br />
>
> ```
> docker run -d -m 100m --name m4 --oom-kill-disable=true nginx
> ```
> - 컨테이너는 메모리가 부족하면 OOM-Killer가 메모리 확보를 위해 프로세스를 중지시키는데, 이를 비활성화 합니다.
<br />

CPU 리소스를 제한하여 컨테이너 실행 후, 부하 테스트를 진행합니다.
> CPU 개수 제한 및 부하 테스트( ```crm``` 커맨드 입력 후 진행하세요.)
> ```
> docker run --cpuset-cpus 1 --name c1 -d stress:latest stress --cpu 1
> ```
> - 1 번 CPU에게 할당합니다.
> <br />
>
> ```
> docker run --cpuset-cpus 0-1 --name c1 -d stress:latest stress --cpu 1
> ```
> - 0 또는 1 번 CPU 중 하나에 할당합니다.
> <br />
>
> CPU 상대적 가중치 할당 및 부하 테스트( ```crm``` 커맨드 입력 후 진행하세요.)
> ```
> docker run -c 2048 --name cload1 -d stress:latest
> ```
> - 1024를 기준으로 2 배를 할당합니다.
> <br />
> 
> ```
> docker run --name cload2 -d stress:latest
> ```
> - 기준 값인 1024를 할당합니다.
> <br />
> 
> ```
> docker run -c 512 --name cload3 -d stress:latest
> ```
> - 1024를 기준으로 절반만 할당합니다.
<br />

Block I/O를 제한하여 컨테이너 실행 후, 부하 테스트를 진행합니다.
> 디스크 이름 확인
> ```
> lsblk
> ```
> - 디스크의 이름을 확인합니다.
> <br />
>
> Block I/O 제한 및 부하 테스트
> ```
> docker run -it --rm --device-write-iops /dev/sda:10 ubuntu:latest /bin/bash
> dd if=/dev/zero of=file1 bs=1M count=10 oflag=direct
> ```
> - --device-write-iops를 적용하여 write 속도의 초당 quota를 제한합니다.
<br />

## 2. 컨테이너 모니터링
컨테이너의 정보를 실시간으로 모니터링합니다.
> 컨테이너 모니터링
> ```
> docker stats
> docker stats [컨테이너 이름]
> ```
> - 컨테이너 이름을 생략하면 전체 컨테이너,  이름을 입력하면 해당 컨테이너만 출력합니다.
<br />

## 3. cAdvisor
cAdvisor 모니터링 도구를 통해 실시간으로 모니터링 합니다.
> cAdvisor 설치
> - https://github.com/google/cadvisor 접속
> - Quick Start: Running cAdvisor in a Docker Container 아래에 설치 명령어를 우분투에 입력합니다.
> <br />
>
> 웹 브라우저로 8080포트에 접속합니다.
