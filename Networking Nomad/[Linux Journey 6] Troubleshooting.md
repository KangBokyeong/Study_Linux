# [Linux Journey 6] Troubleshooting
## 1. ICMP (Internet Control Message Protocol)
- TCP / IP 프로토콜 제품군의 일부로 업데이트 및 오류 메시지를 보내는데 사용
- 패킷 전달 실패와 같은 네트워크 문제를 디버깅하는데 매우 유용한 프로토콜
- 각 ICMP 메시지에는 유형, 코드 및 체크섬 필드가 있음. 
  - 유형 필드는 ICMP 메시지의 유형
  - 코드는 하위 유형
  - 메시지에 대한 자세한 정보 설명
  - 체크섬은 메시지의 무결성 문제를 감지하는데 사용
- 일반적인 ICMP 유형
  - Type 0: Echo 응답
  - Type 3: 목적지 도달 불가
  - Type 8: Echo 요청
  - Type 11: 시간 초과
- 패킷이 대상에 도착할 수 없으면 Type 3 ICMP 메시지가 생성되고 Type 3에는 목적지에 도달 할 수 없는 이유를 더 자세히 설명하는 16 개의 코드 값이 있음.
  - Code 0: 네트워크에 연결 불가
  - Code 1: 호스트에 연결 불가
  - 등등

- 이 메시지는 일부 네트워크 문제 해결 도구를 사용할 때 더 유용할 수 있음.

## 2. ping
- 패킷이 호스트에 도달 할 수 있는지 여부를 테스트하는데 사용
- ICMP 에코 요청 (유형 8) 패킷을 대상 호스트에 보내고 ICMP 에코 응답 (유형 0)을 기다림.
- 호스트가 요청 패킷을 보내고 대상에서 응답을 받을 때 성공함.
- 예시(ping을 사용하여 www.google.com 에 연결할 수 있는지 확인)
  - ping -c 3 www.google.com 명령  
  ![](https://user-images.githubusercontent.com/44868847/52542222-f5b95980-2de0-11e9-8aab-93a55c900fb3.PNG)  
    - -c 플래그 (count)는 카운트에 도달 한 후 에코 요청 패킷의 전송을 중지하는데 사용
    - 첫 번째 부분은 74.125.239.112 (google.com)에 64 바이트 패킷을 보내고 나머지는 우리에게 여행의 세부 정보를 보여줌.
    - 기본적으로 초당 패킷을 보냄.

### icmp_seq
- 전송된 패킷의 시퀀스 번호를 표시하는데 사용.
- 이 경우 3 개의 패킷을 전송했으며 3 개의 패킷이 다시 생성되었음을 알 수 있음.
- ping을 수행하고 일부 시퀀스 번호가 누락 된 경우 일부 연결 문제가 발생하고 모든 패킷이 통과하는 것은 아님.
- 순서 번호가 잘못된 경우 패킷이 1 초 기본값을 초과하므로 연결 속도가 매우 느릴 수 있음.

### ttl (Time To Live)
- 홉 카운터로 사용되며 홉을 만들 때 카운터를 하나씩 줄이고 홉 카운터가 0에 도달하면 패킷이 사라짐.
- 패킷에 수명을 주기위한 것.

### time  
- 에코 요청 패킷을 보낸 후 에코 응답을받는 데 걸린 왕복 시간

## 3. traceroute
- 패킷이 라우팅되는 방법을 확인하는데 사용
- TTL 값이 증가하는 패킷을 보내면 작동
- 첫 번째 라우터가 패킷을 가져오고 TTL 값을 1 씩 감소시켜 패킷을 삭제
- 라우터가 ICMP Time Exceeded 메시지를 다시 보내줌.
- 다음 패킷은 2의 TTL을 가지므로 첫 번째 라우터를 지나치게 만듬.
- 두 번째 라우터에 도달하면 TTL은 0이며 다른 ICMP 시간 초과 메시지를 반환함.
- 패킷을 보내거나 끊을 때 패킷이 통과하는 라우터의 목록을 작성하여 최종 목적지까지 도달하고 ICMP 에코 응답 메시지를 가져옴. 
- 예시
  - traceroute google.com 명령 사용  
  ![](https://user-images.githubusercontent.com/44868847/52542287-8f810680-2de1-11e9-9ccf-943a71ae23a6.PNG)  
    - 각 회선은 저와 저의 목표 사이에있는 라우터 또는 기계
    - 대상의 이름과 IP 주소를 보여 주며 마지막 세 열은 해당 라우터에 도착할 패킷의 왕복 시간에 해당.
    -  기본적으로 경로를 따라 3 개의 패킷을 보냄.
    
## 4. netstat
### Well Known Ports
- / etc / services 파일을 보고 잘 알려진 포트 목록 확인  
![](https://user-images.githubusercontent.com/44868847/52542407-ab38dc80-2de2-11e9-874b-5f110c5dfd2f.PNG)
  - 첫 번째 열은 서비스의 이름
  - 포트 번호와 서비스가 사용하는 전송 계층 프로토콜

### netstat
- 네트워크에 대한 자세한 정보를 얻는 매우 유용한 도구.
- 네트워크 연결, 라우팅 테이블, 네트워크 인터페이스에 관한 정보 등과 같은 다양한 네트워크 관련 정보를 표시
- 소켓과 포트
  - 소켓: 프로그램이 데이터를 보내고 받는 것을 허용하는 인터페이스
  - 포트: 어떤 응용 프로그램이 데이터를 보내거나 받아야하는지 식별하는데 사용
  - 소켓 주소는 IP 주소와 포트의 조합
- 호스트와 대상 간의 모든 연결에는 고유 한 소켓이 필요함.
  - 예: HTTP는 포트 80에서 실행되는 서비스, 많은 HTTP 연결을 가질 수 있으며 각 연결을 유지하기 위해 연결 당 소켓이 생성됨.
  - netstat -at 명령 사용  
  ![4-2](https://user-images.githubusercontent.com/44868847/52542408-ac6a0980-2de2-11e9-9de4-c784ef8d15c2.PNG)
    - netstat -a 명령: 네트워크 연결에 대한 청취 소켓과 청취 소켓을 표시
    - -t 플래그: tcp 연결만 표시
    
    - 열은 왼쪽에서 오른쪽으로 아래와 같음.
      - Proto: 사용된 프로토콜, TCP 및 UDP.
      - Recv-Q(Data that is queued to be received): 수신 대기중인 데이터
      - Send-Q(Data that is queued to be sent): 전송 대기중인 데이터
      - Local Address: 로컬로 연결된 호스트
      - Foreign Address: 원격으로 연결된 호스트
      - State: 소켓의 상태
    - 소켓 상태 목록
      - LISTENING: 소켓이 들어오는 연결을 듣고 있음. TCP 연결 시, 기다려야함을 기억.
      - SYN_SENT: 소켓이 연결을 시도하고 있음.
      - ESTABLISHED: 소켓에 연결이 설정되어 있음.
      - CLOSE_WAIT: 원격 호스트가 종료되었고 소켓이 닫힐 때까지 기다리고 있음.
      - TIME_WAIT: 네트워크에서 패킷을 처리하기 위해 소켓을 닫은 후에 대기 중
      
## 5. Packet Analysis
The subject of packet analysis could fill an entire course of its own and there are many books written just on packet analysis. However, today we will just learn the basics. There are two extremely popular packet analyzers, Wireshark and tcpdump. These tools scan your network interfaces, capture the packet activity, parse the packages and output the information for us to see. They allows us to get into the nitty gritty of network analysis and get into the low level stuff. We'll be using tcpdump since it has a simpler interface, however if you were to pick up packet analysis for your toolbelt, I would recommend looking into Wireshark.

### tcpdump 설치  
> sudo apt install tcpdump

### 인터페이스에서 패킷 데이터 캡처
- sudo tcpdump -i wlan0 명령 사용
  ![5-1](https://user-images.githubusercontent.com/44868847/52542701-e7ba0780-2de5-11e9-9556-57d10dce59e9.PNG)  
  - 패킷 캡처를 실행할 때 많은 일들이 일어나는 것을 보게 될 것임.
  - 백그라운드에서 많은 네트워크 활동이 일어날 것으로 예상됨.
  - www.google.com을 핑 (ping)하기로 결정한 시간에만 캡처한 내용을 촬영

### 출력물에 대한 이해
![5-2](https://user-images.githubusercontent.com/44868847/52542702-e7ba0780-2de5-11e9-86bd-8c0296d225f2.PNG)  
  - 첫 변째 필드: 네트워크 활동의 타임 스탬프
  - IP에는 프로토콜 정보가 들어 있음.
  - 소스 및 대상 주소가 표시. (icebox.lan> nuq04s29-in-f4.1e100.net)
  - seq: TCP 패킷의 시작 및 끝 시퀀스 번호.
  - length: 바이트 단위의 길이
- Google의 tcpdump 출력에서 볼 수 있듯이 www.google.com 으로 ICMP 에코 요청 패킷을 보내고 답례로 ICMP 에코 답장 패킷을 받고 있음.
- 다른 패킷이 다른 정보를 출력하므로 manpage를 참조하여 그 정보 확인

### tcpdump 출력을 파일에 쓰기
> sudo tcpdump -w /some/file
