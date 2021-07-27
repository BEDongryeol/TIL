## Git을 통한 프로젝트 진행
- git을 사용하는 이유는 branch를sandbox처럼 이용할 수 있어서이다
### Branch
- 분기점을 생성하여 독립적으로 코드를 변경할 수 있도록 도와주는 모델

## Project Procedure
1. Repository의 Clone 생성
- ```git clone <Repository 주소>```

2. Branch 생성 및 이동
- ```git branch <branch 이름>```
- ```git branch``` 로 branch 조회 가능
- ```git checkout <branch 이름>```으로 branch 이동

3. Branch 내에서 작업 실행
- 기능 단위 등의 파일을 생성하고 add & commit

4. Merge
- main으로 이동하여 원하는 branch와 merge 실행
- ```git merge <branch 이름>```

5. Sandbox로 사용한 branch삭제
- ```git branch -D <branch 이름>```
