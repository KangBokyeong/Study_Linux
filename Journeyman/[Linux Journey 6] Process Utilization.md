# [Linux Journey 6] Process Utilization
## 1. Tracking processes: top
- top: 가장 중요한 것은 프로세스를 통해 시스템 활용도를 실시간으로 확인할 수 있는 툴이라는 점을 기억
  > top - 18:06:26 up 6 days,  4:07,  2 users,  load average: 0.92, 0.62, 0.59  
  >
  > Tasks: 389 total,   1 running, 387 sleeping,   0 stopped,   1 zombie  
  >
  > %Cpu(s):  1.8 us,  0.4 sy,  0.0 ni, 97.6 id,  0.1 wa,  0.0 hi,  0.0 si,  0.0 st  
  >
  > KiB Mem:  32870888 total, 27467976 used,  5402912 free,   518808 buffers  
  >
  > KiB Swap: 33480700 total,    39892 used, 33440808 free. 19454152 cached Mem  
  >
  > PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND                               
  >
  > 6675 patty    20   0 1731472 520960  30876 S   8.3  1.6 160:24.79 chrome                             
  >
  > 6926 patty    20   0  935888 163456  25576 S   4.3  0.5   5:28.13 chrome 

    - 첫 번째 줄: uptime 명령 실행 시, 볼 수 있는 것과 동일한 정보.
      - 필드 왼쪽에서 오른쪽으로 현재 시간, 시스템 가동 시간, 현재 얼마나 많은 사용자가 로그온하고 있는지, 시스템 부하 평균
    
    - 두 번째 줄: running, sleeping, stopped 그리고 zombied
    - 세 번째 줄: CPU 정보
      - us(user CPU time): 사용자 프로세스가 실행되지 않은 CPU 시간의 백분율
      - sy(system CPU time): 커널 및 커널 프로세스 실행에 소비 된 CPU 시간의 백분율
      - ni(nice CPU time): 정밀한 프로세스 실행에 소비 된 CPU 시간의 백분율
      - id(CPU idle time): 유휴 CPU 시간의 백분율
      - wa(I/O wait):  I / O를 기다리는 데 소비 된 CPU 시간의 백분율. (값이 낮으면 문제는 디스크 또는 네트워크 I/O가 아닐 수 있음.)
      - hi(hardware interrupts): 하드웨어 인터럽트를 처리하는 데 소비 된 CPU 시간의 백분율
      - si(software interrupts): 소프트웨어 인터럽트를 처리하는 데 소비 된 CPU 시간의 백분율
      - st(steal time): 상 컴퓨터를 실행하는 경우 다른 작업을 위해 도난당한 CPU 시간의 백분율
    - 네 번째 줄 및 다섯 번째 줄: 메모리 사용 및 스왑 사용
    - 현재 사용중인 프로세스 목록
      - PID: 프로세스의 ID
      - USER: 프로세스의 소유자 인 사용자
      - PR: 프로세스의 우선 순위
      - NI: 좋은 값
      - VIRT: 프로세스에서 사용하는 가상 메모리
      - RES: 프로세스에서 사용 된 실제 메모리
      - SHR: 프로세스의 공유 메모리
      - S: 프로세스 상태(S=sleep, R=running, Z=zombie, D=uninterruptible, T=stopped)
      - %CPU: 이 프로세스에서 사용하는 CPU의 비율
      - %MEM: 이 프로세스에서 사용 된 RAM의 백분율
      - TIME+: 이 프로세스의 총 활동 시간
      - COMMAND: 프로세스 이름

- 특정 프로세스를 추적하려는 경우에도 프로세스 ID를 지정 가능
  > top -p 1

## 2. lsof and fuser

### lsof
- 파일이 텍스트 파일, 이미지 등이 아니라 시스템, 디스크, 파이프, 네트워크 소켓, 장치 등의 모든 것을 의미
- 프로세스에서 사용 중인 항목을 보려면 lsof 명령 사용
- 명령 사용 시 아래와 같은 결과가 출력 됨.
  > COMMAND    PID  USER   FD   TYPE DEVICE SIZE/OFF NODE NAME  
  > 
  > lxsession 1491 pete  cwd    DIR    8,6     4096  131 .  
  >
  > update-no 1796 pete  cwd    DIR    8,6     4096  131 .  
  >
  > nm-applet 1804 pete  cwd    DIR    8,6     4096  131 .  
  > 
  > indicator 1809 pete  cwd    DIR    8,6     4096  131 .  
  > 
  > xterm     2205 pete  cwd    DIR    8,6     4096  131 .  
  > 
  > bash      2207 pete  cwd    DIR    8,6     4096  131 .  
  > 
  > lsof      5914 pete  cwd    DIR    8,6     4096  131 .  
  > 
  > lsof      5915 pete  cwd    DIR    8,6     4096  131 .  
    
    - 어떤 프로세스가 현재 장치 및 파일을 열어두고 있는지 확인 가능

### fuser
- 프로세스를 추적하는 또 다른 방법
- "file user"의 약자
- 파일이나 파일 사용자를 사용하는 프로세스에 대한 정보를 보여줌.
- fuser 명령 사용 시, 아래와 같은 결과 출력됨.(정확한 명령: fuser -v .)
  >                      USER        PID ACCESS COMMAND     
  >   /home/pete:        pete  1491 ..c.. lxsession  
  >   
  >                      pete  1796 ..c.. update-notifier  
  >   
  >                      pete  1804 ..c.. nm-applet  
  >   
  >                      pete  1809 ..c.. indicator-power  
  >   
  >                      pete  2205 ..c.. xterm  
  >   
  >                      pete  2207 ..c.. bash  

    - 현재 /home/pete 디렉토리를 사용하는 프로세스 확인 가능
     
## 3. Process Threads
- 동일한 프로그램을 실행하는데 사용되므로 흔히 경량 프로세스라고 함.
- 스레드의 종류 두 가지
  - 단일 스레드: 하나의 스레드
  - 다중 스레드: 하나 이상의 스레드
- 모든 프로세스에는 적어도 하나의 스레드가 존재.
- 스레드는 서로 간에 자체 격리 된 시스템 리소스를 쉽게 공유할 수 있으므로 서로 통신하기가 쉬워져 다중 스레드 응용 프로그램을 사용하는 것이 효율적.
- 프로세스 스레드를 보기위한 명령 사용 시 아래와 같은 결과 출력됨. (명령: ps m)

  |  PID TTY    |  STAT  | TIME COMMAND | 
  |:-----:|:-----:|:-----:|
  | 2207 pts/2 |   -    |  0:01 bash |
  |   - -     |   Ss  |   0:01 - |  
  | 5252 pts/2  |  -  |    0:00 ps m | 
  |    - -   |     R+   |  0:00 -  |
    - 프로세스는 각 PID로 표시되며 프로세스 아래에는 스레드 (a로 표시)가 있음.

## 4. CPU Monitoring
- uptime 명령어 입력 시, 아래와 같은 결과 출력
  > 17:23:35 up 1 day,  5:59,  2 users,  load average: 0.00, 0.02, 0.05

- 로드 평균은 시스템의 CPU로드를 확인하는 좋은 방법
- 로드 평균의 숫자는 1분, 5분 및 15분 간격으로 평균 CPU로드를 나타냄.
- CPU로드가 의미하는 바: CPU로드는 CPU가 실행을 대기하는 평균 프로세스 수
- 부하 평균 관찰 시, 코어 수 고려하기!

## 5. I/O Monitoring
- CPU 사용량을 모니터링하고 디스크 사용량을 모니터링하기
- iostat 명령 사용 시, 아래와 같은 결과 출력됨.
  > Linux 3.13.0-39-lowlatency (icebox)     01/28/2016      _ i686 _  (1 CPU)  
  > 
  > 
  > avg-cpu:  %user   %nice %system %iowait  %steal   %idle  
  > 
  >          0.13    0.03    0.50    0.01    0.00   99.33  
  > 
  > 
  > 
  > Device:            tps    kB_read/s    kB_wrtn/s    kB_read    kB_wrtn  
  > 
  > sda               0.17         3.49         1.92     385106     212417  


    - 첫 번째 부분: CPU 정보

      - %user: 사용자 수준 (응용 프로그램)에서 실행하는 동안 발생한 CPU 사용률 백분율 표시
      - %nice: 좋은 우선 순위를 가진 좋은 priority.user CPU 사용률로 사용자 레벨에서 실행하는 동안 발생한 CPU 사용률 백분율 표시
      - %system: 시스템 레벨 (커널)에서 실행하는 동안 발생한 CPU 사용률 표시
      - %iowait: 시스템에 미해결 디스크 I / O 요청이 있었던 동안 CPU가 유휴 상태였던 시간의 백분율 표시
      - %steal: 하이퍼 바이저가 다른 가상 프로세서를 서비스하는 동안 가상 CPU가 비자발적으로 대기 한 시간의 백분율 표시
      - %idle: CPU 또는 CPU가 유휴 상태 였고 시스템에 미해결 디스크 I / O 요청이 없었던 시간의 백분율을 표시
    
    - 두 번째 부분: 디스크 사용률

      - tps: 치에 발행 된 초당 전송 수. 전송은 장치에 대한 I / O 요청이며 불확정 크기. 여러 논리적 요청을 하나의 I / O 요청으로 결합하여 장치에 연결 가능.
      - kB_read/s: 장치에서 읽은 데이터의 양을 초당 킬로바이트 단위로 나타냄.
      - kB_wrtn/s: 초당 킬로바이트로 표시되는 장치에 쓰여진 데이터의 양을 나타냄.
      - kB_read: 읽은 총 킬로바이트의 수 
      - kB_wrtn: 쓴 총 킬로바이트의 

## 6. Memory Monitoring
- vmstat 명령을 사용하여 메모리 사용 모니터링
- 명령 사용시 아래와 같은 결과 출력됨.
  > procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----  
  > 
  > r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st  
  > 
  > 1  0      0 396528  38816 384036    0    0     4     2   38   79  0  0 99  0  0  

- 위 결과에서 필드 별 설명
  - procs 
    - r: 실행 시간에 대한 프로세스 수
    - b: 터럽트가 발생하지 않는 프로세스의 프로세스 수

  - memory 
    - swpd: 사용된 가상 메모리의 양
    - free: 사용 가능한 메모리 양
    - buff: 버퍼로 사용된 메모리 양
    - cache: 캐시로 사용된 메모리 양
    
  - swap 
    - si: 디스크에서 스왑된 메모리 양
    - so: 디스크로 스왑 아웃된 메모리 양

  - io 
    - bi: 블록 장치로부터 수신된 블록의 양
    - bo: 블럭 장치로 보내지는 블럭의 양

  - system 
    - in: 초당 인터럽트 수
    - cs: 초당 컨텍스트 전환 수
    
  - cpu 
    - us: 사용자 시간에 소요 된 시간
    - sy: 커널 시간에 소비 된 시간
    - id: 유휴 시간
    - wa: 입출력 대기 시간

## 7. Continuous Monitoring
### sar 설치
- 시스템에서 이력 분석을 수행하는 데 사용되는 도구
- 먼저 sysstat 패키지 sudo apt install sysstat를 설치하여 설치했는지 확인

### 데이터 수집 설정
- 일반적으로 sysstat을 설치하면 / etc / default / sysstat의 ENABLED 필드를 수정하여 시스템에서 자동으로 데이터 수집 시작

### sar 사용하기
- 하루의 시작부터 세부 정보 나열
  > sudo sar -q

- 하루의 시작부터 메모리 사용에 대한 세부 정보 표시
  > sudo sar -r

- CPU 사용량의 세부 정보 표시
  > sudo sar -P

- 다른 날의 보기를 보려면 / var / log / sysstat / saXX로 이동(XX 보려는 잘짜)
  > sar -q /var/log/sysstat/sa02

## 8. Cron Jobs
- 작업을 예약하는데 사용되는 깔끔한 도구
- 스크립트를 실행하고 실행하게 할 시간 지정 가능
- 예를 들어, / home / pete / scripts / change_wallpaper에있는 스크립트가 있다고 가정
  > 30 08 * * * /home/pete/scripts/change_wallpaper 
    - 필드를 왼쪽에서 오른쪽으로 살펴 보면
      - 분(0-59): 30
      - 시간(0-23): 08
      - 일(1-31): *
      - 월(1-12): *
      - 요일(0-7)[0 과 7은 일요일로 표시됨.]: *
      - 필드의 별표는 모든 값을 일치시키는 것을 의미
      - 매일 8시 30분에 매월 이 프로그램을 실행하고 싶어함.
- cornjob 만들려면 crontab 파일 편집하기     
  > crontab -e
