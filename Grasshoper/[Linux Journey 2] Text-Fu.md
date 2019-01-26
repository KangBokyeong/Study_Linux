# [Linux Journey 2] Text-Fu
## 1. stdout(Standard Out), stderr(Standerd Error)
- 출력 역할을 함.
- [표준출력(STDOUT), 표준에러(STDERR)](https://sarc.io/index.php/forum/tips/551-linux-stdout-stderr-dev-null)
- stdout의 리다이렉터: >> // 1>
- stderr의 리다이렉터: 2>


## 2. stdin(Standerd In)
- 쓰기 역할을 함.
- 리다이렉터: < // 0>

## 3. pipe and tree
- 파이프 연산자: |

|명령어|기능|
|:-------:|:-----:|
|  파이프 연산자 |명령의 표준 출력을 가져 와서 표준 입력을 다른 프로세스로 만들 수있게 해줍니다.|
| tee |내 명령의 출력을 서로 다른 두 스트림에 쓰고 싶을 때 사용|

## 4. env(Environmet)
- 현재 설정한 환경 변수에 대한 많은 정보를 출력
- 셸 및 기타 프로세스에서 사용할 수있는 유용한 정보가 들어 있음.


## 5. cut
- 자르기

|명령어|기능|
|:-------:|:-----:|
| cut -c 1 |첫 번째 문자를 자름(c 뒤에 숫자는 몇 번째 문자인지를 나타냄.)|
| cut -f 1 |첫 번째 필드를 자름(f 뒤에 숫자는 몇 번째 필드인지를 나타냄.)|

## 6. paste
- 문자나 문자열 붙이기

|명령어|기능|
|:-------:|:-----:|
| paste -s |TAB 기준으로 붙혀짐(옆에 아무것도 구분자가 없으면 TAB이 기본임.)|
| paste -d ' ' -s |space를 기준으로 붙혀짐(-d는 구분자(delimiter)를 뜻함.)|

## 7. head
- 맨 위(처음)로 기준을 잡아서 기본 10줄을 보여줌.

|명령어|기능|
|:-------:|:-----:|
| head | 기본 처음 10줄을 보여 줌.|
| head -n 15 |15줄까지 보여 줌.|
| head -c 15 |15글자까지 보여 줌.|

## 8. tail
- 맨 아래(끝)로 기준을 잡아서 기본 10줄을 보여줌.

|명령어|기능|
|:-------:|:-----:|
| tail | 기본 끝 10줄을 보여 줌.|
| tail -n 15 |15줄까지 보여 줌.|
| tail -f | 커지면서 파일을 따라 감.|

## 9. expand and unexpand
- expand: TAB을 공백으로 변경 시 사용
- 파일 저장 시: expand old.txt> new.txt
- unexpand: 공백을 TAB으로 변경 시 사용
- 사용법: unexpand -a new.txt

## 10. join and split
- join: 파일 내용 합치기(줄 별로 합쳐짐.)
- split: 파일 내용 분리시키기

## 11. sort
- 파일 내용 정렬

|명령어|기능|
|:-------:|:-----:|
| sort file1.txt | 기본 오른차순 정렬|
| sort -r file1.txt |거꾸로 정렬(-r은 reverse를 의미 함.)|

## 12. tr(Translate)
- 대소문자 변환

## 13. uniq(Unique)
- 중복 제거 출력

|명령어|기능|
|:-------:|:-----:|
| uniq reading.txt |중복을 제거하여 출력|
| uniq -c reading.txt |각 단어와 그 단어의 중복 개수 동시 출력|
| uniq -u reading.txt |중복되지 않은 단어만 출력|
| uniq -d reading.txt |중복된 단어만 출력|

_ 정렬하고 사용을 하는 것 권장. (sort 명령어와 같이 사용하기.) 그렇지 않으면 중복 제거를 하지 못함.

## 14. wc and nl
- wc: 파일의 단어 총 수 표시(라인 수, 단어 수, 바이트 수 각각 표시)
- 특정 필드의 카운트 보려면, -l, -w 또는 -c 사용.

|명령어|기능|
|:-------:|:-----:|
| wc -l| 라인 수만 표시|
| wc -w| 단어 수만 표시|
| wc -c| 바이트 수만 표시|

- nl(number lines): 파일의 행수 표시

## 15. grep
- 특정 패턴과 일치하는 문자에 대한 파일 검색
- 다른 명령어와 함께 사용 
