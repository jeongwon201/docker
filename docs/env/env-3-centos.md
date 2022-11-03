# 도커 환경 구축 - CentOS 7

VM에 Cent OS 7 을 설치하고, 초기 설정을 진행합니다.   
<br />

### Cent OS 7 다운로드
- https://centos.org/ 에 접속해 Cent OS 7 버전을 다운로드 합니다.   
<br />

### VM에 CentOS Disk 파일 마운트
- OS 설치를 위해 다운로드받은 디스크 파일을 VM의 광학 드라이브에 추가합니다.   

> 디스크 설정   
> - docker-centos VM의 [설정]을 클릭 후 [저장소]로 이동   
> - 오른쪽 속성 탭의 광학 드라이브 우측 CD 모양 아이콘을 눌러 [디스크 파일 선택]을 클릭   
> - 다운로드받은 디스크 파일 선택 후 [열기(O)] 버튼 클릭   
> - [확인]을 눌러 VM 홈 화면으로 이동   
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

### CentOS 서버 구성
- CentOS 설치 후 도커 환경을 구축하기 위해 서버 구성을 진행합니다.   

> VM 내부 Hypervisor(KVM) 중지
> ※ CentOS는 가상 머신 내부에도 Hypervisor를 생성하기 때문에 중지해주어야 합니다.
> - 터미널 오픈  
> - systemctl stop libvirtd (중지)
> - systemctl disable libvirtd (다음 부팅 시 실행 안함)   
> <br />
>
> 네트워크 구성   
> ※ CentOS는 Ubuntu와 다르게 설치 시 네트워크를 설정했기 때문에 별도의 설정이 필요 없습니다.   
> <br />
> host 정보 추가   
> - 터미널 오픈
> - vi /etc/hosts   
> - 아래 내용 추가   
>   - 10.100.0.105  docker-ubuntu.example.com docker-ubuntu   
>   - 10.100.0.106  docker-centos.example.com docker-centos   
> <br />
>   

### CentOS 텍스트 부팅
- 텍스트 부팅으로 변경합니다.   

> 텍스트 부팅 설정   
> - 터미널 오픈
> - systemctl set-default multi-user.target   
<br />

### SSH Server 및 기타 툴 설치
- CentOS는 Ubuntu와 다르게 설치 시 openssh-server가 설치되어있고, 방화벽도 열려있기 때문에 별도의 설정이 필요 없습니다.   
> tree 설치
> - 터미널 오픈
> - yum install -y tree
<br />

### 구성 환경 스냅샷
- 지금까지 진행한 설정을 기억하여 언제든지 복구할 수 있게 스냅샷을 생성합니다.   

> 스냅샷 생성   
> - VM 종료   
> - docker-centos의 햄버거 아이콘 클릭 후 스냅샷   
> [찍기(T)] 클릭 후 이름과 설명 입력 후 확인   
