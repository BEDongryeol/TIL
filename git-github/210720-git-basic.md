# git
 - 파일 하나로 버전관리와 이력을 관리할 수 있는 도구, system.
 - 비선형구조인 branch를 활용하여 안정적인 협업이 가능하다.

## git의 objects
 - Blob : 파일 하나의 내용에 대한 정보 (물품에 비유된다)
 - Tree : Blob이나 subtree의 메타데이터 (디렉토리 위치, 속성, 이름 등) 
 - Commit : 커밋하는 순간의 Snapshot

# git repository clone으로 시작하기
 - git은 원격 저장소(remote repository)로 로컬 저장소와 clone을 통해 연동하여 사용할 수 있다.

## 기본 설정
```zsh
git config --global user.name "github 사용자 이름"
git config --global user.email "github email 주소"
git config --global core.editor "vim" 
git config --global core.pager "cat"
git config --global init.defaultBranch "main"
```
* 내용을 삭제하고 싶을 경우에
```git config --global --unser user.nameg``과 같이 사용할 수 있다.

* 저장 사항이 궁금한 경우
```git config --list```로 설정을 확인할 수 있다.

* 수정이 필요한 경우
```vi ~/.gitconfig```에서 수정할 수 있다.
 
## 로컬 저장소에 clone 생성
1. 나의 github 저장소의 주소를 복사한다.
2. (MAC OS 기준) 터미널에서 로컬 저장소를 생성하고 싶은 경로로 이동하여 폴더(clone)를 생성한다.
3.  터미널에서 ```git clone 복사한 주소```를 입력한다.
	처음 접속 시 github email 주소와 패스워드 입력 필요
4. 생성한 작업을 staging area로 add한다.
	 ```git add filename```
5. add한 작업을 local repository로 commit한다. 
	```git commit filename```

6. commit이 완료된 작업을 github(remote repository)로 push한다.
	```git push origin branch_name```

