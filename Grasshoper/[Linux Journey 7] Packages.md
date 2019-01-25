# [Linux Journey 7] Packages
## Software Distribution
- 업스트림 공급자: 코드 컴파일, 설치 방법 기록
- 패키지 유지 관리자: 소프트웨어를 사용자에게 제공하도록 처리
- 패키지 유지 보수자: 패키지 형태로이 소프트웨어를 검토, 관리 및 배포
> 업스트림 공급자 -- [패키지] --> 패키지 유지 관리자 -- [소프트웨어] --> 사용자

## Package Repositories
- 배포판에는 이미 패키지를 가져올 수 있도록 사전 승인된 출처가 있음.
- 시스템에 표시되는 모든 기본 패키지가 설치됨.
- 데비안 시스템에서이 소스 파일: /etc/apt/sources.list 파일

## tar and gzip
### gzip으로 파일 압축하기
- Linux에서 파일을 압축하는데 사용되는 프로그램.
- .gz 확장자로 끝남.
- 아카이브에 여러 파일 추가 불가

  |명령어|기능|
  |:-----:|:-----:|
  |gzip mycoolfile|파일 압축|
  |gunzip mycoolfile.gz|파일 압축 풀기|

### tar로 아카이브 만들기 
- 아카이브 생성 시, 확장자: .tar
- 예시
  > tar cvf mytarfile.tar mycoolfile1 mycoolfile2
  - c: 생성
  - v: 프로그램이 장황함을 알리고 그것이 무엇을 하는지.
  - f: tar 파일의 파일 이름이 이 옵션 뒤에 와야 하며, tar 파일 생성 시 이름 필요

### tar로 압축 파일 풀기
- tar로 압축파일 풀기 예시
  > tar xvf mytarfile.tar
  - x: 추출
  - v: 프로그램이 장황함을 알리고 그것이 무엇을 하는지.
  - f: 추출할 파일
  
### tar와 gzip으로 압축 파일 압축 / 압축 해제
- 압축된 tar 파일 생성
  > tar czf myfile.tar.gz
- 압축 풀기 및 포장 풀기 
  > tar xzf file.tar
  - 암기법(쉽게 기억할 수 있는 방법): eXtract all Zee Files!(xzf)

### 기타 유틸리티 
- bzip2, 압축, 압축, 압축 해제 등과 같은 다른 압축 및 압축 유형이 있음. 
- 이들은 다소 일반적인 것은 아니지만 여러 유틸리티가 서로 다른 명령을 요구한다는 점을 명심.

## Package Dependencies
- 패키지를 실행하는데 필요한 의존성이 가장 빈번하게 발생
- Linux에서 이러한 종속성은 종종 다른 패키지 또는 공유 라이브러리임.
  - 공유 라이브러리: 다른 프로그램에서 사용하고 스스로 다시 작성하지 않으려는 코드 라이브러리
- 의존성이 없다면 패키지는 깨진 상태로 끝날 것임.
  
## rpm and dpkg

### 패키지 설치
- Debian 
  > dpkg -i some_deb_package.deb

- RPM
  > rpm -i some_rpm_package.rpm

- i(install): 설치를 의미함. --install 이라고 써도 무방

### 패키지 제거
- Debian
  > dpkg -r some_deb_package.deb
  - r: 제거 

- RPM
  > rpm -e some_rpm_package.rpm
  - e: 지우기(제거)

### 설치된 패키지 나열
- Debian
  > dpkg -l
  - l: 목록
  
- RPM
  > rpm -qa
  - q: 쿼리에 대한
  - a: 모든 것

## yum and apt
- 관리 시스템

### 저장소에서 패키지 설치
- Debian
  > apt install package_name

- RPM
  > yum install package_name

### 패키지 제거
- Debian
  > apt remove package_name

- RPM
  > yum erase package_name

### 저장소용 패키지 업데이트
- 항상 최신 상태로 유지하는 것이 가장 좋음.

- Debian
  > apt update; apt upgrade

- RPM
  > yum update

### 설치된 패키지에 대한 정보 얻기
- Debian
  > apt show package_name  

- RPM
  > yum info package_name

## Compile Source Code
1. 소스 코드를 컴파일 할 수있는 도구를 설치하는 소프트웨어가 있어야함.
    > sudo apt install build-essential
2. 패키지 파일의 내용 (대부분 .tar.gz 파일) 추출
    > tar -xzvf package.tar.gz
- 어떤 일을하기 전에 패키지 안의 README 나 INSTALL 파일을 살펴보기.(특정 설치 지침이있는 경우도 있음.)

3. 시스템의 의존성을 검사(패키지 내용 -> configure 스크립트)
    > ./configure
      - ./는 현재 디렉토리에 스크립트 실행 가능함을 보여줌
 
4. 파일보고 소프트웨어 빌드 
  - 기본 명령어
    > make
  
  - 실제로 패키지를 설치하고 올바른 파일을 컴퓨터의 올바른 위치에 복사
    > sudo make install
  
  - 패키지 제거
    > sudo make uninstall  
  
  - 실제로 백그라운드에서 얼마나 진행되는지 알지 못할 수도 있음 

5. make install이 아닌 용이한 명령
  - 쉽게 설치 및 제거 할 수 있는 .deb 파일 생성.
    > sudo checkinstall
  
