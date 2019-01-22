# [Linux Journey 5] Permissions
## File Permissions
- 다른 사용 권한이나 파일 모드가 있음.
  - 예시
    > $ ls -l Desktop/  
    > drwxr-xr-x 2 pete penguins 4096 Dec 1 11:45 .

- 파일 사용 권한의 네 부분
  - 파일 형식: 사용 권한의 첫 번째 문자로 표시됨.
    - (위의 예시에서) 이 경우 d에 해당. d: directory 
  - 실제 사용 권한(나머지 세 부분)
  - 처음 3 비트는 사용자 권한, 그룹 권한 및 기타 권한
    - 차별화하기 쉽도록 파이프 추가
- 각 문자별 권한 알아보기
  > d | rwx | r-x | r-x 
  
  |명령어|기능|
  |:-----:|:-----:|
  | r | 읽기 가능 |
  | w | 쓰기 가능 |
  | x | 파일 실행 가능 (기본적으로 실행 가능한 프로그램) |
  | - | 권한 없음(비어있는 부분에 따라서 생각) |

- 위 예시에서 살펴보면,
  - pete: 읽기, 쓰기, 실행 모든 권한을 갖고 있음을 알 수 있음.
  - 그룹인 penguins: 읽기 및 실행만 권한을 갖고 있음을 알 수 있음.
  - 제 3자인 다른 모든 사용자: 일기 및 실행 권한을 갖고 있음을 알 수 있음.

## Modifying Permissions
- chmod 명령어를 사용하여 권한 변경
- 더하기 기호(+) 및 빼기 기호(-)를 사용해 추가하거나 제거 가능

### Adding permission bit on a file 
- 사용자 파일에 대한 권한 추가

|명령어|기능|
|:-----:|:-----:|
| chmod u+x myfile | 사용자 권한 세트에 실행 권한 비트를 추가하여 myfile에 대한 사용 권한 변경 |


### Removing permission bit on a file 
- 사용자 파일에 대한 권한 제거

|명령어|기능|
|:-----:|:-----:|
| chmod u-x myfile | 사용자 권한 세트에 실행 권한 비트를 제거하여 myfile에 대한 사용 권한 변경 |

### Adding multiple permission bits on a file 
-  한 번에 모든 권한을 변경 가능

|명령어|기능|
|:-----:|:-----:|
| chmod ug + w | 여러 권한 비트 추가 |

### The numerical representations
- 숫자 형식을 사용하여 사용 권하는 변경 방법
  
  |숫자|기능|
  |:-----:|:-----:|
  | 4 | 읽기 권한 |
  | 2 | 쓰기 권한 |
  | 1 | 실행 권한 |

- 예시
  > $ chmod 755 myfile
  
  - 755가 모든 세트에 대한 권한 포함.
    
    |숫자의 합|구성|읽기 권한|쓰기 권한|실행 권한|권한|
    |:-----:|:-----:|:-----:|:-----:|:-----:|
    |7|4 + 2 + 1|O|O|O|사용자|
    |5|4 + 1|O|X|O|그룹|
    |5|4 + 1|O|X|O|다른 모든 사용자|


## Ownership Permissions

### Modify user ownership
- 사용자 소유권 수정

|명령어|기능|
|:-----:|:-----:|
| sudo chown patty myfile | myfile의 소유자를 patty로 설정 |

### Modify group ownership
- 그룹 소유권 수정

|명령어|기능|
|:-----:|:-----:|
| sudo chgrp whales myfile | myfile 그룹의 소유자를 whales로 설정 |

### Modify both user and group ownership at the same time 
- 동시에 사용자 및 그룹 소유권 수정

|명령어|기능|
|:-----:|:-----:|
| sudo chown patty:whales myfile | 뒤에 콜론과 그룹 이름을 추가하여 사용자와 그룹을 동시 설정 |

## Umask
- 기본 권한 세트 변경시 사용
- 숫자 형식을 사용하는 권한에서 볼 수 있는 3비트 권한 세트 사용

|명령어|기능|
|:-----:|:-----:|
| umask 021 | "(새 파일의 기본 권한이) 사용자 -> 모든 것에 액세스하기 / 그룹 -> 쓰기 권한을 없애기 / 다른 사용자 -> 실행 권한 없애기" |

## Setuid
- 사용자가 자신이 아닌 프로그램 파일의 소유자로서 프로그램을 실행 가능할 수 있도록 해줌.
- 비밀 번호 변경을 하고 싶으면? 
  - -> passwd 명령 사용하기
- 암호 명령은 무엇을 하고 있는지? 가장 중요한 것은 / etc / shadow 파일을 수정하는 것!
  > ls -l /etc/shadow
  > -rw-r----- 1 root shadow 1134 Dec 1 11:45 /etc/shadow
  - 이 파일이 root가 소유하고 있는지? 소유한 파일을 어떻게 수정할 수 있는지?
- 다른 권한 세트 살펴 보기
  > ls -l /usr/bin/passwd
  > -rwsr-xr-x 1 root root 47032 Dec 1 11:45 /usr/bin/passwd
  - 자세히 보면 **s** 라는 문자가 새로 생긴 것을 볼 수 있음.
  - 이 권한 비트는 SUID임.
  - 파일이 SUID로 설정되어 있으면, 프로그램을 실행 한 사용자가 실행 권한뿐만 아니라 파일 소유자의 사용 권한도 얻을 수 있음.(이 경우 root)
  - 기본적으로 사용자가 암호 명령을 실행하는 동안 루트로 실행됨.
  - 따라서 passwd 명령 실행 시, 보호된 파일에 액세스 가능.
  - 그 비트를 제거하면 / etc / shadow를 수정할 수 없으므로 암호가 변경된다는 것을 알 수 있음.


### Modifying SUID(SUID 수정)
- UID 사용 권한을 수정하는 두 가지 방법

  |방법|명령어|
  |:-----:|:-----:|
  | Symbolic way(상징적 방법) | sudo chmod u+s myfile |
  | Numerical way(수치적 방법) | sudo chmod 4755 myfile |

  - SUID는 4로 표시되고 권한 집합에 선행. 
  - SUID가 대문자 S 로 표시되는 것을 볼 수 있음. 이는 SUID가 여전히 똑같지만 실행 권한이 없음을 의미함.

## Setgid
- 프로그램이 해당 그룹의 구성원인 것처럼 실행 가능
- 예시
  > ls -l /usr/bin/wall
  > -rwxr-sr-x 1 root tty 19024 Dec 14 11:45 /usr/bin/wall
  - 권한 비트가 그룹 권한 집합에 있음을 알 수 있음.

### Modifying SGID(SGID 수정)
- SGID의 숫자 표현 -> 2
  > sudo chmod g+s myfile
  > sudo chmod 2555 myfile

## Process Permissions(프로세스 권한)
- 이 부분에 관해서는 "[윈도우의 계정과 권한 체계](https://m.blog.naver.com/PostView.nhn?blogId=scvpark&logNo=70163718517&proxyReferer=https%3A%2F%2Fwww.google.com%2F)" 이 사이트 참고!!


## The Sticky Bit
- 소유자 또는 루트 사용자만 파일을 삭제하거나 수정할 수 있음을 의미함.
- 예시
  > ls -ld /tmp
  > drwxrwxrwxt 6 root root 4096 Dec 15 11:45 /tmp
  - 끝 부분에 특별한 권한 비트인 **t** 를 볼 수 있음.
  - 모두가 / tmp 디렉토리에있는 파일을 추가, 기록, 수정 가능.
  - 하지만, root만이 / tmp 디렉토리를 삭제할 수 있음.

### Modify sticky bit(고정 비트 수정)
- 고정 비트에 대한 숫자 표현 -> 1
  > sudo chmod +t mydir
  > sudo chmod 1755 mydir
