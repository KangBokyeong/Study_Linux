# [Linux Journey 2] The Filesystem
## 1. Filesystem Hierarchy

| 명령어 | 기능(설명)|
|:-----:|:-----:|
| / | 전체 파일 시스템 계층 구조의 루트 디렉토리. 이 디렉토리 아래에 모든 것 |
| /bin | 필수 실행 준비 프로그램 (바이너리)에는 ls 및 cp와 같은 가장 기본적인 명령이 포함되어 있음. |
| /boot | 커널 부트 로더 파일을 포함 |
| /dev | 장치 파일 |
| /etc | 핵심 시스템 구성 디렉토리는 바이너리가 아닌 구성 파일 만 보유해야합 |
| /home | 사용자의 개인 디렉토리, 문서, 파일, 설정 등 보유 |
| /lib | 바이너리가 사용할 수 있는 라이브러리 파일 저장 |
| /media |  USB 드라이브와 같은 이동식 미디어의 부착 지점으로 사용됨. |
| /mnt | 일시적으로 마운트 된 파일 시스템 |
| /opt | 선택적 응용 프로그램 소프트웨어 패키지 |
| /proc | 현재 실행중인 프로세스에 대한 정보. |
| /root | 루트 사용자의 홈 디렉토리. |
| /run | 마지막 부팅 이후 실행중인 시스템에 대한 정보 |
| /sbin | 필수 시스템 바이너리가 들어 있음. 일반적으로 루트에서만 실행 가능. |
| /srv | 시스템에서 제공하는 사이트 별 데이터 |
| /tmp | 임시 파일 저장 |
| /usr | 불행하게도이 이름은 홈 폴더와 같은 의미로 사용자 파일을 포함하지 않음. |
| /var | 시스템 로깅, 사용자 추적, 캐시 등에 사용되는 변수 디렉토리. 기본적으로 항상 변경 될 수있는 모든 항목. |

## 2. Filesystem Types
- 다양한 파일 시스템 구현 가능

### 저널링
- 대부분의 파일 시스템 유형에 기본적으로 제공되지만, 그렇지 않은 경우 저널링이하는 일을 알아야함.

### 공통 데스크탑 파일 시스템 유형

| 유형 | 기능(설명)|
|:-----:|:-----:|
| ext4 | 이것은 최신 Linux 파일 시스템의 최신 버전. 구형 ext2 및 ext3 버전과 호환됨. 최대 1 엑사 바이트의 디스크 볼륨과 최대 16 테라 바이트 이상의 파일 크기 지원. Linux 파일 시스템의 표준 선택 사항. |
| Btrfs | "Better or Butter FS"는 스냅 샷, 증분 백업, 성능 향상 등과 함께 제공되는 Linux 용 새 파일 시스템. 널리 사용 가능하지만 아직 안정적이지 않고 호환성이 있음. |
| XFS | 고성능 저널링 파일 시스템으로 미디어 서버와 같은 대용량 파일 시스템에 적합 |
| NTFS and FAT | Windows 파일 시스템 |
| HFS+ | 매킨토시 파일 시스템 |


- df -T 명령어를 사용하여 어떤 파일 시스템이 있는지 확인하기

  |Filesystem |    Type |    1K-blocks  |  Used | Available | Use% | Mounted on |
  |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
  |/dev/sda1|      ext4 |      6461592 | 2402708  | 3707604 | 40% | / |
  | udev    |       devtmpfs |   501356  |     4  |  501352 |  1% | /dev |
  | tmpfs    |      tmpfs   |    102544   | 1068  |  101476 |  2% | /run |
  | /dev/sda6   |   xfs     |  13752320 | 460112 | 13292208  | 4% | /home |

## 3. Anatomy of a Disk
- 본질적으로 여러 블록 장치를 만드는 파티션으로 세분 될 수 있음.

### 파티션 테이블
-  디스크가 어떻게 파티션되는지를 시스템에 알려줌.
- 파티션의 시작과 끝, 부팅 가능한 파티션, 어떤 파티션에 할당 된 디스크의 섹터 등을 알려줌.
- 마스터 부트 레코드 (MBR)와 GUID 파티션 테이블 (GPT)의 두 가지 주 파티션 테이블 스키마가 사용됨.

### 분할
- 우리가 데이터를 구성하는 데 도움이되는 파티션으로 구성됨.
- 한 디스크에 여러 개의 파티션을 가질 수 있으며 서로 겹칠 수 없음.
- 파티션에 할당되지 않은 공간 -> 여유 공간
- 파티션 유형은 파티션 테이블에 따라 다름
- MBR
  - 전통적인 파티션 테이블이 표준으로 사용됨.
  - 기본, 확장 및 논리 파티션을 가질 수 있음.
  - 4 개의 기본 파티션 한계가 있음.
  - 기본 파티션을 확장 파티션으로 만들면 추가 파티션을 만들 수 있음.
    (디스크에 하나의 확장 파티션 만있을 수 있음) 
    그런 다음 확장 파티션 내부에 논리 파티션 추가. 
      - 논리 파티션은 다른 파티션과 마찬가지로 사용됨.
  - 최대 2TB의 디스크 지원

- GPT(GUID Partition Table)
  - 디스크 파티션의 새로운 표준이 되고 있음.
  - 파티션 유형이 하나뿐이므로 많은 파티션을 만들 수 있음.
  - 각 파티션에는 GUID (Globally Unique ID)가 있음.
  - 주로 UEFI 기반 부팅과 함께 사용됨.
  
### 파일 시스템 구조
- 파일과 디렉토리의 체계적인 모음
- 여기서는 더 자세하게 알아 보기

- 부트 블록 (Boot block): 파일 시스템의 처음 몇 섹터에 위치. 실제로는 파일 시스템에 의해 사용되지 않음. 운영 체제를 부팅하는데 사용되는 정보가 들어 있음. 운영 체제에는 하나의 부팅 블록만 필요. 파티션이 여러 개인 경우에는 부팅 블록이 있지만 대부분은 사용되지 않음.
- 수퍼 블록: 부트 블록 다음에 오는 단일 블록. inode 테이블의 크기, 논리 블록의 크기 및 파일 시스템의 크기와 같은 파일 시스템에 대한 정보 포함.
- Inode 테이블: 파일을 관리하는 데이터베이스. 각 파일이나 디렉토리는 inode 테이블에 고유한 항목과 파일에 대한 다양한 정보를 가지고 있음.
- 데이터 블록: 파일과 디렉토리의 실제 데이터


- MBR 파티셔닝 테이블 (msdos)을 사용하는 파티션의 예시
- sudo parted -l 입력하여, 아래와 같이 출력(표는 가독성을 위함)

  > Model: Seagate (scsi)

  > Disk /dev/sda: 21.5GB

  > Sector size (logical/physical): 512B/512B

  > Partition Table: msdos

  | Number | Start | End  |  Size  |  Type   |   File system  |   Flags |
  |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
  | 1  |  1049kB | 6860MB | 6859MB | primary |  ext4  |  boot  |
  | 2  |    6861MB | 21.5GB | 14.6GB | extended | | |
  | 5  |    6861MB | 7380MB | 519MB  | logical  | linux-swap(v1) | |
  | 6  |    7381MB | 21.5GB | 14.1GB | logical  | xfs | |


- 파티션에 고유 ID 만 사용하는 GPT 예시(표는 가독성을 위함)

  > Model: Thumb Drive (scsi)

  > Disk /dev/sdb: 4041MB

  > Sector size (logical/physical): 512B/512B

  > Partition Table: gpt

  |Number | Start |  End   |  Size   |  File system | Name    |    Flags|
  |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
  | 1  |    17.4kB | 1000MB | 1000MB |         |      first||
  |2  |    1000MB | 4040MB | 3040MB  |          |    second||


## 4. Disk Partitioning
- 디스크를 파티션하는데 사용할 수 있는 여러 가지 도구
  - fdisk: 기본 명령 줄 분할 도구. GPT를 지원하지 않음.
  - parted: MBR 및 GPT 파티셔닝을 모두 지원하는 명령 줄 도구
  - gparted: parted의 GUI 버전
  - gdisk: fdisk, MBR 만 지원하지는 않음. GPT만 지원함.
  
- parted를 사용한 파티셔닝 USB 장치를 연결하면 장치 이름이 / dev / sdb2라는 것을 알 수 있음.
  - Launch parted
    > sudo parted
    - 분리 도구에 입력됨. 
    - 여기에서 장치를 분할하는 명령을 실행 가능

  - Select the device
    > select /dev/sdb2
    - 작업할 장치 선택 시, 장치 이름으로 선택

  - 현재 파티션 테이블 보기

    > (parted) print                                                            

    > Model: Seagate (scsi)

    > Disk /dev/sda: 21.5GB

    > Sector size (logical/physical): 512B/512B

    > Partition Table: msdos


    |Number | Start |  End   |  Size  |  Type  |    File system  |   Flags|
    |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
    |1   |   1049kB | 6860MB | 6859MB | primary |  ext4  |          boot|
    | 2    |  6861MB | 21.5GB | 14.6GB | extended|||
    | 5   |   6861MB | 7380MB | 519MB  | logical |  linux-swap(v1)||
    | 6  |    7381MB|  21.5GB | 14.1GB | logical  | xfs||

    - 장치에 사용 가능한 파티션이 표시됨.
    - 시작과 끝 지점: 파티션이 하드 드라이브의 공간을 차지하는 곳

  - 장치 분할
    > mkpart primary 123 4567
    - 시작점과 끝점을 선택하고 파티션을 만들면 사용한 테이블에 따라 파티션 유형을 지정해야함

  - 파티션 크기 조정
    - 공간이 없으면 파티션의 크기 조정 가능
    > resize 2 1245 3456
    - 파티션 번호를 선택한 다음 크기를 조정할 위치의 시작 및 끝 지점 선택
    
## 5. Creating Filesystems
- 파일 시스템 성생하기
  > sudo mkfs -t ext4 /dev/sdb2
  - mkfs(파일 시스템 만들기) 도구를 사용하면 원하는 파일 시스템의 유형과 원하는 위치 지정 가능
  - 기존 파일 시스템 위에 파일 시스템을 만들려고한다면 파일 시스템을 손상된 상태로 남겨 둘 가능성 높음.

## 6. mount and umount
- 파일 시스템의 내용을보기 전에 마운트해야함.
- 장치 위치, 파일 시스템 유형 및 마운트 지점이 필요함.
- 마운트 지점은 파일 시스템이 연결될 시스템의 디렉토리

### mkdir / mydrive 라는 마운트 지점 만들기
  
  > sudo mount -t ext4 /dev/sdb2 /mydrive
    
    - /mydrive로 이동하면 파일 시스템 컨텐츠를 볼 수 있음.
    - -t ext4: 파일 시스템의 유형
    - /dev/sdb2: 장치 위치 
    - /mydrive: 마운트 지점
    - 즉, 파일 시스템의 유형, 저장 위치 마운트 지점 순으로 지정해줌.
  
### 마운트 지점에서 장치를 마운트에서 해제 시
  
  > sudo umount /mydrive 
  > or 
  > sudo umount /dev/sdb2
    
    - 커널은 장치를 찾은 순서대로 장치 이름을 지정
    - 이름 대신 장치의 보편적으로 고유 한 ID (UUID) 사용 가능

### 블록 장치에 대한 시스템의 UUID 보기

  - sudo blkid 명령어 시행 시 아래 와 같은 결과 출력

    > /dev/sda1: UUID="130b882f-7d79-436d-a096-1e594c92bb76" TYPE="ext4" 
    
    > /dev/sda5: UUID="22c3d34b-467e-467c-b44d-f03803c2c526" TYPE="swap" 

    > /dev/sda6: UUID="78d203a0-7c18-49bd-9e07-54f44cdb5726" TYPE="xfs" 
    
    - 장치 이름, 해당 파일 시스템 유형 및 해당 UUID를 볼 수 있음.

### 무언가를 탑재하고 싶을 때
  
  > sudo mount UUID=130b882f-7d79-436d-a096-1e594c92bb76 /mydrive

## 7. /etc/fstab
- 시작할 때 파일 시스템을 자동으로 탑재하려면 파일 시스템 테이블에 대한 단락이 아닌 /etc/fstab("eff strack"으로 발음됨)에 파일을 추가
- 마운트된 파일 시스템의 영구 목록이 포함되어 있음.
- cat /etc/fstab 입력 시 아래 결과 출력됨.

  > UUID=130b882f-7d79-436d-a096-1e594c92bb76 /               ext4    relatime,errors=remount-ro 0       1

  > UUID=78d203a0-7c18-49bd-9e07-54f44cdb5726 /home           xfs     relatime        0       2

  > UUID=22c3d34b-467e-467c-b44d-f03803c2c526 none            swap    sw              0       0

  - 각각의 필드
  
    - UUID: 기기 식별자
    - Mount point: 파일 시스템이 마운트되는 디렉토리
    - Filesystem type: 파일 시스템
    - Options: 기타 마운트 옵션
    - Dump: 덤프 유틸리티가 백업을 수팽할 시기를 결정할 때 사용. 기본값은 0
    - Pass: 검사할 파일시스템을 결정하기 위해 fsck에서 사용하고 값이 0이면 검사되지 않음.

- 항목 추가 시, 위의 항목 구문을 사용하여 / etc / fstab 파일을 직접 수정
- 파일 수정 시, 유의!!

## 8. swap

|Number | Start |  End  |   Size  |  Type   |   File system  |   Flags|
|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
| 5 |     6861MB | 7380MB | 519MB |  logical |  linux-swap(v1) ||

- 시스템에 가상 메모리를 할당하는데 사용됨.
- 메모리가 부족하면 시스템은 이 파티션을 사용하여 유휴 프로세스 메모리를 디스크로 "스왑"하여 메모리가 낭비되지 않도록 함.

### 스왑 공간을위한 파티션 사용
- 스왑 공간으로 사용하기 위해 / dev / sdb2의 파티션 설정
  - 1) 파티션에 아무것도 가지고 있지 않은지 확인
  - 2) mkswap / dev / sdb2를 실행하여 스왑 영역 초기화
  - 3) swapon / dev / sdb2를 실행하여 스왑 장치 활성화
  - 4) 부팅 시 스왑 파티션을 계속 유지하려면 / etc / fstab 파일에 항목 추가. sw은 사용할 파일 시스템 유형
  - 5) 스왑 제거 : swapoff / dev / sdb2

## 9. Disk Usage
- 디스크 사용률을 확인하는 데 사용할 수 있는 몇 가지 도구
- df -h 사용 시 아래 결과 출력 됨.

  |Filesystem  |   1K-blocks  |  Used | Available | Use% | Mounted on |
  |:-----:|:-----:|:-----:|:-----:|:-----:|:-----:|
  | /dev/sda1 |  6.2G | 2.3G | 3.6G | 40% | / |

  - df 명령: 현재 마운트 된 파일 시스템의 활용도를 보여줌.
  - -h 플래그: 사람이 읽을 수있는 형식 제공
  - 장치가 무엇인지, 사용된 용량과 사용 가능한 용량 확인 가능

- 어떤 파일 또는 디렉터리가 이 공간을 차지하고 있는지 알고 싶다면 du 명령 사용

  > du -h
  
  - 현재 디렉토리의 디스크 사용량을 보여줌
  - du -h / 와 함께 루트 디렉토리를 들여다 볼 수 있음.
  
- 디스크 사용량 확인 시 du 명령 사용 권장.

## 10. Filesystem Repair
- fsck(파일 시스템 검사) 명령은 파일 시스템의 일관성을 확인하는데 사용됨.
- 디스크 부팅 시 디스크가 마운트 되기 전에 fsck가 실행 되어 모든 것이 정상적으로 작동하는지 확인
- 디스크가 너무 나빠서 수동으로 해야할 경우도 있음.
- 응급 복구 디스크나 마운트 하지 않고 파일 시스템에 액세스 할 수 있는 곳에서는 이 작업 수행 권장
  
  > sudo fsck /dev/sda

## 11. Inodes
- 데이터베이스를 inode 테이블이라고함.

### inode란 무엇입니까?
- 테이블의 항목이며 모든 파일에 하나씩 있음.
- 다음은 파일에 대한 모든 것을 설명하고 있음.
  - File type: 일반 파일, 디렉토리, 문자 장치 등
  - Owner: 소유자
  - Group: 그룹
  - Access permissions: 액세스 권한
  - Timestamps: mtime (마지막 파일 수정 시간), ctime (마지막 속성 변경 시간), atime (마지막 액세스 시간)
  - 파일에 대한 하드 링크 수
  - 파일 크기
  - 파일에 할당된 블록의 수
  - 파일의 데이터 블록에 대한 포인터 -> 가장 중요함!!
- 기본적으로 inode는 파일 이름과 파일 자체를 제외한 파일에 대한 모든 것 저장

### inode는 언제 만들어 집니까?
- 파일 시스템이 생성되면 inode를위한 공간도 할당됨.
- 디스크 용량에 따라 필요한 inode 공간의 양을 결정하는 알고리즘이 있음.
- 시스템에 남아있는 inode의 수를 보려면 df -i 명령 사용

### inode 정보
- 숫자로 식별되며 파일이 생성되면 inode 번호가 할당되고 번호는 순차적으로 할당됨.
- 새 파일을 만들 때 다른 파일보다 낮은 inode 번호를 얻는 경우가 있음.
  - 이는 inode가 삭제되면 다른 파일에서 다시 사용할 수 있기 때문
- inode 번호를 보려면 ls -li 실행(명령 실행 시 아래와 같은 결과가 출력됨.)

  > 140 drwxr-xr-x 2 pete pete 6 Jan 20 20:13 Desktop

  > 141 drwxr-xr-x 2 pete pete 6 Jan 20 20:01 Documents

  - 첫 번째 필드: inode 번호를 나열

- stat를 사용하여 파일에 대한 자세한 정보를 볼 수 있으며 inode에 대한 정보 제공(명령 실행 시 아래와 같은 결과가 출력됨.)

  > stat ~/Desktop/

  > File: ‘/home/pete/Desktop/’

  > Size: 6               Blocks: 0          IO Block: 4096   directory

  > Device: 806h/2054d      Inode: 140         Links: 2

  > Access: (0755/drwxr-xr-x)  Uid: ( 1000/   pete)   Gid: ( 1000/   pete)

  > Access: 2016-01-20 20:13:50.647435982 -0800

  > Modify: 2016-01-20 20:13:06.191675843 -0800

  > Change: 2016-01-20 20:13:06.191675843 -0800

  > Birth: -


## 12. symlinks
- 앞서 사용한 예시를 다시 살펴보면(명령어는 ls -li)

  > 140 drwxr-xr-x 2 pete pete 6 Jan 20 20:13 Desktop

  > 141 drwxr-xr-x 2 pete pete 6 Jan 20 20:01 Documents
  
  - 세 번째 필드: 링크 수 (-> 파일에있는 총 하드 링크 수)
  

### Symlinks
- 예시

  > echo 'myfile' > myfile

  > echo 'myfile2' > myfile2

  > echo 'myfile3' > myfile3


  > ln -s myfile myfilelink

  > ~/Desktop$ ls -li

  > total 12

  > 151 -rw-rw-r-- 1 pete pete 7 Jan 21 21:36 myfile

  > 93401 -rw-rw-r-- 1 pete pete 8 Jan 21 21:36 myfile2

  > 93402 -rw-rw-r-- 1 pete pete 8 Jan 21 21:36 myfile3

  > 93403 lrwxrwxrwx 1 pete pete 6 Jan 21 21:39 myfilelink -> myfile

  - myfile을 가리키는 myfilelink라는 symlink를 만들었다는 것을 알 수 있음.
  - symlink는 ->로 표시
  - 어떻게 새로운 inode 번호를 얻었는지 주목하고 symlink는 단지 파일명을 가리키는 파일일 뿐임.
  - symlink를 수정하면 파일도 수정됨.
  - inode 번호는 파일 시스템에 고유 -> inode 번호로 다른 파일 시스템의 파일을 참조할 수 없다는 것을 의미
  - symlinks를 사용하면 inode 번호는 사용하지 않고 filename을 사용하여 다른 파일 시스템에서 참조 가능
  
### Hardlinks
- 예시
 
  > ~/Desktop$ ln myfile2 myhardlink

  > ~/Desktop$ ls -li

  > total 16

  > 151 -rw-rw-r-- 1 pete pete 7 Jan 21 21:36 myfile

  > 93401 -rw-rw-r-- 2 pete pete 8 Jan 21 21:36 myfile2

  > 93402 -rw-rw-r-- 1 pete pete 8 Jan 21 21:36 myfile3

  > 93403 lrwxrwxrwx 1 pete pete 6 Jan 21 21:39 myfilelink -> myfile

  > 93401 -rw-rw-r-- 2 pete pete 8 Jan 21 21:36 myhardlink

  - 동일한 inode에 대한 링크가있는 다른 파일을 만듦.
  - myfile2 또는 myhardlink의 내용을 수정하면 변경 내용이 두 파일에 모두 표시되지만 myfile2를 삭제해도 myhardlink를 통해 파일에 액세스 가능
  - ls 명령의 링크 카운트가 작용하는 부분이 있음.
  - 링크 수는 아이 노드에있는 하드 링크 수
  - 파일을 제거하면 링크 수가 줄어듬.
  - inode에 대한 모든 하드 링크가 삭제되면 inode 만 삭제됨.
  - 파일을 만들 때 링크 수는 해당 inode를 가리키는 유일한 파일이므로 1
  - symlink와 달리, inode가 파일 시스템에 고유하기 때문에 파일 시스템 확장 :x:

### Creating a symlink
- symlink 생성
  > ln -s myfile mylink
    - 기호 링크를 작성하려면 기호에 -s와 함께 ln 명령을 사용하고 대상 파일과 링크 이름 지정

### Creating a hardlink
- hardlink 생성
  > ln somefile somelink
    - symlink와 비슷하지만, -s가 생략 됨.
