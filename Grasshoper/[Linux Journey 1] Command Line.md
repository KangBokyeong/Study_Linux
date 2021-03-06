# [Linux Journey 1] Command Line
## 1. pwd(print working directory)
- 현재 어떤 디렉토리에 위치해 있는지 확인 시 사용
### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51187824-054b9c80-1920-11e9-83fb-a42245765a73.PNG" width="60%">

# 2. cd(change directory)
- 디렉토리 변경 시 사용

|명령어|기능|
|:-----:|:-----:|
|cd .|current directory|
|cd ..|parent directory|
|cd ~|home directory|
|cd -|바로 이전 기록의 디렉토리로|
### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51187848-0f6d9b00-1920-11e9-800c-c2a06f80e219.PNG" width="60%">

## 3. ls(list directories)
- 현재 디렉토리의 리스트 확인(어떤 파일이 있는지 등)

|명령어|기능|
|:-----:|:-----:|
|ls -a|리스트 나열|
|ls -l|개수까지 자세히 확인|
|ls -la|숨겨져 있는 파일까지 모두 보여줌|
### 사용 예

<img src="https://user-images.githubusercontent.com/44868847/51187851-11375e80-1920-11e9-9bba-f1c5784ace37.PNG" width="60%">
<img src="https://user-images.githubusercontent.com/44868847/51187854-12688b80-1920-11e9-8818-da3013d1e09a.PNG" width="60%">

## 4. touch
- 빈 파일 생성 시 사용
### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51187857-1399b880-1920-11e9-9a9b-c01ac45b22a3.PNG" width="60%">


## 5. file
- [자세한 사용법 및 정의는 여기서](https://korbillgates.tistory.com/161)

## 6. cat
- [여기 자세한 사용법 및 정의가 잘 나와 있음.](http://www.incodom.kr/Linux/%EA%B8%B0%EB%B3%B8%EB%AA%85%EB%A0%B9%EC%96%B4/cat)

## 7. less
- 파일 안을 볼 수 있음.

|명령어|기능|
|:-----:|:-----:|
|q|less 종료 시 사용. 다시 shell 화면으로 돌아 옴.|
|Page up, Page down, Up and Down|방향키와 페이지 업다운 키를 이용하여 커서 위치 조정|
|g|텍스트 파일의 처음으로 이동|
|G|텍스트 파일의 끝으로 이동|
|/search|텍스트 문서에서 특정 텍스트 검색 가능.검색할 단어 앞에 '/'사용.|
|h|도움이 필요할 때 사용|

## 8. history
- 이전에 했던 기록 살펴 보기
- 'clear'를 사용해 터미널에 있는 기록 삭제

## 9. cp(copy)
- 파일이나, 디렉토리 복사

|명령어|기능|
|:-----:|:-----:|
| * |모든 단일 문자 또는 문자열을 나타내는데 사용|
| ? |한 문자를 나타내는데 사용|
| [] |괄호 안에있는 문자를 나타내는데 사용|
| cp -r |디렉토리 복사, 디렉토리 안에 있는 디렉토리들과 파일을 복사|
| cp -i |덮어쓰기, i(: interactive)|

## 10. mv(move)
- 파일명, 디렉토리명 바꾸기
- 파일 다른 곳에 옮기기(하나 이상도 가능)

|명령어|기능|
|:-----:|:-----:|
| mv |위에 정리한 내용의 기능을 함(일반적으로)|
| mv -i |cp와 마찬가지로 덮어 쓸 때 사용|
| mv -b |이전의 이름으로 백업|

## 11. mkdir(make directory)
- 현재 존재하지 않는 디렉토리 생성
- 한 번에 여러 개의 디렉토리 생성 가능 

|명령어|기능|
|:-----:|:-----:|
| mkdir |위에 정리한 내용의 기능을 함(일반적으로)|
| mkdir -p |하위 디렉토리까지 생성 하는데 사용|

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252187-88332c80-19de-11e9-9b45-90a21dc8aa8b.PNG" width="60%">

## 12. rm(remove)
- 파일이나 디렉토리 삭제

|명령어|기능|
|:-----:|:-----:|
| rm -f | 권한이 있는 한 사용자에게 메시지 표시 없이 그냥 파일 삭제 시 사용|
| rm -i |실제로 파일이나 디렉토리를 제거 할 것인지 묻는 프롬프트가 표시|
| rm -r | 모든 파일과 서브 디렉토리를 제거 시 사용|
| rmdir | 디렉토리 삭제 시 사용|

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252183-85d0d280-19de-11e9-8a05-7e31120e209d.PNG" width="60%">

## 13. find
- 파일, 디렉토리 등 검색
- 검색 할 디렉토리, 검색 할 디렉토리를 지정 필수
- 찾으려는 파일 유형 지정 가능(아래 명령어 입력하여)

|명령어|기능|
|:-----:|:-----:|
| -type | 타입으로 검색하고자 할 때 사용 |
| -name | 폴더명으로 검색하고자 할 때 사용 |
|-type -name|타입과 폴더명으로 검색하고자 할 때 사용|

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252190-89645980-19de-11e9-8fca-851cccaea26f.PNG" width="60%">

## 14. help
- 도움말 출력

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252201-9123fe00-19de-11e9-838d-8d39564fcf1a.PNG" width="60%">
<img src="https://user-images.githubusercontent.com/44868847/51252203-92edc180-19de-11e9-9736-566d7069d9f2.PNG" width="60%">

## 15. man
- 메뉴얼 출력

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252209-95501b80-19de-11e9-8b40-8da7463d246d.PNG" width="60%">
<img src="https://user-images.githubusercontent.com/44868847/51252211-997c3900-19de-11e9-872a-356df307503f.PNG" width="60%">

## 16. whatis
- 프로그램에 대한 간단한 설명 제공

### 사용 예
<img src="https://user-images.githubusercontent.com/44868847/51252213-9b45fc80-19de-11e9-9f11-ad46f0170779.PNG" width="60%">

## 17. alias
- 명령의 별칭 생성
- 별칭 이름을 지정 후 명령에 설정
- 반대로 unalias를 이용하면 별칭이 사라짐.
- 직접 사용해 보기!! 

## 18. exit
- 터미널 종료
- 'logout'도 사용 가능

