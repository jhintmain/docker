
## 3장 . 도커이미지 만들기

## 학습 ##
- 도커 컨테이너도 별도의 환경 변수를 가질 수 있다 > 호스트 컴퓨터의 것 아니고 도커가 부여해줌
- Dockerfile은 application을 패키징하기 위한 간단한 스크립트이다
- 도커 이미지는 이미지 레이어가 모인 논리적 대상
- 레이어는 도커 엔진의 캐시에 저장된 물리적 파일
- 이미지 레이어는 여러 이미지와 컨테이너에서 공유됨.
  - 이미지 레이어는 재사용될수 있지만 수정할 수는 없다
- Dockerfile의 인스트럭션은 각각 하나의 이미지 레이어와 1:1연결되어있다. 인스트럭션의 결과ㄱ가 이전빌드와 같아면 캐시된레이어를 재사용한다 > 똑같은 인스트럭션 실행하는 낭비줄임
  - 도커가 캐시에 일치하는 레이어가 잇는지 확인하는데 Hash를 이용함
  - 캐시 불일치로 실행되는 인스트럭션이 있다면 그 이후 인스트럭션은 달라진게 없어도 실행된다.
  - 때문에, 잘 수정하지 않은 인스트럭션을 앞으로, 잘 수정되는 인스트럭션을 뒤로 배치 해야한다.
- 명령어 옵션
    - env (e) {KEY}={VALUE}
    - tag (t) : 이미지 태그 설정
  
- 명령어
  - docker images build --tag web-ping : 이미지 빌드
  - docker images build -t web-ping:v2 : 이미지 빌드2-버전추가 (*버전 미설정시 기본 latest)
  - docker image history web-ping : 이미지 히스토리 확인
  
## Dockerfile 인스트럭션
- FROM : 이미지 시작점. 모든 이미지는 다른 이미지로부터 출발
- ENV :  환경 변수값지정. key=value 형태
- WORKDIR :  컨테이너안 파일시스템에 디렉터리를 만들고, 해당 디렉터리를 작업 디렉터리로 지정
- COPY : 로컬 파일시스템을 컨테이너안 이미지로 복사
- CMD : 컨테이너 실행시 실행할 면령어 지정
- ENTRYPOIN : CMD와 같음
- RUN : 빌드중 컨테이너안 명령어를 실행(?_?)
- EXPOSE : 호스트 컴퓨터 open포트

## 실습 ##
# 1. 대화형으로 컨테이너 실행
dockcon run -it --name ch03-lab diamol/ch03-lab

# 문서수정 후 exit
vi ch03.txt
exit;

# 생성된 컨테이너 확인
docker ps -a
이름 : ch03-lab 컨테이너 확인

# docker container help로 쓸만한 명령문 찾기
- commit      Create a new image from a container's changes

# 컨테이너로 이미지 만들기
dockcon commit ch03-lab diamol/ch03-lab:new

# 이미지 확인
docker images

# 생성된 이미지로 컨테이너 실행
docker run -it diamol/ch03-lab:new

# 수정된 ch03.txx가 잇는지 확인
