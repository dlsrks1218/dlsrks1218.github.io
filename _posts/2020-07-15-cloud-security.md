---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 2일차"
date: 2020-07-15 20:10:11 -0400
background: '/img/posts/05.jpg'
---

## 시스템 기반의 구성 요소  

### 시스템 기반  

애플리케이션을 가동시키기 위해 필요한 하드웨어나 OS, 미들웨어 등과 같은 인프라를 칭함  

### 기능 요구사항(functional requirement)  

시스템의 기능으로서 요구되는 사항. 시스템이나 소프트웨어에서 무엇을 할 수 있는지 모아놓은 것  

### 비기능 요구사항(non-functional requirement)  

시스템의 성능이나 신뢰성, 확장성, 운용성, 보안 등과 같은 요구사항. 시스템 기반 지식이 필요함  

### 구성 요소  

* 하드웨어  

* 네트워크  

* OS(운영체제)

* 미들웨어  

## 클라우드와 온프레미스  

### 온프레미스(on-premises)  

자사에서 데이터센터를 보유하고 시스템 구축부터 운용까지 모두 직접 수행하는 형태. 설치형  

### 퍼블릭 클라우드  

외부에 공개되어 사용자들이 인터넷을 경유하여 사용할 수 있는 클라우드. 인프라에 대한 초기 투자가 필요 없음. 이용한 시간이나 데이터 양에 따라 요금을 지불하는 형태. 시스템 이용량이 증가하였을 때 인프라 기반을 쉽게 증설(오토 스케일링) 가능  

* scale up 수직확장  

* scale out 수평 확장을 비용적 측면에서 좀 더 권장함  

* scale up의 비용 > scale out 비용  

### 프라이빗 클라우드  

특정 기업 그룹에게만 제공되는 클라우드 서비스  

### 하이브리드 클라우드  

퍼블릭, 프라이빗 클라우드를 혼합해 사용하는 서비스  

## 클라우드가 적합한 케이스  

### 트래픽의 변동이 많은 시스템  

트래픽 양에 따라 시스템 기반의 서버 사양이나 네트워크 대역을 가늠하는 설계(사이징)이 어려운 시스템은 단기간에 쉽게 증설 가능한 클라우드 시스템으로 구성하는 것이 유리  

### 재해 대책으로 해외에 백업을 구축하고 싶은 시스템  

### 서비스를 빨리 제공하고 싶은 시스템  

## 온프레미스가 적합한 케이스  

### 높은 가용성이 요구되는 시스템  

### 기밀성이 높은 데이터를 다루는 시스템  

### 특수한 요구사항이 있는 시스템  

## 시스템 기반의 구축/운용 흐름  

### 과정  

시스템화 계획, 요구사항 정의 -> 인프라 설계 -> 인프라 구축 -> 운용  

### 시스템 구축 계획 및 요구사항 정의 단계  

시스템 구축 범위 선정, 인프라 요구사항 정의, 예산 책정, 프로젝트 체게화, 레거시 시스템과의 연계, 시스템 마이그레이션 계획  

### 인프라 설계 단계  

인프라 아키텍처, 네트워크 토폴로지 설계, 장비 선택, 조달, OS, 미들웨어 선택, 시스템 운용, 시스템 마이그레이션 설계  

### 인프라 구축 단계  

네트워크 부설, 서버 설치, OS, 미들웨어 셋업, 애플리케이션, 라이브러리 설치, 테스트, 시스템 릴리스, 마이그레이션  

### 운용 단계  

서버 프로세스, 네트워크, 리소스, 배치 job 모니터링, OS, 미들웨어 버전 업그레이드, 시스템 장애 시 대응  

## 하드웨어와 네트워크 기초 지식

* **페이지 12 ~ 25 추가하기**  

## 미들웨어  

### 웹 서버 / 웹 애플리케이션 서버  

### 데이터베이스 서버  

* RDB  

* NoSQL(Redis, MongoDB, Apache Cassandra)  

## 인프라 구성 관리  

하드웨어, 네트워크, OS, 미들웨어, 애플리케이션 구성 정보를 관리하고 적절한 상태로 유지하는 작업  

### IaC(Infrastructure as Code)  


### 인프라 구성 관리 툴  

* Bootstrapping - OS 시작 자동화, OS 설치, 가상 환경 설정, 네트워크 구성 설정  

  * Vagrant  

* Configuration - OS 설정(보안/서비스 시작 등), 미들웨어의 설치 및 설정  

  * Ansible

* Orchestration - 여러 서버의 관리 자동화, 애플리케이션 배포, 서버군의 오케스트레이션  

  * Kubernetes - 컨테이션 관리 툴  

## CI/CD  

### Continuous Integration  

코드 수정할 때마다 테스트 실행하고 확실히 작동하는 코드를 유지하는 방법  

### Continuous Deployment(Delivery)  

개발을 끝낸 후에 제품 환경에 애플리케이션을 배포하여 서비스를 릴리스하는 과정을 반복  

---

## Docker  

컨테이너 기반의 오픈소스 가상화 플랫폼  

* 백엔드 프로그램, 데이터베이스, 메시지 큐 -> 컨테이너로 추상화 가능  

### 가상화 방식  

* 호스트형 가상화  

  * OS를 가상화하여 **무겁고 느림**

* 하이퍼바이저형 가상화  

  * CPU를 가상화 시술 이용 방식 -> Kernel-based Virtual Machine  

  * 전체 OS를 가상화 하지 않음, 호스트 형식에 비해 속도 향상  

  * **추가적 OS**는 여전히 필요, 성능 문제  

* 컨테이너형 가상화

  * 프로세스 격리 -> 리눅스 컨테이너  

  * CPU나 메모리는 프로세스에 필요한 만큼 추가로 사용  

  * **성능 손실 거의 없음**  

  * 컨테이너들 사이는 서로 영향을 주지 않음  

  * 컨테이너 생성 속도 빠름(1~2초 내)  

### 도커 이미지  

* 컨테이너 실행에 필요한 파일과 설정 값 등을 포함 -> 상태값이 없고 Immutable(영속적, 특정화된 값이 포함 되어있지 않다)  

* 실체화 -> 컨테이너  

* 허브에 등록하거나 레지스트리 저장소를 직접 만들어 관리  

* Layer 저장 방식 : 유니온 파일 시스템을 이용해 **여러 Layer를 하나의 파일 시스템으로 사용 가능**  

### Dockerfile  

* 도커 이미지를 생성하기 위한 스크립트 파일  

* 자체 Domain Specific Language, DSL 사용 -> 이미지 생성 과정 기술  

* 소스와 함께 버전 관리가 되며, 누구나 수정 가능  

### 명령어 -> 수업자료 13pg  

* docker <~> prune -a -> 사용되지 않는 리소스를 찾아서 삭제  

  * <~>에 들어갈 수 있는 명령어(image, container, network, volume, system)  



---

## 실습  

```shell
# 우분투 공식 이미지 설치
docker pull ubuntu:latest
# 현재 작동중인 프로세스(컨테이너) 확인
docker ps
# 컨테이너 삭제
docker rm <컨테이너 ID>
# 설치된 이미지 확인
docker images
# 이미지 삭제
docker rmi <이미지 ID>
# 해당하는 이미지가 없을때 run하면 설치 먼저 수행 후 실행
# -it -> interactive한 과정 수행(shell 프로그램 수행과 같은 의미)
docker run -it ubuntu:latest /bin/bash
# 컨테이너에 대한 정보 확인
docker inspect <컨테이너 ID>
# --rm(컨테이너 종료 시 삭제)
docker run --rm -it ubuntu:16.04 /bin/bash
# 컨테이너 멈추기
docker stop <컨테이너 ID>
# 멈춘 컴테이너 다시 시작
docker start <컨테이너 ID>
```  

### Ubuntu 컨테이너  

```shell
# 호스트 이름 확인 (컨테이너 이름이 호스트 이름이 됨)
cat /etc/hostname
# 우분투 컨테이너 종료
exit
# 호스트 ip 주소 확인
hostname -i
```  

### MySQL 컨테이너

```shell
# mysql 공식 이미지 설치
docker pull mysql:5.7
# -p (포트포워딩) Container 포트:Host 포트
# -e (환경변수 지정)
# --name (컨테이너 이름)
# 마지막은 이미지이름(tag 포함)
docker run --rm -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
# mysql 컨테이너 접근 & mysql 실행
docker exec -it <컨테이너 ID or 컨테이너 이름> mysql -u root -p
```  

* Volume 마운트를 통해 데이터를 유지

### Tensorflow 컨테이너  

```shell
# -d(detach, 분리하여 백그라운드에 돌아가도록 함)
docker run --rm -d -p 8888/8888 teamlab/pydata-tensorflow:0.1
```

### VM에 도커 설치  

* <https://github.com/joneconsulting/k8s/blob/master/kubernetes_install.md>  

```shell
# Vagrantfile가 있는 디렉토리 -> k8s
cd k8s
vagrant status
vagrant up
vagrant ssh node1, 2, 3
```  

* 모든 노드에 대해 수행할 것  

```shell
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
# 모든 시스템 디렉토리의 값 읽기
sysctl --system
# 쿠버네티스 리포지토리 설정
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
# 쿠버네티스 설치
yum install -y --disableexcludes=kubernetes kubeadm-1.15.5-0.x86_64 kubectl-1.15.5-0.x86_64 kubelet-1.15.5-0.x86_64
############################################################################################################################
# 1. 쿠버네티스 설정 - Master
## 실행
systemctl enable --now kubelet
## 초기화
kubeadm init --pod-network-cidr=10.96.0.0/16 --apiserver-advertise-address=192.168.56.10 ###<- 설치 성공 후 아래 커맨드 부분을 복사 (생성되는 값은 본인의 환경에 따라 다름)>
kubeadm join 192.168.56.10:6443 --token x1qogf.3i1d8zc267sm4gq8 \
--discovery-token-ca-cert-hash sha256:1965b56832292d3de10fc95f92b8391334d9404c914d407baa2b6cec1dbe5322
## 환경변수 설정
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl get pods --all-namespaces # 모든 pods가 Running 상태인지 확인 
## Calico 기본 설치(Kubernetes Cluster Networking plugin)
kubectl apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml
kubectl get pods --all-namespaces
## Calico는 기본적으로 192.68.0.0/16 대역 사용하기 때문에, IP가 중복 될 경우에는 위의 방법 말고(kubectl apply) calico.yaml 파일을 다운로드 후 코드 수정, Calico 설치
curl -O https://docs.projectcalico.org/v3.8/manifests/calico.yaml  
sed s/192.168.0.0\\/16/10.96.0.0\\/12/g -i calico.yaml
kubectl apply -f calico.yaml

# 2. 쿠버네티스 노드 연결 - Node
## 연결(Master의 init 작업에서 복사 한 커맨드를 사용)
kubeadm join 192.168.56.10:6443 --token x1qogf.3i1d8zc267sm4gq8 \
--discovery-token-ca-cert-hash sha256:1965b56832292d3de10fc95f92b8391334d9404c914d407baa2b6cec1dbe5322  
## 연결 시 오류 발생하면 kubeadm reset 명령어로 초기화 후 다시 실행 (Node 모두 초기화)
kubeadm reset

# 3. 마스터에서 확인 및 대시보드 설치 - Master
## 확인
kubectl get nodes
## 대시보드 설치
kubectl apply -f https://raw.githubusercontent.com/kubetm/kubetm.github.io/master/sample/practice/appendix/gcp-kubernetes-dashboard.yaml
## Proxy 설정
nohup kubectl proxy --port=8001 --address=192.168.56.10 --accept-hosts='^*$' >/dev/null 2>&1 &
## 접속
http://192.168.56.10:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/

# 4. 테스트
## Pod 실행
kubectl run nginx-test --image=nginx --port 80 --generator=run-pod/v1
## Service 실행
kubectl expose pod nginx-test 
kubectl get services
## Service Type 변경
kubectl edit service nginx-test # (ClusterIp -> NodePort)
## 확인 (port는 service에서 forwarding 된 port 사용)
http://192.168.56.10:30039/ # (<- port forwarding)
http://192.168.56.11:30039/ # (<- port forwarding)
```  
