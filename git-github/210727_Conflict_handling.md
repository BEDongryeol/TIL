## git을 통한 협업 시 충돌 상황 해결
- 충돌 상황 해결 방법 설명을 위해 고의적인 충돌을 생성하겠습니다.

### 충돌 상황 생성
1. branch 생성
- 분기점을 생성하는 시점입니다.
- 생성한 branch가 main과 같은 상태를 갖게되는 시점입니다.


2. branch  main (file 수정 & add & commit)
- branch main에서 파일을 수정하면 생성된 branch와 차이가 생기게 됩니다.


3. 새로운 branch (file 수정 & add & commit)
- 새로 생성한 branch에서도 파일을 수정, add, commit을 해줍니다.


이 시점에서 'bracn main'의 상태와 '새로운 branch'에 파일 상태가 달라집니다.


4. branch main에서 파일 실행
- 상태가 달라진 상황에서 파일을 실행하면 오류가 발생합니다.
- Merge conflict ~ (어떠한 파일에서 Merge충돌이 났는지 알려줍니다.)


5. 해당 파일 실행
- Merge conflict가 발생한 파일을 오픈합니다.
- HEAD에서 생성한 코드, 새로운 branch에서 생성한 코드를 `=====`로 구분하여 보여줍니다.


6. 선택
- 두 코드 중 하나를 선택 (필요 없는 코드와 특수문자를 지워주고 저장합니다.)
- 재조합 (코드를 조합하고 싶은 경우 원하는 코드를 작성합니다.)


7. File add & commit
- ```git add <filename>``` 으로 파일을 staging 해줍니다.
- ```git commit``` 파일 이름 없이 commit하여 줍니다.


8. Done

