# Docker - 설치

준비된 Ubuntu, CentOS VM에 Docker를 설치합니다.   
<br />

### Docker 설치
- 도커의 기술 문서를 참고하려면 다음 링크로 접속하세요.
- <a href="https://docs.docker.com">Docker Docs Link</a>
- Download and install >> Docker for Linux >> Install Docker Desktop on Ubuntu
<br />

##### Ubuntu
> 도커 설치 전 필요한 프로그램 프로그램 설치  
> - sudo apt-get update   
> - sudo apt-get install apt-transport-https ca-certificates curl gnupg-agent software-properties-common   
> <br />
> 
> 도커 인증서 저장   
> - curl -fsSl https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -   
> <br />
> 
> 도커 리포지토리 및 URL 등록   
> - sudo add-apt-repository "deb [arch-amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"   
> <br />
>
> 도커 설치   
> - sudo apt-get update   
> - sudo apt-get install docker-ce docker-ce-cli containerd.io   
> <br />
>
> 도커 설치 확인   
> - sudo docker version   
<br />

##### CentOS
> 루트 계정 접속   
> - su -
> <br />
> 
> 도커 설치 전 필요한 프로그램 프로그램 설치   
> - yum install -y yum-utils   
> <br />
> 
> 도커 리포지토리 및 URL 등록   
> - yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo   
> <br />
>
> 도커 설치   
> - yum install docker-ce docker-ce-cli containerd.io   
> <br />
> 
> 서비스 데몬 시작
> - systemctl start docker
> - systemctl enable docker (부팅 시 도커 데몬 실행)
> <br />
> 
> 도커 설치 확인   
> - docker version   
