
## 2장 . 도커의 기본적인 사용법

## 학습 ##
- 컨테이너를 실행할때 마다 컨테이너내부의 컴퓨터 이름/IP는 바뀐다
- 가상머신과 docker의 공통점/차이점
    - 공통점 : application이 실행될 독립적이 환경이 생긴다
    - 차이점 : 가상머신은 docker와 달리 호스트 컴퓨터의 os를 공유하지 않고 별도 os를 필요로함 (가상머신 하나 올릴때마다 os설치해야함)

- 명령어 옵션
    - interactive(i) : 컨테이너 접속상태
    - tty (t) : 터미널 세션을 통해 컨테이너 조작
    - detach (d) : 백그라운드에서 실행
    - publish (p) : 컨테이너 포트를 호스트 컴퓨터에 공개
  
- 명령어
    - docker container logs : 컨테이너에서 수집된 모든 로그 출력
    - docker container inspect : 컨테이너 상세 정보 출력
    - docker container stats :실행중인 컨테이너의 상세 정보 확인
    - docker container rm --force $(docker container ls --all --quiet) : 모든컨테이너 삭제
    - $() 문법은 괄호안 명령의 출력을 다른 명령으로 전달
## 실습 ##

# docker container 실행 (웹서버 띄우기)
dockcon run -d -p 80:80 diamol/ch02-hello-diamol-web

#  docker --help 로 사용 가능한 명령문 찾아보기
Commands:
attach      Attach local standard input, output, and error streams to a running container
commit      Create a new image from a container's changes
_**cp          Copy files/folders between a container and the local filesystem**_
create      Create a new container
diff        Inspect changes to files or directories on a container's filesystem
events      Get real time events from the server
export      Export a container's filesystem as a tar archive
history     Show the history of an image
import      Import the contents from a tarball to create a filesystem image
inspect     Return low-level information on Docker objects
kill        Kill one or more running containers
load        Load an image from a tar archive or STDIN
logs        Fetch the logs of a container
pause       Pause all processes within one or more containers
port        List port mappings or a specific mapping for the container
rename      Rename a container
restart     Restart one or more containers
rm          Remove one or more containers
rmi         Remove one or more images
save        Save one or more images to a tar archive (streamed to STDOUT by default)
start       Start one or more stopped containers
stats       Display a live stream of container(s) resource usage statistics
stop        Stop one or more running containers
tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE
top         Display the running processes of a container
unpause     Unpause all processes within one or more containers
update      Update configuration of one or more containers
wait        Block until one or more containers stop, then print their exit codes

# 수정할 index 파일 로컬에 생성
vi ch02_new_index.html

# 로컬에 있는 index.html 파일 컨테이너 내부로 복사
dockcon cp ch02_new_index.html blissful_goodall:/usr/local/apache2/htdocs/index.html