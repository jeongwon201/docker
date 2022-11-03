# 도커 환경 구축 - CentOS 7

VM에 Cent OS 7 을 설치하고, 초기 설정을 진행합니다.   
<br />

### Cent OS 7 다운로드
- https://centos.org/ 에 접속해 Cent OS 7 버전을 다운로드 합니다.   
<br />

### VM에 CentOS Disk 파일 마운트
- OS 설치를 위해 다운로드받은 디스크 파일을 VM의 광학 드라이브에 추가합니다.   

> docker-centos VM의 [설정]을 클릭 후 [저장소]로 이동   
> 오른쪽 속성 탭의 광학 드라이브 우측 CD 모양 아이콘을 눌러 [디스크 파일 선택]을 클릭   
> 다운로드받은 디스크 파일 선택 후 [열기(O)] 버튼 클릭   
> [확인]을 눌러 VM 홈 화면으로 이동   
<br />

### VM 실행
- OS 설치를 위해 VM을 실행합니다.   

> [시작] 버튼을 클릭하여 VM을 실행   
<br />

### CentOS 설치
- OS를 설치합니다.

> OS 설치
> - 언어 선택 후 [Continue]
> - DATE & TIME
>   - 서울 선택 후 [Done]
> - SOFTWARE SELECTION
>   - GNOME Desktop 체크 후 [Done]
> - INSTALLATION DESTINATION
>   - (자동 설정됨)[Done]
> - NETWORK & HOST NAME
>   - Ethernet (enp0s3) 켜기
>   - Host name: docker-centos.example.com 입력 후 [Apply]
>   - Configure의 IPv4 Settings, 아래와 같이 설정 후 [Save]
>     - Method: Manual
>     - IP: 10.100.0.106
>     - Netmask: 24
>     - Gateway: 10.100.0.1
>     - DNS: 10.100.0.1
>   - [Done]
> - [Begin Installation] 버튼
> - ROOT PASSWORD
>   - Root Password: password 입력 후 [Confirm]
>   - [Done]
> - USER CREATION
>   - Full name: guru
>   - Password: work
> <br />
>
> - 설치 완료 후 Rebbot
> <br />
>
> - LICENSE INFORMATION
>   - I accept 체크 후 [Done]
> - [FINISH CONFIGURATION] 버튼
> <br />
>
> - 재부팅 후 나오는 Configuration은 모두 Next 하거나 Skip
<br />

※ 처음 우분투 설치 시 버튼이 안보이는 현상   
- 설치 종료 > 임시 작업 환경에서 setting > displays > resolution 변경 > 바탕화면의 Install Ubuntu 실행   
<br />

### Ubuntu 서버 구성
- Ubuntu 설치 후 도커 환경을 구축하기 위해 서버 구성을 진행합니다.   

> 로그인   
> - guru / work 로 로그인 합니다.   
> <br />
>
> DHCP 방식의 네트워크를 수동 네트워크로 변경
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
> hostname 변경   
> - 터미널 오픈   
> - vi /etc/hostname   
> - docker-ubuntu.example.com 로 변경 후 저장   
> <br />
>
> host 정보 추가   
> - 터미널 오픈
> - vi /etc/hosts   
> - 아래 내용 추가   
>   - 10.100.0.105  docker-ubuntu.example.com docker-ubuntu   
>   - 10.100.0.106  docker-centos.example.com docker-centos   
> <br />
>
### Ubuntu 루트 계정 설정
- Ubuntu는 root 계정을 설정하지 않으면 root 계정을 이용하지 못하기 때문에 root 계정 설정을 진행합니다.   

> root 계정 비밀번호 설정
> - 터미널 오픈   
> - sudo passwd root   
> - 새 암호로 password 입력   
> <br />
>
> root 계정 전환   
> - su - root   
> - 암호 입력   
<br />

### Ubuntu 텍스트 부팅
- 텍스트 부팅으로 변경합니다.   

> 텍스트 부팅 설정   
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

> 스냅샷 생성   
> - VM 종료   
> - docker-ubuntu의 햄버거 아이콘 클릭 후 스냅샷   
> [찍기(T)] 클릭 후 이름과 설명 입력 후 확인   
