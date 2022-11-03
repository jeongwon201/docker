# 도커 환경 구축 - VM
Ubuntu와 CentOS 위에 도커 환경을 구축하기 위해 VM을 설정합니다.     

### Virtual Box 설치
- https://www.virtualbox.org/ 링크 접속 후 OS에 맞는 버전을 설치합니다.   
<br />

### Virtual Box 네트워크 구성
- Ubuntu와 CentOS에 원격으로 접속 가능하도록 NAT 네트워크를 구성합니다.   
> 파일(F) >> 환경 설정 >> 네트워크 >> 추가   
>    
> 추가된 NAT 네트워크를 더블 클릭하여 다음과 같이 변경합니다.   
>    
> - 네트워크 이름: localNetwork   
> - 네트워크 CIDR: 10.100.0.0/24   
> - DHCP 지원: 활성화(체크)   
> - IPv6 지원: 비활성화(체크 해제)   
>    
> 포트 포워딩을 클릭합니다.   
> 
