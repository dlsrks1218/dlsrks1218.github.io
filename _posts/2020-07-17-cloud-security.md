---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 4일차"
date: 2020-07-17 20:10:11 -0400
background: '/img/posts/01.jpg'
---

## Docker  

### 레지스트리  

* 도커 이미지를 저장할 수 있는 공간  

* 도커 설치시 기본 레지스트리는 Docker Hub로 지정되어 있음  

* 로컬 환경에 직접 구축가능한 프라이빗 레지스트리가 존재함, 공개하지 않을 정보, 이미지를 담기 위함  

  * docker hub에 있는 registry 이미지를 다운받아 프라이빗 네트워크 안에 구축 가능  

### 베이스 이미지 및 manager 노드 추가 실습  

* 교재 pg 159~160 실습, Ubuntu16.04 이미지에 Nginx 패키지 설치  

```shell
mkdir work2 && cd work2

vi index.html
# index.html 안에 작성할 내용
<h1>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
Ubuntu16.04 이미지를 베이스로 하여 Nginx 웹서버를 구동하겠습니다
</h1>

cd /root/work2

vi Dockerfile
# Dockerfile 안에 작성할 내용
FROM ubuntu16.04
RUN apt-get update && apt-get install -y -q nginx
COPY index.html /var/www/html
CMD ["nginx", "-g", "daemon off;"]

docker build -t webap .

docker run -d -p 80:80 webap:latest
# 태그 변경
docker tag docker-nginx localhost:5000/example/docker-nginx
# 로컬 레지스트리에 push
docker push localhost:5000/example/docker-nginx
# 레지스트리에 등록된 리포지토리 확인
curl -X GET http://localhost
```

* **CentOS에 도커 설치 후 레지스트리 이미지 풀**  

```shell
# 추가된 매니저 노드에 도커 설치
# 루트계정 비밀번호 설정
su -
# SELinux 설정
setenforce 0
sestatus
sed -i 's/^SELINUX=enforcing$/SELINUX=permissive/' /etc/selinux/config
# 방화벽 해제
systemctl stop firewalld && systemctl disable firewalld
systemctl stop NetworkManager && systemctl disable NetworkManager
# 스왑 비활성화
swapoff -a && sed -i '/ swap / s/^/#/' /etc/fstab
# iptables 커널 옵션 활성화
cat <<EOF >  /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
EOF
# 모든 시스템 디렉토리의 값 읽기
sysctl --system
# Centos update
yum update -y
# Docker 설치
yum install -y yum-utils device-mapper-persistent-data lvm2
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
yum update -y && yum install docker-ce -y
# 직접 사용자 및 비밀번호 입력하기
useradd dockeradmin
passwd dockeradmin
password: dockeradmin
usermod -aG docker dockeradmin
systemctl enable --now docker && systemctl start docker
```

```shell
# manager 노드에 레지스트리 이미지 풀
docker pull registry
# 레지스트리 컨테이너 실행
docker run --rm -d -p 5000:5000 --name registry registry
# 생성된 레지스트리 확인
curl -X GET http://localhost:5000/v2/_catalog
# manager 노드에 node1의 Dockerfile과 index.html을 복사
# 도커파일을 빌드하여 docker-nginx라는 이름의 이미지 생성
docker build -t docker-nginx .
```

* node3에서 maganer노드에 구축된 프라이빗 레지스트리에서 이미지를 다운받아 실행  

```shell
# 호스트네임(manager) 포트(5000)의 레지스트리로부터 이미지 다운
docker pull manager:5000/example/docker-nginx
# https 설정을 안하면 에러가 나므로 http로 우회시켜 주거나 인증서 등록이 필요함
# https -> http로 사용 가능케 등록
vi /etc/docker/daemon.json
{
  "insecure-registries" : ["manager:5000"]
}

systemctl restart docker

docker run --rm -d -p 80:80 manager:5000/example/docker-nginx

http://localhost:38000
```

* 추후 문제 낼 수 있는 것  

  * 우분투 혹은 센트os 이미지에서 nginx 혹은 nodejs를 설치해보세요  

## Docker 네트워크  

* 네트워크 내의 여러 컨테이너에서 각자 실행중인 컨테이너에 대해 inspect해보면 IPAddress는 같지만 NetworkID와 EndpointID가 다르면 서로 통신이 안됨(**다른 네트워크**)  

  * 컨테이너 생성시 고유의 NetworkID와 EndpointID가 부여됨  

### 도커 네트워크 실습  

```shell
# docker inspect <컨테이너>
"Networks": {
    "bridge": {
        "IPAMConfig": null,
        "Links": null,
        "Aliases": null,
        "NetworkID": "1dfc2a8303ee0c158e060187c4dabef75fc1e28f97f73d0c3da39c00625225c4",
        "EndpointID": "9062519d939850eb3221a9aa91d4c5024f08e578f4823b6c3b2995d6087ce16a",
        "Gateway": "172.17.0.1",
        "IPAddress": "172.17.0.2",
        "IPPrefixLen": 16,
        "IPv6Gateway": "",
        "GlobalIPv6Address": "",
        "GlobalIPv6PrefixLen": 0,
        "MacAddress": "02:42:ac:11:00:02",
        "DriverOpts": null
    }
}
```

* 도커 네트워크 목록 확인  

```shell
## manager
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
1dfc2a8303ee        bridge              bridge              local
1323130baeb3        host                host                local
942b2a880cce        none                null                local

## node1
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
44da4bac921f        bridge              bridge              local
e8533d3c2dae        host                host                local
aab36896fedd        none                null                local

## node2
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
9a030f6558f6        bridge              bridge              local
017a497a2779        host                host                local
cc1fb761ea8e        none                null                local

## node3
docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
59e8a2244602        bridge              bridge              local
3e832c7dc19e        host                host                local
0220de45fbf9        none                null                local

```  

### 도커 컴포즈  

* 도커 앱을 정의하고 실행하는 도구  

* Tool for defining and running multi-container Docker applications  

  * 각 컨테이너별로 별도의 Docker 명령어 실행 (long-winded arguments)  

  * 한 번에 여러 개의 컨테이너 동시 실행  

* 추가 설치가 필요하며 yaml파일 형식을 이용  

* yaml -> key, value 형태로 작성  
  
  * YAML format에 Docker 생성 , 설정 관련 된 작업을 작성해 놓은 Script 파일  

  * 들여쓰기 depth가 같아야 문법 오류가 생기지 않는다  

* 도커 커맨드 혹은 복잡한 설정을 쉽게 관리하기 위한 도구  

### 도커 컴포즈 실습  

```shell
# Docker compose 설치 - 모든 노드에 수행
curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
docker-compose -version
```

* 레지스트리 컨테이너를 도커 컴포즈를 통해 만들어보기  

  * 도커 이미지를 다운받고 옵션을 부여하여 컨테이너 실행하는 과정을 도커 컴포즈로 한방에 수행  

```shell
vi docker-compose.yml
version: "3.1"
services: 
  registry:
    # docker run --name registry
    container_name: registry
    # FROM registry:latest
    image: registry:latest
    # docker run -p 5000:5000
    ports: 
      - 5000:5000
    # docker run -v ./registry-data:/var/lib/registry
    volumes: 
      # 현재 디렉토리에 없는 폴더면 생성도 해줌
      - "./registry-data:/var/lib/registry"

# 도커 컴포즈 실행
docker compose up
# -d - detach 모드
docker-compose up -d
# 도커파일을 다시 빌드
docker-compose up --build
# 도커 컴포즈 멈추기
docker-compose down
```

* 도커 컴포즈를 통해 생성한 프라이빗 레지스트리에 이미지 푸시해보기  

```shell
# 기존에 생성한 이미지(localhost:5000/example/docker-nginx) 프라이빗 네트워크에 푸시
docker push localhost:5000/example/docker-nginx
docker-compose down
docker-compose up -d
curl -X GET http://localhost:5000/v2/_catalog
```

## 도커 스웜  

### 도커 스웜을 통한 오케스트레이션 - 현재는 쿠버네티스로 옮겨감  

여러 도커 호스트를 클러스터로 묶어주는 컨테이너 오케스트레이션  

* Compose - 여러 컨테이너로 구성된 도커 애플리케이션을 관리(주로 단일 호스트)  

* Swarm - 클러스터 구축 및 관리(주로 멀티 호스트)  

* **Service** - 스웜에서 클러스트 안의 서비스(컨테이너 하나 이상의 집합)을 관리  

* **Stack** - 스웜에서 여러 개의 서비스를 합한 전체 애플리케이션을 관리  

* **클러스터링을 통해 이중화 가능**  

### 클러스터링  

여러 대의 서버나 하드웨어를 모아서 한 대처럼 보이게 하는 기술을 의미  

* 가용성(Availability)  

시스템에 오류나, 고장이 발생해도 다른 정상적인 서버나 하드웨어가 대신해서 처리를 계속해주어 신뢰성 확보  

* 확장성(Scalability)  

클러스터화하여 처리를 분산시킴으로써 높은 처리 성능을 얻을 수 있으며 오토스케일 기능을 지원함  

## 도커 스웜 실습  

* manager 노드에 Swarm 설정 및 나머지 노드는 Swarm 참여  

```shell
# manager 노드 - 스웜모드 활성화, advertise-addr(사용할 ip 주소 지정)
# 명령어 수행 결과 중 docker swarm join --token ~~ 이하 명령어를 기억해두고 워커가 될 노드들에 수행해야함
docker swarm init --advertise-addr 192.168.56.14
# node 1,2,3 - 생성된 스웜에 worker로 참여
docker swarm join --token SWMTKN-1-0hx4n5xvqcxnoxiq8dpdy0gubrcxxefu7acjib7tnzcpfjbrnt-bvvmtm052olmpxvc34veraqm8 192.168.56.14:2377
# 이미 조인한 워커일 경우 docker swarm leave를 해주어야함
# manager 노드에서 클러스터링 확인
docker node ls

# node 1,2,3 - 프라이빗 레지스트리에서 이미지 다운하기 위해 http 우회용 파일 추가
vi /etc/docker/daemon.json
{
  "insecure-registries" : ["manager:5000"]
}

systemctl restart docker

docker pull manager:5000/example/docker-nginx
```  

* 매니저 노드에서 도커 서비스 단위로 배포(강사님 교안에 스웜 코드 실습시 붉은 글씨만 사용할것)  

```shell
# manager노드에서 복제본 1개씩 두고 서비스를 생성
# 리더나 워커 중 아무 곳에 생성될건데 리더일 확률이 높음
docker service create --replicas 1 -p 80:80 --name my-nginx manager:5000/example/docker-nginx
# 생성한 서비스 확인 -> 도커 스웜에 의해 만들어진 이름
docker service ls
# my-nginx 서비스의 레플리카 셋을 3개 만들어 스케일 아웃하겠다 -> 리더나 워커 통틀어 3개의 서비스 생성됨
docker service scale my-nginx=3

# manager 노드에서 복제한 서비스(my-nginx)들 확인
docker service ps my-nginx
# 서비스 삭제
docker service rm my-nginx

# 모든 노드에서 스웜 종료
docker swarm leave
```

* 스웜 구성 실습!!

```shell
# manager에서 docker-compose를 통해 registry 컨테이너 실행
docker-compose up
# node 1,2,3 - docker image 다운받기 -> 안해도 서비스 생성때 레플리카셋 늘리면 자동으로 다운됨
# docker pull manager:5000/example/docker-nginx
# manager - 스웜 생성
docker swarm init --advertise-addr 192.168.56.14
# node 1,2,3 - swarm 참여
docker swarm join --token SWMTKN-1-0gudqx54or73foxsbbzwf062yh6tf8f4e8vx2dm45hqstghwsd-8oori6gtf7bth8abuj0zyezwp 192.168.56.14:2377
# manager - docker service 생성
docker service create --replicas 1 -p 80:80 --name my-nginx manager:5000/example/docker-nginx
# manager에서 서비스 확인
docker service ls
# manager에서 스케일 아웃
docker service scale my-nginx=6
# manager에서 스웜 참가 노드 확인
docker service ps my-nginx
```

## 이슈  

### 1. ubuntu 이미지 베이스로 nginx 설치시 index.html이 존재할 루트 디렉토리가 /usr/share/nginx/html 이 아니라 /var/www/html 이므로 도커파일 작성시 주의할 것  

### 2. 프라이빗 레지스트리 구축해서 올려놓은 이미지를 다운 받으려고 할 때, 기본으로 https 프로토콜 사용함. https 설정을 안하면 에러가 나므로 http로 우회시켜 주거나 인증서 등록이 필요함  

```shell
vi /etc/docker/daemon.json
{
  "insecure-registries" : ["myregistrydomain.com:5000"]
}
systemctl restart docker

docker pull <image of myregistry>
```

### 3. 스웜 구성한 서비스는 매니저에서 종료해주어야함(워커에서 컨테이너 종료는 의미가 없음), 서비스 종료시 구축했던 프라이빗 레지스트리 컨테이너를 구성한 docker-compose의 상태가 Exited가 됨 -> 왜일까  

### 4. docker swarm leave는 워커들 부터 수행 후 리더는 --force 옵션이 필요함  

```shell
docker service rm <서비스명>
# node 1,2,3 부터 leave 후 리더는 --force 옵션 필요
docker swarm leave
docker swarm leave --force
```
