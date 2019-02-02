# [Linux Journey 7] Logging
## 1. System Logging
- 데이터가 실제로 로그의 형태로 당신의 시스템에 저장되도록 보내짐
- 데이터는 대개 /var 디렉토리에 보관되며, /var 디렉토리는 로그와 같은 가변 데이터를 보관하는 위치에 있음.

- 메시지들이 시스템에서 수신되는 방법은? 정보를 시스템 로거로 전송하는 syslog라는 서비스가 있음.

- 실제로 많은 구성 요소를 포함하고 있으며, 중요한 구성 요소 중 하나는 syslogd(새로운 Linux 배포에서 rsyslogd를 사용함)라고 하는 데몬
-  데몬은 이벤트 메시지가 발생할 때까지 기다렸다가 이 메시지에 대해 알고자 하는 작업을 필터링하여 파일, 콘솔 또는 아무 작업도 하지 않음으로 파일을 전송 가능

- 일반적으로 로그 형식은 타임스탬프와 이벤트 세부 정보를 포함해야 함.

- syslog의 한 가지 예시 
  > less /var/log/syslog  
  > Jan 27 07:41:32 icebox anacron[4650]: Job `cron.weekly' started
    - 1월 27일 07:41:32에 우리의 크론 서비스가 운영되었다는 것을 볼 수 있음.
    - 주간 근무 syslog가 수집하는 모든 이벤트 메시지는 /var/log/syslog 파일에서 볼 수 있음.
  
## 2. syslog
- syslog 서비스: 로그를 관리하고 시스템 로거로 전송
- Rsyslog는 syslog의 고급 버전이며, 대부분의 Linux 배포는 이 새 버전을 사용해야 함.
- syslog 서비스가 수집하는 모든 로그의 출력은 /var/log/syslog(인증 메시지를 제외한 모든 메시지)에서 찾을 수 있음.
- 시스템 로거에서 어떤 파일이 유지되는지 알아보려면 /etc/rsyslog.d의 구성 파일 참조
- 참조 방법(예시)
  > less /etc/rsyslog.d/50-default.conf   
  > '# First some standard log files.  Log by facility.'  
  > '#'  
  > auth,authpriv.*                 /var/log/auth.log  
  > *.*;auth,authpriv.none          -/var/log/syslog  
  > #cron.*                         /var/log/cron.log  
  > #daemon.*                       -/var/log/daemon.log  
  > kern.*                          -/var/log/kern.log  
  > #lpr.*                          -/var/log/lpr.log  
  > mail.*                          -/var/log/mail.log  
  > #user.*                         -/var/log/user.log  
    - 위 '는 실제로 없으며, 이 md문법 특성상 형식이 변화되기에 방지하기 위해서 붙였을 뿐임.
    - 파일을 기록하기 위한 이러한 규칙은 왼쪽 열의 선택기와 오른쪽 열의 동작에 의해 표시
    - 파일, 콘솔 등에서 로그 정보를 보낼 위치를 알려줌.
    - 모든 애플리케이션과 서비스가 로그 관리를 위해 rsyslog를 사용하는 것은 아니므로, 로그 내용을 구체적으로 알고 싶으면 이 디렉토리 내부를 살펴봐야 함.
    
- 로그인 동작에 대한 자세한 내용은 로거 명령을 사용하여 수동으로 로그를 전송(아래와 같이)
  > logger -s Hello
    - /var/log/syslog 내부를 살펴 보면 로그에 이 항목이 표시됨.

## 3. General Logging
- 시스템에서 수행하는 작업을 한 눈에 파악할 수 있는 일반 로그 파일
- /var/log/messages
  - 모든 중요하지 않은 메시지와 디버그되지 않은 메시지가 포함되어 있으며 부팅하는 동안 기록 된 메시지 (dmesg), auth, cron, daemon 등이 포함됨.
  - 시스템이 작동하는 방식을 간단히 파악하는데 매우 유용

- /var/log/syslog
  -  auth 메시지를 제외한 모든 것을 기록하며 시스템의 오류를 디버깅하는데 매우 유용

## 4. Kernel Logging
- /var/log/dmesg 
  - 부팅 시, 시스템은 커널 링버퍼에 대한 정보 기록
  - 부팅 중 하드웨어 드라이버, 커널 정보 및 상태 등에 대한 정보 표시
  - 부팅 할 때마다 재설정 됨.
  - 부팅 중 또는 하드웨어 문제 발생 시 문제가 발생한 경우에는 dmesg가 가장 적합한 위치임.
  - dmesg 명령을 사용하여 이 로그를 볼 수도 있음.
 
- /var/log/kern.log 
  - 커널 정보와 이벤트를 시스템에 로깅
  - dmesg 출력 로깅

## 5. Authentication Logging
- 로그인에 문제가 있는지 확인하는데 매우 유용
- /var/log/auth.log
  - 사용자 로그인 및 사용된 인증 방법과 같은 시스템 인증 로그가 포함됨.

  - 샘플 정보
    > Jan 31 10:37:50 icebox pkexec: pam_unix(polkit-1:session): session opened for user root by (uid=1000)


## 6. Managing Log Files
- logrotate를 사용하여 효율적인 디스크 공간 관리
- logrotate 유틸리티는 로그 관리 수행
- 로그에는 보존 할 로그의 수와 로그를 지정하여 공간을 절약하기 위해 로그를 압축하는 방법 등을 지정할 수있는 구성 파일이 있음.
-  logrotate 도구는 보통 cron을 하루에 한 번 실행하고 구성 파일은 /etc/logrotate.d에 있음.
