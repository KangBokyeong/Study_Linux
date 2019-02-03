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
## 4. NFS
## 5. Samba
