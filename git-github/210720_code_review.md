## Git에서의 코드리뷰

### 코드리뷰의 필요성
- 프로젝트 진행 시 실험적인 코딩 등 완전하지 않은 작업을 branch main에 영향을 주지않고 수행할 수 있다.  

### 코드 리뷰 방법
 1. reviewer의 repository에 작성 시 'username' 폴더를 생성한다.
 2. 'user'의 repository에 작성 시 날짜, 주제 등의 이름을 가진 repo를 생성한다. 

### 절차
 1. 작업을 위한 branch를 생성한다.
	```git branch 'branch이름'```
 2. 현재 작업중인 branch를 확인하고, 생성한 branch로 이동시켜준다.
	- 작업중인 branch 확인 : ```git branch``` 
	- 생성한 branch로 이동 : ```git switch 'branch이름'```
 3. 원하는 작업을 수행 및 저장 후 github의 저장소로 보낸다.
	- Staging Area로add :  ```git add 파일명```
	- Local repository로  commit : ```git commit 파일명```
	- Remote repository(github)로 push : ```git push origin branch이름``` 

 4. 코드 리뷰 요청
	- repository > seeting > Manage access > Invite a collaborator
	- Collaborator의 feedback 등을 통해 코드가 완전해지면 branch main으로 Merge 할 수 있다. 

