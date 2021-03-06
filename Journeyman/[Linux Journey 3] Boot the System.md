# [Linux Journey 3] Boot the System
## 1. Boot Process Overview
- 간략한 소개
- Linux 부팅 과정의 4단게:
  1. BIOS(Basic Input / Output System)
     1. 하드웨어를 초기화하고 POST (Power-On Self Test)를 통해 모든 하드웨어의 상태 확인
     2. BIOS의 주요 작업은 부트 로더를로드하는 것
  
  2. Bootloader
      1. 커널을 메모리에로드 한 다음 일련의 커널 매개 변수로 커널 시작. 
      2. 가장 일반적인 부트 로더 중 하나는 보편적인 리눅스 표준인 GRUB.

  3. Kernel
      1. 즉시 장치와 메모리가 초기화. 
      2. 커널의 주요 작업은 init 프로세스를로드하는 것.

  4. Init
      1. 시작된 첫 번째 프로세스
      2. init은 시스템에서 필수적인 서비스 프로세스를 시작하고 중지 함을 기억!!
      3.  Linux 배포판에는 init의 세 가지 주요 구현이 있음.

## 2. Boot Process: BIOS
### BIOS
- 시스템 무결성 검사 수행
- IBM PC 호환 컴퓨터에서 가장 많이 사용되는 펌웨어
- BIOS의 주요 목표는 시스템 부트 로더를 찾는 것
- 하드 드라이브를 부팅하면 부팅 블록을 검색하여 시스템을 부팅하는 방법을 찾음.
- 디스크 파티션 방법에 따라 마스터 부트 레코드 (MBR) 또는 GPT를 찾음.
- MBR은 하드 드라이브의 첫 번째 섹터, 첫 번째 512 바이트에 있음.
- MBR에는 디스크의 다른 프로그램을로드하는 코드가 들어 있음.
- GPT로 디스크를 분할하면 부트 로더의 위치가 약간 변경됨.

### UEFI(Unified Extensible Firmware Interface)
- BIOS를 사용하지 않고 시스템을 부팅하는 또 다른 방법
- BIOS의 후계자로 설계되었으므로 오늘날 대부분의 하드웨어에는 UEFI 펌웨어가 내장되어 있음.
- 시작에 대한 모든 정보를 .efi 파일에 저장
- 이 파일은 하드웨어의 EFI 시스템 파티션이라는 특수 파티션에 저장됨.
- 이 파티션 안에는 부트 로더가 들어 있음.
- 기존의 BIOS 펌웨어에서 많은 개선 사항 제공
- Linux를 사용하고 있기 때문에 대부분의 사람들이 BIOS를 사용하고 있음.

## 3. Boot Process: Bootloader
- 부트 로더의 주요 책임:
  - 운영 체제로 부팅하면 비 Linux 운영 체제로도 부팅 가능
  - 사용할 커널 선택
  - 커널 매개 변수 지정 

- Linux 용 가장 일반적인 부트 로더는 GRUB, 시스템에서 사용하는 것이 가장 일반적
- LILO, efilinux, coreboot, SYSLINUX 등과 같이 사용할 수있는 많은 다른 부트 로더가 있음.
- 커널 매개변수를 살펴볼 필요가 있을 것임.
- 파라미터는 시작 시 'e' 키를 사용하여 GRUB 메뉴로 들어가면 찾을 수 있음.
- GRUB가 없다면 부팅 매개 변수 살펴 보기:
  - initrd: 초기 RAM 디스크의 위치 지정.
  - BOOT_IMAGE: 커널 이미지가 있는 곳.
  - root: 루트 파일 시스템의 위치. 커널은 이 위치를 검색하여 init을 찾음. 종종 UUID 또는 / dev / sda1과 같은 장치 이름으로 표현.
  - ro: 꽤 표준 적. 파일 시스템을 읽기 전용 모드로 마운트.
  - quiet: 부팅할 때 백그라운드에서 진행되는 메시지를 볼 수 없도록 추가.
  - splash: 스플래시 화면 표시

## 4. Boot Process: Kernel
-  시작하는 방법

### Initrd vs Initramfs
- 커널은 initrd를 마운트하고 필요한 부팅 드라이버를 얻은 다음 필요한 모든 것을로드 할 때 initrd를 실제 루트 파일 시스템으로 대체
- initramfs: 실제 루트 파일 시스템에 필요한 모든 드라이버를로드하기 위해 커널 자체에 내장 된 임시 루트 파일 시스템

### Mounting the root filesystem
- 루트 파일 시스템 마운트하기
- 이제 커널에는 루트 장치를 만들고 루트 파티션을 마운트하는 데 필요한 모든 모듈이 있음.
- 루트 파티션은 실제로 읽기 전용 모드로 먼저 마운트되므로 fsck가 안전하게 실행되고 시스템 무결성 검사 가능
- 루트 파일 시스템을 읽기-쓰기 모드로 다시 마운트함.

## 5. Boot Process: Init
- Linux에서 init의 세 가지 주요 구현: System V init (sysv) /Upstart / Systemd

### System V init (sysv)
- 전통적인 init 시스템.
- 시작 스크립트에 따라 프로세스를 순차적으로 시작하고 중지.
- 기계의 상태는 런레벨로 표시되며 각 런레벨은 다른 방법으로 기계를 시작하거나 정지.

### Upstart
- 이전 Ubuntu 설치에서 찾을 수있는 init
- 작업 및 이벤트에 대한 아이디어를 사용하고 이벤트에 응답하여 특정 작업을 수행하는 작업을 시작하여 작동함.

### Systemd
- init의 새로운 표준이며 목표 지향적.
- 기본적으로 달성하고자하는 목표가 있으며 목표를 완료하기 위해 목표의 의존성을 충족 시키려고 시도함.
