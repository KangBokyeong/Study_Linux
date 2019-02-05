# [Linux Journey 1] Network Sharing
## 1. File Sharing Overview
- 네트워크의 다른 컴퓨터 간에 데이터를 복사하는 두 가지 방법을 설명
- 하나의 간단한 파일 공유 도구: scp 명령
- scp 명령은 secure copy를 의미하며, cp 명령이 수행하는 것과 똑같이 작동하지만, 동일한 네트워크의 호스트 간에 당신이 한 호스트에서 다른 호스트로 복사할 수 있도록 함.
- ssh를 통해 작동함.
- 로컬 호스트에서 원격 호스트로 파일을 복사하려면
  > scp myfile.txt username@remotehost.com:/remote/directory
- 원격 호스트에서 로컬 호스트로 파일을 복사하려면
  > scp username@remotehost.com:/remote/directory/myfile.txt /local/directory
- 로컬 호스트에서 원격 호스트로 디렉토리를 복사하려면
  > scp -r mydir username@remotehost.com:/remote/directory

## 2. rsync
- 복사하고 있는 데이터가 이미 있는 경우에 미리 체크하는 특별한 알고리즘 사용
- 차이점 복사
- 체크섬으로 복사하는 파일의 무결성을 확인
- 작은 최적화로 파일 전송 유연성이 향상되고 원격 및 로컬 디렉터리 동기화, 데이터 백업, 대용량 데이터 전송 등에 rsync가 이상적임.
- 일반적으로 사용되는 몇 가지 rsync 옵션
  - v(verbose output): 자세한 출력
  - r(recursive into directories) 디렉토리로 재귀적으로
  - h(human readable output): 사람이 읽을 수 있는 출력
  - z:전송이 용이하도록 압축. 느린 연결에 적합

- 동일한 호스트의 파일 복사 / 동기화
  > rsync -zvr /my/local/directory/one /my/local/directory/two
- 원격 호스트에서 로컬 호스트로 파일 복사 / 동기화
  > rsync /local/directory username@remotehost.com:/remote/directory
- 로컬 호스트에서 원격 호스트로 파일 복사
  > rsync username@remotehost.com:/remote/directory /local/directory

## 3. Simple HTTP Server
- Python에는 HTTP를 통해 파일을 제공하기위한 매우 유용한 도구가 있음.
- 네트워크의 다른 컴퓨터에서 액세스 할 수 있는 빠른 네트워크 공유를 만들고자 할 때 유용
- 실행 예시
  > python -m SimpleHTTPServer
    - 로컬 호스트 주소를 통해 액세스 할 수있는 기본 웹 서버가 설정됨.
    - 이 머신을 실행 한 머신의 IP 주소를 가져온 다음 다른 머신에서 http : // IP_ADDRESS : 8000을 사용하여 브라우저에서 액세스 해보기
    - 웹 브라우저에서 http : // localhost : 8000을 입력하여 사용 가능한 파일을 볼 수 있음.

## 4. NFS(Network File System)
- Linux에서 가장 표준적인 네트워크 파일 공유.
- 이를 사용하면 서버가 네트워크를 통해 하나 이상의 클라이언트와 디렉토리 및 파일 공유 가능
- NFS 클라이언트 설정 예시
  > sudo service nfsclient start  
  > sudo mount server:/directory /mount_directory  

- Automounting
  - 지정된 디렉토리에서 파일에 액세스하면 automount가 원격 서버를 검색하여 자동으로 마운트함.

## 5. Samba
- Linux 유틸리티를 Linux에서 CIFS(Common Internet File System)와 함께 사용하는 것
- 파일 공유 외에도 프린터와 같은 리소스 공유 가능

### Samba와 네트워크 공유 만들기
- Windows 컴퓨터에서 액세스 할 수있는 네트워크 공유를 만드는 기본 단계를 수행

### Samba 설치
> sudo apt update 
> sudo apt install samba  

### smb.conf 설정
- Samba의 설정 파일은 /etc/samba/smb.conf에 있으며,이 파일은 공유해야 할 디렉토리, 액세스 권한 및 기타 옵션을 시스템에 알려 주어야 함.
- 기본 smb.conf에는 주석 처리 된 코드가 이미 많이 포함되어 있으며 이를 사용자 자신의 구성을 작성하는 예제로 사용 가능
- 명령 예시
  >  sudo vi /etc/samba/smb.conf  
  
### Samba에 대한 암호 설정
> sudo smbpasswd -a [username]  

### 공유 디렉토리 만들기
> mkdir /my/directory/to/share  

### Samba 서비스 재시작
> sudo service smbd restart 

### Windows를 통해 Samba 공유에 액세스
- Windows의 경우, 실행 프롬프트에서 네트워크 연결을 입력하십시오. \\ HOST \ sharename.

### Linux를 통해 Samba / Windows 공유에 액세스
> smbclient //HOST/directory -U user

### 시스템에 Samba 공유 첨부
- 파일을 하나씩 전송하는 대신 시스템에 네트워크 공유를 마운트 가능 
  > sudo mount -t cifs servername:directory mountpount -o user=username, pass=password
