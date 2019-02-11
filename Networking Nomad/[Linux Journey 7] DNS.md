# [Linux Journey 7] DNS
## 1. What is DNS (Domain Name System)?
- 사람이 IP 주소 대신 이름으로 웹 사이트 및 호스트를 추적 가능.
- 근본적으로 IP 주소에 대한 호스트 이름의 분산 데이터베이스.

## 2. DNS Components
- DNS의 구성 요소
### Name Server
- 이를 통해 DNS 설정 
- DNS 설정 로드
- "누가 google.com입니까?"와 같은 것을 알고 싶어하는 클라이언트 또는 다른 서버의 질문에 응답
- 네임 서버가 그 쿼리에 대한 답을 모를 경우 다른 네임 서버로 리다이렉트함.
- 사용자가 찾고있는 실제 DNS 레코드를 보유하고있는 "권위있는"서버이거나 DNS 레코드가 포함 된 권한있는 서버를 찾을 때까지 다른 서버에 요청하는 "재귀 적"메시지를 의미
  - 재귀 서버는 권한있는 서버에 도달하는 대신 캐시 된 정보 보유 가능

### Zone File
- 네임 서버 내부에 존재
- 네임 서버가 도메인에 관한 정보를 저장하는 방법이나 알지 못하는 경우 도메인에 접속하는 방법

### Resource Records
- 영역 파일은 리소스 레코드의 항목으로 구성됨.
- 각 행은 레코드이며 호스트, 이름 서버, 기타 자원 등에 대한 정보를 포함함.
- 필드는 아래와 같이 구성됨.
  - Record name: 레코드 이름
  - TTL: DNS TTL에서 레코드를 버리고 새 레코드를 얻은 시간은 시간으로 표시됨. 레코드의 TTL은 1 시간. 
  - Class: 레코드 정보의 네임 스페이스. 가장 일반적으로 IN은 인터넷에 사용됨.
  - Type: 레코드 데이터에 저장된 정보의 유형. 
  - Data: 레코드 유형에 따라 A 레코드이거나 다른 경우 IP 주소 포함 가능.  

![2-1](https://user-images.githubusercontent.com/44868847/52573889-70748a00-2e5e-11e9-9c13-d5c7b3fa774b.PNG)  

## 3. DNS Process
- 호스트가 DNS를 사용하여 도메인 (catzontheinterwebz.com)을 찾는 방법에 대한 예 살펴 보기.

### Local DNS Server
- 먼저 우리의 호스트는 "catzontheinterwebz.com은 어디에 있습니까?"라고 물음.
- 로컬 DNS 서버는 알지 못하므로 루트 서버에 요청하기 위해 깔때기의 상단부터 시작


### Root Servers
- 인터넷을 위한 루트 서버가 13개 있고, 인터넷에 대한 DNS 요청을 처리하기 위해 미러링되고 전세계에 배포됨.
- 실제로 수백 개의 서버가 작동하고 있으며, 그들은 서로 다른 조직에 의해 제어되고 최상위 도메인에 대한 정보를 포함하고 있음.
- 대부분의 사용자는 ISP가 제공하는 재귀적 DNS 서버와 직접 대화하여 catzontheinterwebz.com의 위치를 찾는 작업을 수행해야 함.
최상위 도메인은 .org, .com, .net 등의 주소로 알고 있을 것임.
- 루트 서버는 catzontheinterwebz.com이 어디에 있는지 모르기 때문에, 그것이 우리에게 주는 IP 주소로 .com 최상위 도메인 DNS 서버에 물어보는 것을 알려줌. 

### Top-Level Domain
- ".com" 주소를 알고 있는 이름 서버에 또 다른 요청을 보내서 catzontheinterwebz.com이 어디에 있는지 물어봄.
- 영역 파일에 catzontheinterwebz.com이 없지만 catzontheinterwebz.com.의 이름 서버에 대한 기록을 봄.
- 우리에게 그 이름 서버의 IP 주소를 알려주고 그곳을 찾아보라고 말해줌.

### Authoritative DNS Server
- 우리가 원하는 기록을 가지고 있는 DNS 서버에 최종 요청을 보냄.
- 네임 서버는 catzontheinterwebz.com에 대한 영역 파일을 가지고 있으며, 이 호스트에 대한 'www'에 대한 자원 기록이 있다고 봄.
- 우리에게 이 호스트의 IP 주소를 주고 우리는 마침내 인터넷에서 그 사이트에 접속할 수 있음.

## 4. /etc/hosts
- 우리 머신이 DNS를 눌러 쿼리를 하기 전에 먼저 우리 머신에서 로컬을 조회함.

### /etc/hosts
- 일부 호스트 이름과 IP 주소의 매핑이 포함되어 있음.
- 필드에는 IP 주소, 호스트 이름, 호스트의 별명이 있음.
- cat /etc/hosts 명령 사용  
  ![](https://user-images.githubusercontent.com/44868847/52574533-de6d8100-2e5f-11e9-9e8e-f9171ebfb15a.PNG)
    - 일반적으로 이 파일에 기본값으로 나열된 로컬 호스트 주소를 볼 수 있음.
    - /etc/hosts.deny 또는 /etc/hosts.allow 파일을 수정하여 호스트에 대한 액세스 관리 가능
    - (그러나, 보안에 신경을 쓰는 사람이라면 실제로 갈 길이 아니며 대신 방화벽 규칙을 수정해야 함.)
    
- 또 다른 예시(파일을 수정하고 다음 행을 추가해 보기)  
  ![](https://user-images.githubusercontent.com/44868847/52574535-de6d8100-2e5f-11e9-9b5d-e27233b20174.PNG
    - 파일을 저장하고 www.google.com으로 이동헤 보면,
    - (문제가 생길 것임.)
    - 그건 우리가 www.google.com을 완전히 잘못된 IP 주소로 매핑했기 때문임.
    - 우리의 호스트는 처음에 IP 주소 매핑을 로컬로 찾기 때문에, google.com을 찾기 위해 DNS에 도달하지 않음.

### /etc/resolv.conf
- 수동으로 관리되지 않음.
- DNS 이름 서버 매핑을 관리하려면 배포 별 설정 참조  

![](https://user-images.githubusercontent.com/44868847/52574536-df061780-2e5f-11e9-9622-cd303e3b7df6.PNG)



## 5. DNS Setup
- Linux와 함께 사용할 인기있는 DNS 서버의 빠른 비교 목록
- 완전한 목록은 아니지만, 만약 당신의 DNS 서버를 설정하고 있다면 어디에서 찾아야 하는지에 대한 아이디어를 줄 것임.


### BIND (Berkeley Internet Name Domain)
- 인터넷에서 가장 인기있는 DNS서버는 리눅스 배포에 사용되는 표준.
- 모든 기능을 갖춘 힘과 유연성이 필요하다면 사용.

### DNSmasq
- 경량이며 BIND보다 훨씬 쉽게 구성 가능.
- 단순성을 원하고 BIND의 모든 종소리와 휘파람을 필요로하지 않는다면 사용.
- DHCP 및 DNS 설정에 필요한 모든 도구가 포함되어 있으므로 소규모 네트워크에 적합.

### PowerDNS
- 완전한 기능을 갖춘 BIND와 유사하게 옵션을 통해 조금 더 유연하게 사용 가능.
- MySQL, PostgreSQL 등과 같은 여러 데이터베이스에서 정보를 읽어 보다 쉽게 관리 가능


## 6. DNS Tools
### nslookup (name server lookup)
- 이름 서버를 조회하여 자원 레코드에 관한 정보를 찾을 때 사용.
- google.com의 이름 서버가 어디에 있는지 알아보려면, nslookup www.google.com 명령 사용  
  ![](https://user-images.githubusercontent.com/44868847/52575569-c39c0c00-2e61-11e9-9276-7124a63ccef8.PNG)

### dig (domain information groper)
- DNS 이름 서버에 대한 정보를 얻는 강력한 도구
- nslookup보다 유연하고 DNS 문제 해결에 좋음.
- dig www.google.com 명령 사용  
  ![](https://user-images.githubusercontent.com/44868847/52575567-c39c0c00-2e61-11e9-8515-9e49a0f0c4f1.PNG)
