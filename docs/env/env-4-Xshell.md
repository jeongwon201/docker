# 도커 환경 구축 - XShell
XShell을 설치하고 VM에 SSH 원격 접속을 시도합니다.

### XShell 설치
- https://www.netsarang.com/ko/xshell-download/ 에 접속해 XShell을 다운로드 합니다.
<br />

### XShell 설치
- 타 프로그램처럼 설치하면 됩니다.
<br />

### XShell 세션
- VM에 SSH 원격 접속을 위해 세션을 생성하고 설정합니다.

##### Ubuntu
> 세션 생성
> - 세션 만들기
> - 다음과 같이 설정
>   - 이름: docker-ubuntu
>   - 프로토콜: SSH
>   - 호스트: 127.0.0.1
>   - 포트 번호: 105
> <br />
> 
> - 사용자 인증 탭으로 이동
> - 다음과 같이 설정
>   - 사용자 이름: guru
>   - 암호: work
>   - 방법: Password
> <br />
>
> - [확인] 클릭   
<br />

##### CentOS
> 세션 생성
> - 세션 만들기
> - 다음과 같이 설정
>   - 이름: docker-centos
>   - 프로토콜: SSH
>   - 호스트: 127.0.0.1
>   - 포트 번호: 106
> <br />
> 
> - 사용자 인증 탭으로 이동
> - 다음과 같이 설정
>   - 사용자 이름: guru
>   - 암호: work
>   - 방법: Password
> <br />
>
> - [확인] 클릭
