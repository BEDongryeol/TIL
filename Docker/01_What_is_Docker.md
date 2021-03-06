> ## Docker

도커는 리눅스의 응용 프로그램들을 `프로세스 격리 기술`들을 사용해 컨테이너로 실행하고 관리하는 오픈 소스 프로젝트이다.

도커는 리눅스에서 OS 수준 가상화의 `추상화` 및 `자동화 계층`을 제공한다.


> ### Container

컨테이너 하나를 애플리케이션 하나로 생각할 수 있다.

각 컨테이너는 독립적으로 CPU, Memory, Disk, Network 등을 부여받으며, 기능을 추가시킬 때도 `독립적으로` 기능을 추가하고 배포할 수 있다.

> ### Docker Host

Linux 커널을 가지고, docker를 실행할 수 있는 플랫폼을 말한다. 

Host 입장에서 컨테이너는 프로세스로 실행시킨다.

> ### Container Image

컨테이너가 실행중인 프로세스라면 이미지는 실행될 수 있는 바이트 코드라고 이해하면 편하다.

내부에 `Image Layer`, `Source Code Layer` 등 여러 레이어로 구성되어 있다.


> ### 명령어

`docker pull [image]` : 이미지 다운로드

`docker images` : 이미지 목록 확인

`docker ps` : 실행 중인 컨테이너 확인
	- 컨테이너는 프로세스이므로 ps로 명명한 거 같다.

`docker run -d -p 80:80 --name web [image]` : image 실행
	- `-d` : 백그라운드로 실행
	- `-p 80:80` : 포트 포워딩
	- `--name` : 컨테이너의 이름 명명

