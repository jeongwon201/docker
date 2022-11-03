# 도커 환경 구축 - Ubuntu 20.04 LTS

VM에 Ubuntu 20.04 LTS 버전을 설치하고, 초기 설정을 진행합니다.   
<br />

### Ubuntu 20.04 LTS 다운로드
- https://ubuntu.com/ 에 접속해 Ubuntu 20.04 LTS 버전을 다운로드 합니다.   
<br />

### VM에 Ubuntu Disk 파일 마운트
- OS 설치를 위해 다운로드받은 디스크 파일을 VM의 광학 드라이브에 추가합니다.   

> docker-ubuntu VM의 [설정]을 클릭 후 [저장소]로 이동합니다.   
> 오른쪽 속성 탭의 광학 드라이브 우측 CD 모양 아이콘을 눌러 [디스크 파일 선택]을 클릭합니다.   
> 다운로드받은 디스크 파일 선택 후 [열기(O)] 버튼을 누릅니다.   
> [확인]을 눌러 VM 홈 화면으로 이동합니다.   
<br />

### VM 실행
- OS 설치를 위해 VM을 실행합니다.   

> [시작] 버튼을 클릭하여 VM을 실행합니다.   
<br />

### Ubuntu 설치
- OS를 설치합니다.

> - 언어 선택 후 [Ubuntu 설치]를 클릭합니다.   
> - 키보드 레이아웃 선택 후 [계속하기]를 클릭합니다.   
> - [일반 설치]와 [Ubuntu 설치 중 업데이트 다운로드] 체크 후 [계속하기]를 클릭합니다.   
> - [디스크를 지우고 Ubuntu 설치] 선택 후 [지금 설치(I)]를 클릭합니다.   
> - 포맷 팝업 창에서 [계속하기]를 클릭합니다.   
> - 타임존 설정에서 지도 상 대한민국을 클릭하여 [Seoul]로 설정 후 [계속하기]를 클릭합니다.   
> - 계정 설정을 다음과 같이 설정 후 [계속하기]를 클릭합니다.   
>   - 이름: guru   
>   - 컴퓨터 이름: docker-ubuntu.example.com   
>   - 사용자 이름 선택: guru   
>   - 암호 선택: work   
>   - [로그인할 때 암호입력] 체크   
> - 설치 완료 후 재부팅합니다.   

※ 처음 우분투 설치 시 버튼이 안보이는 현상   
- 설치 종료 > 임시 작업 환경에서 setting > displays > resolution 변경 > 바탕화면의 Install Ubuntu 실행   
<br />

### Ubuntu 서버 구성
- Ubuntu 설치 후 도커 환경을 구축하기 위해 서버 구성을 진행합니다.   

> 로그인   
> - guru / work 로 로그인 합니다.   
> <br />
>
> DHCP 방식의 네트워크를 수동 네트워크로 변경합니다.   
> - 설정의 네트워크 탭으로 이동합니다.   
> - IPv4 탭 클릭 후 다음과 같이 설정합니다.   
>   - IPv4 방식: 수동   
>   - 주소   
>     - 주소: 10.100.0.105   
>     - 네트마스크: 24   
>     - 게이트웨이: 10.100.0.1   
>   - 네임서버(dns): 10.100.0.1   
> <br />
>
> 터미널 오픈 후 hostname을 변경합니다.   
> - vi /etc/hostname   
> - docker-ubuntu.example.com 로 변경 후 저장   
> <br />
>
> host 정보를 추가합니다.   
> - vi /etc/hosts   
> - 아래 내용 추가   
>   - 10.100.0.105  docker-ubuntu.example.com docker-ubuntu   
>   - 10.100.0.106  docker-centos.example.com docker-centos   
> <br />
>
### Ubuntu 루트 계정 설정
- Ubuntu는 root 계정을 설정하지 않으면 root 계정을 이용하지 못하기 때문에 root 계정 설정을 진행합니다.   

> root 계정 비밀번호를 설정합니다.   
> - 터미널 오픈   
> - sudo passwd root   
> - 새 암호로 password 입력   
> <br />
>
> root 계정으로 전환합니다.   
> - su - root   
> - 암호 입력   
<br />

### Ubuntu 텍스트 부팅
- 텍스트 부팅으로 변경합니다.   

> 텍스트 부팅을 설정합니다.   
> - systemctl set-default multi-user.target   
<br />

### SSH Server 설치
- XShell을 이용해 VM에 원격으로 접속하기위해 SSH Server의 설치를 진행합니다.   

> openssh-server 및 기타 툴 설치   
> - 터미널 오픈   
> - apt-get update   
> - apt-get install -y openssh-server curl vim tree   
<br />

### 구성 환경 스냅샷
- 지금까지 진행한 설정을 기억하여 언제든지 복구할 수 있게 스냅샷을 생성합니다.   

> 스냅샷을 생성합니다.   
> - VM 종료   
> - docker-ubuntu의 햄버거 아이콘 클릭 후 스냅샷   
> [찍기(T)] 클릭 후 이름과 설명 입력 후 확인   
