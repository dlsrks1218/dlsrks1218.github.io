---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 5일차"
date: 2020-07-20 20:10:11 -0400
background: '/img/posts/02.jpg'
---

## Proxy 서버  

프록시 서버(영어: proxy server 프록시 서버)는 **클라이언트가 자신을 통해서 다른 네트워크 서비스에 간접적으로 접속할 수 있게 해 주는 컴퓨터 시스템이나 응용 프로그램**을 가리킨다. 서버와 클라이언트 사이에 중계기로서 대리로 통신을 수행하는 것을 가리켜 '프록시', 그 중계 기능을 하는 것을 프록시 서버라고 부른다.  

프록시 서버 중 일부는 프록시 서버에 요청된 내용들을 캐시를 이용하여 저장해 둔다. 이렇게 캐시를 해 두고 난 후에, 캐시 안에 있는 정보를 요구하는 요청에 대해서는 원격 서버에 접속하여 데이터를 가져올 필요가 없게 됨으로써 전송 시간을 절약할 수 있게 됨과 동시에 불필요하게 외부와의 연결을 하지 않아도 된다는 장점을 갖게 된다. 또한 외부와의 트래픽을 줄이게 됨으로써 네트워크 병목 현상을 방지하는 효과도 얻을 수 있게 된다.  

* 목적  

  * 익명으로 컴퓨터를 유지 (주로 보안을 위하여)  
  
  * 캐시를 사용하여 리소스로의 접근을 빠르게 하기 위해. 웹 프록시는 웹 서버로부터 웹 페이지를 캐시로 저장하는 데 흔히 쓰인다.  
  
  * 네트워크 서비스나 콘텐츠로의 접근 정책을 적용하기 위해. (이를테면 원치 않는 사이트를 차단)  
  
  * 사용률을 기록하고 검사하기 위해 (이를테면 회사는 인터넷 이용을 파악)  
  
  * 보안 및 통제를 뚫고 나가기 위해  
  
  * 바이러스 전파, 악성 루머 전파, 다른 정보들을 빼낼 목적으로  
  
  * 역으로 IP추적을 당하지 않을 목적으로  
  
  * 전달에 앞서 악성 코드를 목적으로 전달된 콘텐츠를 검사하기 위해  
  
  * 밖으로 나가는 콘텐츠를 검사하기 위해 (데이터 유출 보호)  
  
  * 지역 제한을 우회하기 위해  

## Docker  

### Docker Swarm  

* 리더 : 워커들에게 작업을 나누어 주는 매니징 역할을 수행  

* 워커 : 리더로부터 나누어 받은 작업을 수행  

* swarm에서 사용하는 포트  

  * TCP port 2377 : cluster managerment 통신에 사용  

  * TCP/UDP port 7946 : node간의 통신에 사용  

  * TCP/UDP port 4789 : overlay network 트래픽에 사용  

* replicas에 해당하는 노드를 지워도 스웜에 의해 갯수가 유지됨  

### Docker Stack  

* stack : **하나 이상의 서비스를 그룹으로 묶은 단위**, 애플리케이션 전체 구성 정의  

  * 여러 서비스를 함께 다룰 수 있음  

  * 스택을 사용해 배포된 서비스 그룹은 **overlay 네트워크**에 속함  

### Docker Network  

* Host 네트워크 : host 드라이버로 제공하는 네트워크로 컨테이너를 위한 별도 네트워크없이 호스트와 동일한 네트워크를 사용한다  

* Bridge 네트워크 : **하나의 호스트 내의 컨테이너들끼리 통신**할 수 있도록 연결해주는 네트워크  

* Overlay 네트워크 : **다른 호스트끼리 통신**할 수 있도록 여러 네트워크 종류를 혼합해 사용하는 네트워크  

  * 스택을 위한 오버레이 네트워크는 스택에 속한 서비스가 서로 통신할 때 사용된다  

### Ingress  

* Ingress는 외부로부터 서버 내부로 유입되는 네트워크 트래픽을, egress는 서버 내부에서 외부로 나가는 트래픽을 의미함  

* 단일 진입점을 만들어 하나의 통로를 만들어주는 것(Gateway라고도 함)  

* 오버레이 네트워크 중 ingress 네트워크는 **호스트(노드)에서 서비스로의 포워딩을 담당**한다. 외부에 포트를 공개한 서비스를 스웜에 배포하면 ingress 네트워크에 서비스를 참여시키고 다음의 두 IP를 할당한다.  

  * 서비스에 대한 가상 IP  

  * 서비스의 각 컨테이너에 대한 IP  

## 실습  

### 1. 오버레이 네트워크 기반 ingress 서비스 스택 배포  

* 시나리오  

1. 리더(manager), 워커(node1, 2, 3) 생성
2. 노드끼리 통신을 위해 overlay network (ch03) 생성
3. /ingress/docker-compose.yml로 프라이빗 레지스트리 구축
4. /simple-web/Dockerfile을 빌드하여 simple-web 이미지 생성 후 프라이빗 레지스트리에 푸시
5. /ingress/ch03-webapi.yml안에 정의된 대로 nginx 서비스(nginx-proxy 이미지)와 api 서비스(프라이빗 레지스트리의 simple-web 이미지)를 가지는 스택을 생성하고 배포, 확인

```shell
# 오버레이 네트워크 생성
## attachable -> 생성할 네트워크 이름
docker network create --driver=overlay --attachable ch03

### vagrant up된 노드 중 manager에 파일 만들기
cd /root/work/
tree -L 2 ingress
ingress
├── ch03-webapi.yml
├── docker-compose.yml
├── registry-data
│   └── docker
└── simple-web
    ├── Dockerfile
    └── hello.js
###

# 프라이빗 레지스트리 다시 구축
docker-compose up -d

# ingress/simple-web의 도커파일로 이미지 빌드
docker build -t simple-web .
docker tag simple-web manager:5000/example/simple-web
# 프라이빗 레지스트리에 이미지 푸시
docker push manager:5000/example/simple-web

# 도커 스택 배포, -c <파일명> <스택이름>
docker stack deploy -c /stack/ch03-webapi.yml simple-web
# 각 서비스들에 대한 상세 정보 확인
docker stack services simple-web
# 어디에 배포되었나 확인
docker stack ps simple-web
# 프록시 서버를 통한 통신 확인 - Round Robin 방식으로 호출할때마다 호출되는 서비스가 바뀜
## 아래 명령어의 결과는 실제 노드에 실행중인 컨테이너 ID(hostname)
curl -X GET http://192.168.56.13:80
curl -X GET http://192.168.56.13:80
curl -X GET http://192.168.56.13:80
# 스택 삭제
docker stack rm simple-web

# visualizer 스택 배포
docker stack deploy -c visualizer.yml visualizer
## visualizer localhost:49000 -> 9000:8080 실행
localhost:49000
```

### Docker Stack 명령어 정리  

```shell
# 현재 도커 스웜에 배포된 스택 리스트 확인
docker stack ls
# 스택에 포함된 서비스에 대한 간단 정보, 서비스 ID, 이름
docker stack services <스택 이름>
# 스택의 서비스들이 어디 노드에 배치되었는지, 서비스에 해당하는 컨테이너들의 ID와 배치된 위치 정보
docker stack ps <스택 이름>
```

### 2. 퀴즈  

```shell
work3
├── Dockerfile
└── index.html

1. 도커파일을 이미지화 하고, 프라이빗 레지스트리(my-web)에 푸시
    - 포트 8888(expose 8888)

2. docker swarm에 배포
    - replicas 4개로 배포
    - 모든 my-web 서비스는 nginx-proxy로 접속되도록 수정
    - my-web서비스는 replicas 4개
    - nginx-proxy 서비스는 replicas 2개  

# 결과로 생성되는 이미지 이름 확인 후 프라이빗 레지스트리 푸시하기 위한 태그 변경
vi Dockerfile

FROM ubuntu:16.04
RUN apt-get update
RUN apt-get install -y -q nginx
COPY index.html /var/www/html
CMD ["nginx", "-g", "daemon off;"]

/root/work
Dockerfile
index.html

docker build -t my-web .
docker tag my-web manager:5000/example/my-web

docker push manager:5000/example/my-web:1.0
curl -X GET http://manager:5000/v2/_catalog

vi ch03-my-web.yml
version: "3"
services:
    nginx:
        image: gihyodocker/nginx-proxy
        deploy:
            replicas: 4
            placement:
                constraints: [node.role != manager]
        environment:
            # nginx-proxy가 사용하는 내부 변수
            BACKEND_HOST: my-web_api:80
        depends_on:
            - api
        networks:
            - ch03
        ports:
            - 8088:80
    api:
        image: manager:5000/example/my-web
        deploy:
            replicas: 4
            placement:
                constraints: [node.role != manager]
        networks:
            - ch03
networks:
    ch03:
        external: true

docker stack deploy -c ch03-my-web.yml my-web
docker stack ls
docker stack services my-web
docker stack ps my-web
# index.html 내용 혹안
curl -X GET http://192.168.56.11:8088
# visualizer
localhost:49000
```

### 3. Swarm을 이용한 실전 애플리케이션 개발 - todoapp DB  

```shell
cd /root/work
mkdir db && cd db
mkdir -p etc/mysql/conf.d
mkdir -p etc/mysql/mysql.conf.d
mkdir -p sql

# 강사님 리포지토리의 k8s/swarm/todo/tododb/와 상태 같게 만들기
db
├── add-server-id.sh
├── Dockerfile
├── etc
│   └── mysql
│       ├── conf.d
│       │   └── mysql.cnf
│       └── mysql.conf.d
│           └── mysqld.cnf
├── init-data.sh
├── prepare.sh
└── sql
    ├── 01_ddl.sql
    └── 02_dml.sql
# tododb 이름으로 이미지 빌드
docker build -t tododb .
# 프라이빗 레지스트리 푸시용으로 태그 변경
docker tag tododb manager:5000/example/tododb
# 프라이빗 레지스트리 푸시
docker push manager:5000/example/tododb
# 프라이빗 레지스트리에 푸시한 리포지토리 확인
curl -X GET http://manager:5000/v2/_catalog

# overlay 네트워크 생성
docker network create --driver=overlay --attachable todoapp
# slave 부분은 제외한 yml파일로 배포
docker stack deploy -c todo-mysql.yml todo-mysql
# 에러시 로그 확인
docker service logs <서비스 id>

```

### 4. 혼자 실습  

```shell
docker network create --driver overlay --attachable ch04

docker build -t my-web .
docker build tag my-web manager:5000/example/my-web


vi ch04-my-web.yml
version: "3"
services:
    nginx:
        image: gihyodocker/nginx-proxy
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks:
            - ch04
        depends_on:
            - api
        ports:
            - 8088:80
        environment:
            BACKEND_HOST: my-web_api:80
    api:
        image: manager:5000/example/my-web
        deploy:
            replicas: 3
            placement:
                constraints: [node.role != manager]
        networks:
            - ch04
networks:
    ch04:
        external: true
```
