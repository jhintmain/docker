
## 9장 . 컨테이너 모니터링으로 투명성있는 어플리케이션 만들기

## 학습 ##
- 이번장의 목표는 도커를 이용한 체계적인 모니터링을 하는것.
- [프로메테우스]로 어플리케이션 컨테이너에서 측정된 수치를 수집후 [그라파나]를 사용하여 수치를 시간화 대시보드 형태로 구성. > 모두 오픈 소스이다 
- 프로메테우스/그라파나 를 사용하기 위해선 설정이 몇개 필요하다
  - metrics-addr : 127.0.0.1:9323
  - experimental : true
# 프로메테우스
- 어플리케이션 컨테이너에서 측정된 수치를 수집.

## 실습 ##
- ch04의 3가지 기능 프로메테우스와 연동
- docker-compose.yml 파일 작성
- version: "3.7"

services:
accesslog:
image: diamol/ch09-access-log
ports:
- "82:80"
networks:
- app-net

iotd:
image: diamol/ch09-image-of-the-day
ports:
- "81:80"
networks:
- app-net

image-gallery:
image: diamol/ch09-image-gallery
ports:
- "80:80"
depends_on:
- accesslog
- iotd
networks:
- app-net

prometheus:
image: diamol/ch09-prometheus
ports:
- "9090:9090"
environment:
- DOCKER_HOST=${HOST_IP}
networks:
- app-net

networks:
app-net:
external:
name: nat

