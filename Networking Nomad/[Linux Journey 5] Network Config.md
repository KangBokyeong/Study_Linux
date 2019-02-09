# [Linux Journey 5] Network Config
## 1. Network Interfaces
- 커널이 네트워킹의 소프트웨어 측면을 하드웨어 측면에 연결하는 방법
- ifconfig -a 사용 시 아래와 같은 결과 출력
  ![](https://user-images.githubusercontent.com/44868847/52522727-7e92a100-2ccc-11e9-98a5-be94c478a8e8.PNG)

### ifconfig 명령
- ifconfig 툴을 사용하면 네트워크 인터페이스 구성 가능.
- 네트워크 인터페이스가 설정되어 있지 않으면 커널의 장치 드라이버와 네트워크가 서로 대화하는 방법을 알지 못할 것임.
- bootup에서 실행됨.
- config files를 통해 인터페이스 구성. 수동으로도 수정 가능.
- ifconfig의 출력은 왼쪽의 인터페이스 이름을 나타내고 오른쪽은 상세 정보를 보여줌.
- 루프백 인터페이스는 당신의 컴퓨터를 나타내는데 사용됨. (디버깅이나 로컬로 실행되는 서버에 연결하는데 좋음.)

- 인터페이스의 상태는 올리거나 내릴 수 있음.
- ifconfig 출력에서 가장 많이 볼 수있는 필드: HWaddr (인터페이스의 MAC 주소), inet 주소 (IPv4 주소) 및 inet6 (IPv6 주소)
- / etc / network / interfaces에서 인터페이스 정보 확인 가능


### 인터페이스를 생성하고 불러 오려면
> ifconfig eth0 192.168.2.1 netmask 255.255.255.0 up  
  - eth0 인터페이스에 IP 주소와 넷 마스크가 할당되고 차례대로 표시됨.
  
### 인터페이스를 가져 오거나 내리는 방법
> ifup eth0  
> ifdown eth0  

### ip 명령
- 시스템의 네트워킹 스택 조작 가능
- 사용중인 배포본에 따라 네트워크 설정을 조작하는 기본 방법이 될 수 있음.

### 모든 인터페이스에 대한 인터페이스 정보를 표시하려면 
> ip link show  

### 인터페이스의 통계를 표시하려면 
> ip -s link show eth0  

### 인터페이스에 할당 된 IP 주소를 표시하려면 
> ip address show  

### 인터페이스를 위아래로 가져 오려면 
> ip link set eth0 up  
> ip link set eth0 down  

### 인터페이스에 IP 주소를 추가하려면 
> ip address add 192.168.1.1/24 dev eth0  


## 2. route
### route 추가 및 삭제
- 새 route 추가
  > sudo route add -net 192.168.2.1/23 gw 10.11.12.3  
- route 삭제
  > sudo route del -net 192.168.2.1/23  

### ip 명령을 사용하여 route 추가 및 삭제
- route 추가 
  > ip route add 192.168.2.1/23 via 10.11.12.3  
- route 삭제
  - 방법1
    > ip route delete 192.168.2.1/23 via 10.11.12.3  
  - 방법2
    > ip route delete 192.168.2.1/23  

## 3. dhclient
- dhclient.leases 파일에서 dhclient.conf를 읽은 후 시스템 재부팅을 통해 dhclient가 임대 목록을 추적하면 dhclient.leases 파일이 읽혀서 이미 할당 된 임대가 무엇인지 알려줌.

### 새로운 IP를 얻으려면
> sudo dhclient  

## 4. Network Manager
- 네트워크 관리자
- 네트워크를 자동으로 구성
- 시작할 때 NetworkManager는 네트워크 하드웨어 정보를 수집하고 무선, 유선 등의 연결을 검색한 다음 이를 활성화 함.
- NetworkManager와 상호 작용하는 명령 줄 도구: nm-tool, nmcli

### nm-tool
- NetworkManager의 상태와 장치 보고
- nm-tool 명령 사용시 아래와 같은 결과 출력
  ![](https://user-images.githubusercontent.com/44868847/52522728-805c6480-2ccc-11e9-925d-d6865f6b87fb.PNG)

### nmcli
- NetworkManager의 제어 및 수정 가능

## 5. arp
- ARP를 사용하여 MAC 주소를 조회 할 때 먼저 로컬 시스템에 저장된 ARP 캐시를 확인하면 실제로 아래와 같이 이 캐시를 볼 수 있음.
  ![](https://user-images.githubusercontent.com/44868847/52522729-82262800-2ccc-11e9-8b9a-0e3dc819156e.PNG)

- 컴퓨터가 부팅 시, ARP 캐시는 실제로 비어 있음.
- 패킷이 다른 호스트로 전송 될 때 ARP 캐시가 채워짐.
- ip 명령을 통한 arp 캐시 확인 가능
  > ip neighbour show  

### ARP 캐시에없는 대상에 패킷을 보내면 다음과 같은 결과가 발생
1. 원본 호스트는 ARP 요청 패킷을 사용하여 이더넷 프레임을 만듬.
2. 원본 호스트는이 프레임을 전체 네트워크로 브로드 캐스트함.
3. 네트워크의 호스트 중 하나가 올바른 MAC 주소를 알고 있으면 MAC 주소가 포함 된 응답 패킷과 프레임을 전송함.
4. 원본 호스트는 MAC 주소 매핑에서 IP를 ARP 캐시에 추가 한 다음 패킷 전송을 계속함.

