
## 6장 . 도커 볼륨을 이용한 퍼시스턴트 스토리지

## 학습 ##
- 컨테이너속 데이터가 사라지는 이유
   - 모든 컨테이너는 독립된 파일 시스템을 같는다.
   - 같은 이미지에서 빌드해도 컨테이너마다 파일시스템은 별개.
   - ex ) 같은 이미지에서 실행해도 number1.txt과 number2.txt는 다름
     - dockcon run --name rn1 diamol/ch06-random-number
     - dockcon run --name rn2 diamol/ch06-random-number
     - dockcon cp rn1:/random/number.txt number1.txt
     - dockcon cp rn2:/random/number.txt number2.txt

## 볼륨(volume)
- 볼륨은 컨테이너간 파일 공유보단 업데이트간 상태보존을 위한 용도로 사용해야하며, 이미지에서 정의하기 보다 명시적으로 관리하는것이 좋다.
- 볼륨에 이름을 붙여 생성하고 업데이트시 다른 컨테이너로 옮겨 연결하면 된다.
- dockerfile에서의 VOLUME과 dockcon의 --volume은 별개 기능이다
   - dockcon run에서 볼륨을 지정하지 않으면 항상 새로운 볼륨이 형성된다
   - 볼륨은 무작위 식별자를 가지므로 같은 볼륨을 공유하고 싶으면 만들어서 명명하서 쓰거나 무작위실별자를 기억해야한다
   * 볼륨은 명명헤서 쓰는것이 좋다


## 바인드 마운트(bind mount)
- 호스트 스토리지를 컨테이너에 좀더 직접적으로 연결 할 수 있는 수단.
- 컨테이너와 호스트 양방향접근 가능
- 호스트 컴퓨터의 파일 시스템을 명시적으로 지정해 컨테이너 데이터로 사용 할 수 있다.
- 호스트 컴퓨터에 대한 공격을 방지하기 위해 최소한의 권한을 가진 계정으로 실행되어 접근하기 위해서는 권한 상승이 필요함

## 파일시스템 마운트의 한계
- 이미존재하는 디텍터리에 마운트 하면 마운트 원본 디렉터리가 기존 디렉터리를 완전히 대체. 이미지에 포함된 원래파일은 사용 할 수 없다.
  - ex) 
  - dockcon run diamol/ch06-bind-mount
  - > abc.txt<br>def.txt
  - dockcon run --mount type=bing,source=$(pwd)/new,target='/init' diamol/ch06-bind-mount
  - > 123.txt<br>456.txt
    - 호스트 컴퓨터의 /new 디렉토리가 이미지 원본 파일을 덮어 버렸다.
  
- 단일 파일 마운트
  - 리눅스에서는 이어쓰기가 되게 해준다. 윈도에서는 안됌.

## 명령어 ##
1. --volumes-from (v) : 볼륨 설정



## 실습 ##
1. todo-list 어플리케이셔 이미지로 컨테이너 실행해 연결된 볼룸 확인하기
   - dockcon run -d -p 80:80 --name todo1 diamol/ch06-todo-list
   - dockcon inspect todo1
   - docker volume ls
   
2. 같은 볼륨을 공유하는 컨테이너 만들기
   - dockcon run -d -p 81:80 --name todo2 diamol/ch06-todo-list
   - dockcon exec todo2 ls /data  > empty
   - dockcon run -d -p 82:80 --name todo3 --volumes-from todo1 diamol/ch06-todo-list
   - dockcon exec todo3 ls /data > todo-list.db

3. 볼륨생성 후 컨테이너 연결
   - docker volume create todo-list
   - dockcon run -d -p 83:80 -v todo-list:/data --name todo-v1 diamol/ch06-todo-list
   - 사이트 들어가서 데이터 추가후 컨테이너 삭제
   - dockcon run -d -p 83:80 -v todo-list:/data --name todo-v2 diamol/ch06-todo-list
   - 사이트 들어가보면 아까 추가한 데이터 그대로 확인가능

4. 마운트하여 컨테이너 연결
   - mkdir ./databases
   - dockcon run --mount type=bind,source=$source,target=$target -d -p 84:80 diamol/cho06-todo-list
   - ls ./databases >  todo-list.db

5. 연습문제
   - 마운트를 추가하여 diamol/ch06-lab이미지로 컨테이너 실행후 현재 등록된 할 일 확인
   - 