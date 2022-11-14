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
> ```
> mkdir build
> cd build
> vim Dockerfile
> ```
> Dockerfile   
> > FROM debian   
> > MAINTAINER jeongwon   
> > RUN apt-get update; apt-get install stress -y   
> > CMD ["/bin/sh", "-c", "stress -c 2"]
> ```
> docker build -t stress .
> ```

1. 컨테이너 리소스 제한
2. 컨테이너 모니터링
3. cAdvisor
