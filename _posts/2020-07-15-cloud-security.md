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

* 하드웨어 : 시스템 기반을 구성하는 물리적인 요소, 서버 장비 본체나 데이터를 저장하기 위한 스토리지, 전원 장치 등 + 하드웨어 설치 하는 데이터 센터의 설비(소방, 보안 건물 설비 등) 도 포함됨  

* 네트워크 : 시스템 이용자가 원격지에서 액세스할 수 있도록 서버들을 연결하기 위한 요구사항, 라우터, 스위치, 방화벽 등과 같은 네트워크 장비나 그것들을 연결하기 위한 케이블 배선 등도 포함됨  

* OS(운영체제) : 하드웨어나 네트워크 장비를 제어하기 위한 기본 소프트웨어, 리소스, 프로세스를 관리  

* 미들웨어 : 서버 os 상에서 서버가 특정 역할을 다하기 위한 기능을 갖고 있는 s/w  

## 클라우드와 온프레미스  

### 온프레미스(on-premises)  

자사에서 데이터센터를 보유하고 시스템 구축부터 운용까지 모두 직접 수행하는 형태. 설치형  

### 퍼블릭 클라우드  

외부에 공개되어 사용자들이 인터넷을 경유하여 사용할 수 있는 클라우드. 인프라에 대한 초기 투자가 필요 없음. 이용한 시간이나 데이터 양에 따라 요금을 지불하는 형태. 시스템 이용량이 증가하였을 때 인프라 기반을 쉽게 증설(오토 스케일링) 가능  

* scale up 수직 확장  

* scale out 수평 확장을 비용적 측면에서 좀 더 권장함  

* scale up의 비용 > scale out 비용  

### 프라이빗 클라우드  

특정 기업 그룹에게만 제공되는 클라우드 서비스  

### 하이브리드 클라우드  

퍼블릭, 프라이빗 클라우드를 혼합해 사용하는 서비스  

### 클라우드가 적합한 케이스  

* **트래픽의 변동이 많은 시스템**  

  * 트래픽 양에 따라 시스템 기반의 서버 사양이나 네트워크 대역을 가늠하는 설계(사이징)이 어려운 시스템은 단기간에 쉽게 증설 가능한 클라우드 시스템으로 구성하는 것이 유리  

* **재해 대책으로 해외에 백업을 구축하고 싶은 시스템**  

* **서비스를 빨리 제공하고 싶은 시스템**  

### 온프레미스가 적합한 케이스  

* **높은 가용성이 요구되는 시스템**  

* **기밀성이 높은 데이터를 다루는 시스템**  

* **특수한 요구사항이 있는 시스템**  

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

### 서버 장비  

* CPU : 프로그램의 설계나 처리 등을 수행하는 전자회로 부품을 말함. 코어 수가 많을 수록 연산을 동시에 처리할 수 있는 수가 늘어남  

* 메모리 : CPU가 직접 액세스할 수 있는 기억장치(주기억장치). 비싸고 빠름  

* 스토리지 : 데이터베이스에 기록하는 데이터 등과 같은 영구적인 데이터를 저장하는 디바이스(보조기억장치)

### 네트워크 주소  

* MAC 주소(물리 주소 / 이더넷 주소) : 네트워크 인터페이스 카드(NIC)나 무선 랜 칩과 같은 네트워크 부품에 물리적으로 할당되는 48비트 주소, 앞 24비트는 제조업체 식별 번호, 뒷 24비트는 고유 번호 -> OSI 2계층 데이터 링크 계층에서 사용  

* IP 주소 : 네트워크에 연결된 컴퓨터나 네트워크 장비에 할당되는 식별 번호, IPv4 -> 8비트씩 4개로 구분된 32비트 주소  

### OSI 참조 모델과 통신 프로토콜  

OSI 참조 모델 : 통신 프로토콜을 7계층으로 나누어 정의

* 계층 번호 / 대표 프로토콜 / 대표 통신 기기  

7.응용 계층 / HTTP, DNS, SMTP, SSH / 방화벽, 로드 밸런서  

6.표현 계층  

5.세션 계층  

4.전송 계층 / TCP, UDP  

3.네트워크 계층 / IP, ICMP / 라우터, L3 스위치  

2.데이터 링크 계층 / Ethernet / L2 스위치, 브리지  

1.물리 계층 / / 리피터 허브  

### 방화벽  

* 패킷 필터형 : 통과하는 패킷을 포트 번호나 IP 주소를 바탕으로 필터링하는 방법  

* 애플리케이션 게이트웨이형 : 패킷이 아니라 애플리케이션 프로토콜 레벨에서 외부와의 통신을 대체 제어하는 것, **프록시(대리) 서버**라고 함  

### 라우터 / L3 스위치  

* 2개 이상의 서로 다른 네트워크 간을 중계하기 위한 통신 장비  

* 경로 선택 기능  

* 라우터에 설정된 테이블 바탕으로 경로 설정하는 정적 경로(static route)  

* 라우팅 프로토콜에서 설정된 동적 경로(dynamic route)  

## 운영체제  

하드웨어나 네트워크를 제어하는 것이 OS의 역할  

### Linux  

리누스 토발즈가 개발한 Unix 호환 서버 OS  

* 리눅스 커널  

OS의 코어로써, 메모리 관리, 파일 시스템, 프로세스 관리, 디바이스 제어 등 OS로서 하드웨어나 애플리케이션 소프트웨어를 제어하기 위한 기본적 기능을 갖고 있는 소프트웨어  

* 리눅스 배포판  

Debian계열, RedHat 계열 등등이 존재  

### Linux 커널  

* 하드웨어 제어에 관한 OS의 핵심이 되는 기능(응용프로그램, 메모리, 파일시스템, 장치 관리)을 담당  

* 컴퓨터의 모든 자원 초기화 및 제어  

* 디바이스 관리 : 하드웨어(CPU/메모리/디스크/입출력)를 디바이스 드라이버라는 소프트웨어를 이용해 제어  

* 프로세스 관리 : 리눅스는 명령을 실행할 때 해당 프로그램 파일에 쓰여 있는 내용을 읽어 들여 메모리상에 전개한 후 메모리 상의 프로그램(프로세스)을 실행  

  * 이러한 프로세에 PID라는 식별자를 붙여 관리함  

  프로세스의 실행을 위해 필요한 CPU를 효율적으로 할당하는 역할을 하고 있음  

* 메모리 관리 : ㅍ로세스 실행 시 메모리상에 프로그램이 전개됨과 동시에 프로그램 안에서 이용하는 데이터도 메모리 상에 전개됨  

* shell을 통해 조작  

  * 애플리케이션 실행/정지/재실행  

  * 환경변수 관리  

  * 명령 이력 관리(명령 히스토리)  

  * 명령 실행 결과 표시 및 파일 출력  

### Linux 파일 시스템  

* VFS(Virtual File System) : 가상 파일 시스템이라는 장치를 사용하여 데이터에 대한 투과 액세스를 가능하게 함, VFS에서는 각 디바이스를 파일로 취급  

### Linux 보안 기능  

* 계정에 대한 권한 설정 : 시스템을 이용할 수 있는 사용자 계정에 권한 설정 가능, root 사용자 외 일반 사용자 존재, 루트 사용자는 시스템 종료나 파일 시스템의 마운트/언마운ㅌ, 앱의 설치와 같은 실행 권한을 가지며, 시스템 전체를 관리함  

* 네트워크 필터링을 사용한 보안 기능 : 리눅스가 네트워크를 경유하여 멀티 유저가 이용하는 것을 전재로 한 OS이기 때문에 네트워크에 관한 기능이 풍부함  

  * 패킷 필터링 : 패킷이 헤더부분(송신원 IP주소, 수신처 IP주소, 포트 번호 등)을 보고 조건과 일치하면 설정한 액션을 수행  

* SELinux(Security-Enhanced Linux) : 리눅스 커널에 강제 액세스 제어 기능을 추가한 기능  

### 디렉토리의 계층 구조  

* 트리 구조이며 루트 디렉토리(/) 한 개 휘하의 서브 디렉토리 여러 개  

* 중요 루트 디렉토리  

  1. /dev - 장치 파일이 담긴 디렉토리  

  2. /home - 사용자 홈 디렉토리가 생성되는 디렉토리  

  3. /opt - 추가 패키지가 설치되는 디렉토리  

  4. /root - 루트 계정의 홈 디렉토리  

  5. /usr(unix system resource) -> Windows의 Program Files와 같은 역할  

  6. /etc - 각종 설정 파일  

## 미들웨어  

### 웹 서버 / 웹 애플리케이션 서버  

* 웹서버 : 클라이언트가 보내온 HTTP 요청을 받아 웹 콘텐츠를 응답으로 반환하거나 다른 서버사이드 프로그램을 호출하는 기능을 가진 서버  

  * Apache HTTP server - 오픈소스 웹서버, 폭넓게 사용되어왔음  

  * Nginx - 오픈소스 웹서버, 소비 메모리가 적으며, 리버스 프록시 기능과 로드 밸런서 기능도 갖고 있음  

    * 리버스 프록시 - 컴퓨터 네트워크에서 클라이언트를 대신해서 한 대 이상의 서버로부터 자원을 추출하는 프록시 서버의 일종  

* DB 서버 : 시스템이 생성하는 다양한 데이터를 관리하기 위한 미들웨어  

  * 데이터의 검색, 등록, 변경, 삭제와 같은 기본 기능 외에 트랜잭션 처리 등도 포함  

  * RDB - 데이터를 2차원 표 형식으로 관리하는 데이터베이스로, 여러 표(table)를 결합하여 사용 가능  
  
    * MySQL, PostgreSQL, Oracle DB  

  * NoSQL - 병렬 분산 처리나 유연한 스키마 설정 등이 특징으로, KVS(Key-Value Store)나 도큐먼트 지향 데이터베이스, XML 데이터베이스 등이 존재  

    * 대량의 데이터 축적이나 병렬처리에 뛰어나기에 다수의 사용자 액세스를 처리할 필요가 있는 온라인 시스템에 널리 이용  

    * Redis(KVS), MongoDB(Document), Apache Cassandra(RDB에 가까움)  

* 시스템 감시 툴 : 시스템이 안정적으로 가동 되는 지 확인하기 위한 툴로, 미리 설정한 경계 값을 초과한 경우 정해진 액션을 수행함  

  * Zabbix, Datadog, Mackerel  

## 인프라 구성 관리  

하드웨어, 네트워크, OS, 미들웨어, 애플리케이션 구성 정보를 관리하고 적절한 상태로 유지하는 작업  

### IaC(Infrastructure as Code)  

* 필요성 : 인프라 구성 관리가 부족할 경우 제품 환경에서 가동 중인 인프라의 설계서나 서버의 파라미터 시트가 실제 설정 값과 달라서 문제가 발생할 수 있음  

* 장점  

  * 애플리케이션 개발에서의 소스코드 관리와 똑같이 버전 관리 소프트웨어로 변경 이력을 관리할 수 있음  

  * 인적 실수를 배제할 수 있음

  * EX) Dockerfile로 인프라 구성 정보를 기술 가능  

### 인프라 구성 관리 툴  

* Bootstrapping - OS 시작 자동화, OS 설치, 가상 환경 설정, 네트워크 구성 설정  

  * Vagrant, Kickstart  

* Configuration - OS 설정(보안/서비스 시작 등), 미들웨어의 설치 및 설정  

  * Ansible, Puppet, Chef  

* Orchestration - 컴퓨터 시스템과 애플리케이션, 서비스의 자동화된 설정, 관리, 조정을 의미, 여러 서버의 관리 자동화, 애플리케이션 배포, 서버군의 오케스트레이션  

  * Kubernetes - 컨테이션 관리 툴  

## CI/CD  

### Continuous Integration(지속적 통합)  

* 코드 수정할 때마다 테스트 실행하고 확실히 작동하는 코드를 유지하는 방법  

* Jenkins와 같은 툴이 있음  

* 코드로써 인프라 구성을 관리한다면 개발 멤버가 항상 동일한 환경에서 개발할 수 있으므로 CI에서 필요한 환경의 구성 관리가 더욱 쉽다  

### Continuous Deployment(지속적 배포)  

* 개발을 끝낸 후에 제품 환경에 애플리케이션을 배포하여 서비스를 릴리스하는 과정을 반복  

* 요건 정의->설계->코딩->테스트라는 프로세스를 거쳐 개발을 끝낸 후 제품 환경에 배포하여 서비스를 릴리스  

* 릴리스한 시점에서 사용자의 요구를 만족시키지 못했을 때가 발생할 수 있음  

* 모든 기능을 한 번에 다 만들지 않고 기능을 추가할 때 마다 앱을 제품 환경에 배포하고 이용자의 피드백에 기초하여 다음 개발 기능을 결정 -> Agile 개발 스타일  

* 짧은 사이클의 개발과 릴리스를 반복함으로써 이용자가 원하는 앱을 적시에 제공 가능  

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

* docker system info - 도커가 사용하고 있는 시스템 상황  

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
# -p (publish, 포트포워딩) Host 포트:Container 포트
# -e (environment, 환경변수 지정)
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

* 3번 노드(node3)는 도커가 아닌 로컬에 mysql 클라이언트 설치  

```shell
yum install -y mysql
# -h(접속할 호스트) node1 -> node3(mysql)
mysql -h 192.168.56.11 -u root -p

# node2의 도커에 mysql(이름:mysql2) 설치하고 node2(docker(mysql))에서 node1(mysql)에 접속
## node2에서
docker exec -it mysql2 /bin/bash
mysql -h 192.168.56.11 -u root -p
```

```shell
# 도커가 사용하고 있는 리소스 확인
docker system info
# 도커가 사용하고 있는 디스크 크기 확인
docker system df
# 시스템의 용량 확인
df -h
```

* 노드1에 웹 서버(Nginx) 구동 실습  

```shell
docker search nginx
docker pull nginx:latest
# 80(container):80(centos), Host - 18000(MacOS)
docker run --rm -d -p 80:80 --name webserver nginx:latest
# 호스트 pc에서 확인하려면
http://localhost:18000
# 컨테이너 상태 확인
docker stats webserver
# 컨테이너 로그 확인
docker logs webserver
# nginx 이미지를 나만의 태그를 달아
docker tag nginx:latest dlsrks1218/nginx:v1.0
# 도커 로그인
docker login
# 도커 허브에 내 이미지 푸시
docker push dlsrks1218/nginx:v1.0

# mysql 이미지를 내 태그 달아 푸시
docker tag mysql:5.7 dlsrks1218/mysql:v1.0
docker push dlsrks1218/mysql:v1.0
# node3에서 내 mysql 이미지 풀해서 컨테이너 생성
docker pull dlsrks1218/mysql:v1.0
## mysql 컨테이너 실행시 -e 옵션 필수
docker run --rm -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql dlsrks1218/mysql:v1.0
```

---

## 발생한 이슈  

### 1. Centos ssh 붙을때 backward, forward-word 안되서 키바인딩 추가로 해결  

```shell
# Centos ssh 붙을때 backward, forward-word 키바인딩 추가
vi ~/.bash_profile
bind '"\e\e[C": forward-word'
bind '"\e\e[D": backward-word'

# 명령어로 추가하기
echo "bind '\"\\e\\e[C\": forward-word'" >> ~/.bash_profile
echo "bind '\"\\e\\e[D\": backward-word'" >> ~/.bash_profile
source ~/.bash_profile
```

### 2. 도커 mysql 컨테이너 실행 시 -e 환경변수 옵션을 안주면 아무것도 실행안됨  

```shell
# mysql 컨테이너 실행 시 -e 옵션이 없으면 아무것도 실행안됨
# docker run --rm -d -p 3306:3306 --name mysql3 dlsrks1218/mysql:v1.0
docker run --rm -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql3 dlsrks1218/mysql:v1.0
```  
