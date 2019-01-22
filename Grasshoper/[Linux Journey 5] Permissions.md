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

$ sudo chown patty myfile
This command will set the owner of myfile to patty.

### Modify group ownership

$ sudo chgrp whales myfile
This command will set the group of myfile to whales.

### Modify both user and group ownership at the same time 
If you add a colon and groupname after the user you can set both the user and group at the same time.

$ sudo chown patty:whales myfile

## Umask

## Setuid

## Setgid

## Process Permissions

## The Sticky Bit
