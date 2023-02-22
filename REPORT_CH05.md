
## 5장 . 도커 허브 등 레지스트리에 이미지 공유하기

## 학습 ##
- 도커는 소프트웨어 배포 기능을 내장하고 있음.(이미지 없어도 알아서 다운받아주는)
- 이미지 전체 이름(image reference)은 chd 4가지 요소로 구성된다.
  - 레지스트리도메인 (기본은 docker.id)| 이미지 작성자 계정이름 | 이미지 레포지토리이름(보통 어플리케이션이름) | 이미지 태그 (기본은 latest)
- 직접 개발한 애플리케이션 이미지를 관리하려면 image reference의 모든 값을 사용해야한다
- 규모가 큰 회사는 사내 네트워크나 전용 클라우드 환경에 자사 도커 레지스트리를 별도로 꾸리는 경우가 많다
- docker 허브에 push하기전 계정 생성 필수
  - https://hub.docker.com 
  - username : parkjihea
  
- 명령어 옵션
    - restart : 컨테이너 자동 재시작
  
- 레지스트리 http 허용 설정
  - /etc/docker/daemon.json 파일을 열어 시처럼 작성합니다. 없을 경우 생성하면 됩니다.
  - {
    "insecure-registries" : ["{local-domain}:{5000}"]
    }
  - sudo systemctl restart docker
  - 확인 : docker info  > Insecure Registries 확인
  
- 이미지 태그 효율적 사용
  - 기본적 방법 [major].[minor].[path]
  - [path] 수정시, 버그 수정뿐 기능은 같음
  - [minor] 수정시, 기능 추가만 잇음
  - [major] 수정시, 완전히 다른 기능을 가진다
  
- 직접 만든 dockerfile 에는 정확한 버전을 지정해 주는 것이 좋다.

- 도커허브는 레지스트리의 신뢰성을 위하 검증된 퍼블리셔와 공식 이미지 제도를 도입했다.
  - 검증된 퍼브리셔는 도커허브에 이미지를 배포하는 큰 기벙을 검증된 퍼블리셔롤 지정하고 이들이 배포하는 이미지는 취약점탐지,승이절차를걸쳐 공개된다
  - 공식이미지는 주로 오픈소시로 해당 프로젝트 개발팀과 도커가 함계 이미지를 관리한다. 역시 취약적 탐색을 마치고 주기적으로 업데이트 된다.
  
- 골든이미지란?
  - 자신이 선호하는 기반 이미지로 전환하는것.
  - 공식이미지를 기반 이미지로 삼아 인증서나 환경 설정값 등 자신이 필요한 설정을 추가한것이다.


## 실습 ##
# gallery/ui 모든 이미지 push 
docker image push registry.local:5000/gallery/ui -a

# gallery/ui 태그 목록 확인
curl http://localhost:5000/v2/gallery/ui/tags/list

# 이미지 매니페스트 확인
curl --head http://registry.local:5000/v2/gallery/ui/manifests/latest \
-H 'Accept: application/vnd.docker.distribution.manifest.v2+json'

# 이미지 삭제 요청
curl -X DLETE http://registry.local:5000/v2/gallery/ui/manifests/sha256:80130a9e1e64ee65aa563d810b1e3b59571a36025635278308d8359b3592fa9f

