# [Linux Journey 5] Init
## 1. System V Overview
- 주요 목적: 시스템에서 필수 프로세스를 시작하고 중지하는 것
- 세 가지 주요 구현: System V, Upstart 및 systemd (Linux에서)
- 가장 전통적인 버전 살펴보기
- Sys V는 순차적으로 프로세스를 시작하고 멈춤.
- foo-b가 작동하기 훨씬 전에 foo-a라는 서비스를 시작하려면 foo-a가 이미 실행 중인지 확인해야함
- 크립트를 통해 이러한 스크립트가 서비스를 시작하고 중지하며, 자체 스크립트를 작성하거나 대부분의 경우 운영 체제에 이미 구축되어 필수 서비스를 로드하는 데 사용되는 스크립트를 사용할 수 있다고 함.
- 장점: 의존성을 해결하는 것이 상대적으로 쉬움.
- 단점: 한 번에 한 가지가 시작되거나 중지되기 때문에 성능이 좋지 않음.
- 시스템 V를 사용할 때 시스템 상태는 0에서 6까지 설정된 런레벨에 의해 정의됨.
  - 0: 셧다운
  - 1: 단일 사용자 모드
  - 2: 네트워킹이 없는 다중 사용자 모드
  - 3: 네트워킹이 있는 다중 사용자 모드
  - 4: 미사용
  - 5: 네트워킹 및 GUI가 있는 다중 사용자 모드
  - 6: 재부팅

- 시스템이 시작되면, 자신이 속한 런레벨을보고 그 런레벨 구성 안에있는 스크립트 실행
- 스크립트는  /etc/rc.d/rc[runlevel number].d/ 또는 /etc/init.d에 있음.
- S(시작)나 K(kill)로 시작하는 스크립트는 각각 시작과 종료 시 실행됨.
- 문자들 옆에 있는 숫자는 그들이 실행하는 순서
- 예시

  > /etc/rc.d/rc0.d$ ls
  > K10updates  K80openvpn        
    - 런 레벨 0 또는 셧다운 모드로 전환 시, 업데이트 서비스를 중지시키기 위한 스크립트를 실행하고 나서 openvpn을 열려고 할 것임.
    - 스템 부팅 실행 레벨을 확인하려면 /etc/inittab 파일에서 기본 실행 레벨 확인
    - 기본적인 실행 레벨 변경 가능

## 2. System V Service
- Sys V 서비스를 관리하는데 사용할 수 있는 명령줄 도구

### 서비스 나열
> service --status-all

### 서비스 시작
> sudo service networking start

### 서비스 중지
> sudo service networking stop

### 서비스 재시작
> sudo service networking restart

## 3. Upstart Overview
- 엄격한 시작 프로세스, 작업 차단 등과 같은 Sys V의 문제를 개선하기 위해 만들어짐.
- 이벤트 및 작업 중심 모델은 발생하는 이벤트에 응답 할 수 있도록 함.
- 작업 및 해당 구성 목록을 보려면 ls /etc/init 명령 실행(아래와 같은 결과 출력됨.)

  > acpid.conf                   mountnfs.sh.conf  
  > alsa-restore.conf            mtab.sh.conf  
  > alsa-state.conf              networking.conf  
  > alsa-store.conf              network-interface.conf  
  > anacron.conf                 network-interface-container.conf  
    
- 이러한 작업 구성에는 작업 시작 방법 및 작업 시작시기에 대한 정보가 포함됨.
- network.conf 파일에서 예를 들어 보면: 
  > start on runlevel [235]   
  > stop on runlevel [0]  
    - 이는 런레벨 2, 3, 5에서 네트워킹을 시작하고 런 레벨 0에서 네트워킹을 중단한다는 의미.

- 구성 파일을 쓰는 방법에는 여러 가지가 있으며, 사용 가능한 다른 작업 구성을 보면 이를 발견 가능
- Upstart의 작동 방식:
  - 첫 번째: / etc / init에서 작업 구성 로드
  - 두 번째: 시작 이벤트가 발생하면 해당 이벤트에 의해 트리거된 작업 실행
  - 세 번째: 이러한 작업은 새로운 이벤트를 만든 다음 해당 이벤트가 더 많은 작업을 트리거함.
  - 네 번째: Upstart는 필요한 모든 작업을 완료 할 때까지 계속 이 작업(첫 번째 -> 두 번째 -> 세 번째) 수행

## 4. Upstart Jobs
- 많은 이벤트와 작업 실행 가능
- Upstart 시스템에서 사용할 수있는 많은 유용한 명령:

  - 작업 보기
    > initctl list  
    > shutdown stop/waiting  
    > console stop/waiting  
    > ...
      
      - 상태가 다른 업스트림 작업 목록이 표시됨. 
      - shutdown stop/waiting 에서 예를 들면,
      - 첫 번째 값(shutdown): 작업 이름
      - 두 번째 값(stop): 실제로 작업 목표
      - 세 번째 값(waiting): 현재 상태
      
  - 특정 작업 보기
    > initctl status networking  
    > networking start/running

  - 수종으로 작업 시작
    > sudo initctl start networking
  
  - 수동으로 작업 중지
    > sudo initctl stop networking

  - 수동으로 작업 재시작
    > sudo initctl restart networking

  - 수동으로 이벤트 내보니기
    > sudo initctl emit some_event

## 5. Systemd Overview
- 목표를 사용하여 시스템 가동
- 기본적으로 달성하고자하는 목표가 있으며이 목표에도 달성해야하는 종속성이 있음.
- 매우 유연하고 강력하며, 프로세스를 시작하기 위한 엄격한 순서를 따르지 않음.
- 일반적인 Systemd 부팅 시 발생하는 동작 순서:
  - 첫 번째: systemd는 일반적으로 /etc/systemd/system 또는 /usr/lib/systemd/systemd/system에 있는 구성 파일 로드
  - 두 번째: 그런 다음 일반적으로 default.target인 부팅 목표 결정
  - 세 번째: Systemd는 부팅 대상의 종속성 파악 및 활성화

- Sys V runlevels와 비슷하게 systemd는 다른 대상으로 부팅:
  - poweroff.target: 시스템 종료
  - rescue.target: 단일 사용자 모드
  - multiuser.target: 네트워킹이 있는 다중 사용자
  - graphical.target: 네트워킹 및 GUI가있는 다중 사용자
  - reboot.target: 다시 시작
    - default.target의 기본 부팅 목표는 일반적으로 graphical.target을 가리킴.

- systemd가 작동하는 주요 개체는 단위.
- 단순히 서비스를 중지하고 시작하는 것이 아니라, 파일 시스템을 탑재할 수 있고, 네트워크 소켓 등을 감시할 수 있으며, 그러한 강건성 때문에 다른 유형의 장치를 작동시킬 수 있음.
- 가장 일반적인 단위들:
  - Service units: 시작하고 중지했던 서비스들. 이 단위 파일은 .service로 끝남.
  - Mount units: 마운트 파일 시스템, 이 단위 파일은 .mount로 끝남
  - Target units: 다른 단위를 그룹화. .target에서 끝남.

## 6. Systemd Goals
- 장치 파일의 간략한 개요와 장치를 수동으로 제어하는 방법 알아보기
- 기본 단위 서비스 단위 파일: foobar.service
  - [Unit]
    > Description=My Foobar  
    > Before=bar.target
  
  - [Service]
    > ExecStart=/usr/bin/foobar
  
  - [Install]
    > WantedBy=multi-user.target
  
  - 간단한 서비스 목표
  - 파일의 시작 부분에 [Unit] 섹션
    - 유닛 파일에 설명을 제공하고 유닛 활성화 시기 제어 가능
  - 그 다음 부분에 [Service] 섹션
    - 서비스를 시작, 중지 또는 재 로드 가능
  - 마지막 부분은 [Install] 섹션
    - 종속성에 사용됨.
- systemd 유닛과 함께 사용할 수있는 몇 가지 명령
  - 목록 단위
    > systemctl list-units

  - 단위 상태 보기
    > systemctl status networking.service

  - 서비스 시작
    > sudo systemctl start networking.service

  - 서비스 중지
    > sudo systemctl stop networking.service

  - 서비스 재시작
    > sudo systemctl restart networking.service

  - 단위 활성화
    > sudo systemctl enable networking.service

  - 단일 비활성화
    > sudo systemctl disable networking.service

## 7. Power States
- 시스템 종료시
  
  > sudo shutdown -h now
    - 이렇게 하면 시스템이 중지되고(전원 꺼짐), 이 작업을 수행할 시간도 지정해야 함
    - 몇 분 안에 시스템을 종료하는 시간 추가 가능
  > sudo shutdown -h +2
    - 이렇게 하면 2분 안에 시스템이 종료될 것임.
    - 셧다운 명령을 사용하여 다시 시작할 수도 있음
  >  sudo shutdown -r now
    - 또는 재부팅 명령어
  > sudo reboot
