---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "그림으로 배우는 클라우드 2일차"
date: 2020-07-06 20:10:11 -0400
background: '/img/posts/02.jpg'
---

## 클라우드가 제공하는 서비스  

* 가상 서버  

  * 가상 CPU의 성능과 메모리 용량을 사용용도와 시스템의 규모에 맞게 선택  

* 로드 밸런스 기능  

* 스토리지 서비스  

  * VPN 망이나 전용선 등 높은 보안 수준의 네트워크 서비스를 이용 가능  

* 데이터베이스 서비스  

## 가상 서버에서 사용할 수 있는 옵션 기능  

* 로드 밸런서  

  * 복수개의 존을 선택하여 시스템의 이중화 및 부하의 분산처리가 가능  

* 오토 스케일  

  * 자동으로 가상 서버의 대수를 증감시키는 기능  

* 정기적인 스냅 샷  

  * 서버 디스크의 백업을 자동으로 생성  

## 클라우드를 이용한 시스템 구축의 개념  

### 시스템 구성이 클라우드 서비스에 의존한다  

* 클라우드 사업자가 제공하는 표준화 된 서비스와 결합한다 -> 클라우드 디자인 패턴  

### 장애 발생이 전제된 설계  

* 클라우드는 온프레미스와 비교했을 때, 안정적인 운영 시기로 들엇ㅆ을 때의 장애가 비교적 발생하기 쉽다고 알려져 있음  

* 장애가 발생해도 문제없이 운용할 수 있도록 설계 하는 Design for Failure의 개념이 필요  

* 작업 자동화 도구 등을 통해 서버의 기동, 정지, 정기적 백업과 같은 작업을 자동화 시켜 일손을 최대한 줄이는 구조를 만들어내야 함  

* 클라우드는 자유도가 낮지만 시스템 구축의 노하우가 패턴으로써 공유되고 있음  

## 클라우드를 활용한 중소 규모 시스템  

### 개요  

* 가상 서버는 로드 밸런서로 다중화된 구성을 취하고, 전체 데이터를 백업  

* 프로덕션 환경, 테스트 환경, 개발 환경을 3면으로 구성해야 함  

* 네트워크로 인터넷과 VPN 망을 통하는 2개의 루트 확보  

* 프로덕션 데이터 베이스 서버와 백업 서버를 별도로 구성  

## 클라우드를 활요한 빅데이터 분석 사이트  

### 클라우드 도입의 배경  

* 점포 및 EC(전자 상거래) 사이트에서 생성되는 구매 정보와 고객 정보 등의 데이터 분석에 클라우드 활용  

* 생성되는 데이터 양에 따른 시스템 부하에 변동이 있어도 유연하게 대응 가능  

### 시스템 개요  

* 병렬 분산 처리 -> Apache Hadoop  

  * 고속 처리를 위해 물리 서버(베어 메탈)을 이용 -> 밤 동안 일괄적으로 데이터를 분석하고 처리  

  * Map + Reduce : 데이터를 쪼개고, 걸러서 통합한다  

  * HDFS : Hadoop Distributed File System  

* 실시간 분석 -> Apache Spark  

* Business Intelligence(BI) : 기업에서 데이터를 수집, 정리, 분석하고 활용해 효율적 의사결정을 하는 방법  

## 클라우드 관리 플랫폼  

### 구성 관리  

* 클라우드 서비스의 서버와 네트워크 같은 시스템 구성 및 액세스 규칙 등을 관리 포털 화면을 통해 손쉽게 할 수 있는 기능  

### 성능 관리  

* 구축된 클라우드 서비스의 시스템 및 서버 환경의 CPU나 메모리 등의 성능을 모니터링하고 관리하는 기능  

### 운영 관리  

* 서버 및 네트워크 작업의 모니터링, 정기적인 데이터 백업, 리소스 모니터링 및 경고, 정기적인 작업 수행과 같은 서비스를 운영하고 관리하는 기능  

### 멀티 클라우드 관리  

* 클라우드 서비스별로 API와 제공하는 서비스가 다르지만, 이들을 통합적으로 관리하는 기능  

### 사용자 관리  

* 운영 관리 및 비용 관리 시스템 등의 소속 및 직위에 따른 접근 권한을 설정하는 기능  

## 클라우드를 실현하는 기술들  

* 가상화 기술  

  * 하드웨어 리소스(예 : CPU, 메모리, 스토리지)  

* 컨테이너 기술  

  * 애플리케이션을 싱햏하기 위한 영역(사용자 공간)을 여러개로 나누어 사용  

* 분산처리 기술(Apache Hadoop, Spark)  

  * 대량의 데이터를 여러 서버에 분산하여 동시에 병렬로 빠르고 효율적으로 처리하는 기술  

* 데이터 베이스 기술(RDB, NoSQL)  

## 서버 가상화 기술  

### 주된 가상화 기술  

* 서버, 네트워크, 스토리지 가상화가 존재  

### 서버 가상화의 장점  

* 하나의 물리적 서버 리소스에 여러 개의 서버 환경을 할당하고, 각각의 환경에 OS와 애플리케이션을 실행할 수 있게 만들어 줌  

* 물리 서버의 수가 줄어들어 공간 절약과 비용 절감에 도움이 됨  

* 각각의 서버는 독립된 상태로 가상 서버 중 하나가 바이러스와 같은 위협에 노출되더라도 다른 가상 서버에게 영향을 미치지 않음  

### 3종류의 서버 가상화 기술  

* Type 1 - Native / Hypervisor 하이퍼바이저 형 가상화  

  * 하나의 물리 서버 하드웨어 위에 하이퍼바이저라는 가상화 소프트웨어 동작시키고 그 위에 게스트 OS를 가동시킴, 물리 서버보다 처리 능력이 떨어짐  

  * VMware - vsPhere, MS - Hyper-V, Citrix - Xen, Linux - KVM  

* Type 2 - Hosted 호스트 형 가상화  

  * VMware - Vmware Workstation, Oracle - Virtual Box  

* Container 컨테이너 형 가상화  

  * Docker  

## 컨테이너 기술  

### 기술 개요  

* 컨테이너 기술 : 하나의 OS에서 애플리케이션을 실행하기 위한 영역(사용자 공간)을 여러 개로 나누어 사용하는 것  

* 컨테이너는 애플리케이션의 실행 환경을 가상화  

* 가상화 오버헤드가 적기 때문에 빠르게 시작하고 정지할 수 있으며 성능의 저하가 거의 없음  

* 1대의 물리 서버에 매우 많은 수의 컨테이너를 탑재할 수 있음  

### 장점  

* 하나의 호스트 OS에서 여러 OS를 동시에 이용 가능  

* 다른 컨테이너로의 복제성, 이식성이 뛰어남  

* 가상화 환경위에 또 다른 OS를 동작시킬 필요가 없어 개별 컨테이너가 필요로 하는 하드웨어 리소스가 줄어 부팅이 빠름  

### 단점  

* OS의 커널을 공유하기 때문에, OS 별로 이미지는 자신의 OS에서만 실행 가능  

* 한 호스트 OS에서 여러 컨테이너를 동작시키기 때문에, 컨테이너 하나가 공격 받을 경우, 나머지 컨테이너 또한 위험에 노출될 가능성 존재  

## 분산 처리 기술  

* 데이터를 여러 개의 서버에 나누어 병렬로 처리  

* 가격 부담을 줄이면서 대량의 데이터를 고속으로 처리 가능해짐  

### 분산 처리를 구현하는 소프트웨어  

* Apache Hadoop  

  * 1대의 마스터 서버와 여러 대의 슬레이브 서버로 구성  

  * 처리 능력은 슬레이브 서버의 대수에 비례하여 증가  

  * 큰 데이터를 모은 일괄처리에 적합함  

  * HDFS + MapReduce

* Apache Spark  

  * 메모리에서 대량 데이터를 병렬 분산 처리  

  * 메모리 안에서만 읽고 쓰므로 속도가 매우 빨라 실시간 처리에 유리  

## 데이터베이스 기술  

### RDB  

* 여러 개의 데이터를 행과 열이 있는 표 형식으로 표현하여, 복잡한 데이터의 관계를 처리할 수 있도록 만든 데이터 베이스  

### NoSQL  

* Not only SQL, RDB와 같은 관계형 데이터베이스가 아닌 데이터베이스  

* 대량의 데이터를 분산시켜 고속으로 처리하는 분산 데이터 베이스  

* 종류  

  * 키 밸류 형  
  
    * Memcached, Redis, Riak  

  * 컬럼 지향형  

    * Hbase, Cassandra  

  * 문서 지향형  

    * MongoDB, Couchbase  

  * 그래프형  

    * Neo4j

## 스토리지 기술  

### 블록 스토리지  

* 일정 크기의 블록으로 나뉜 스토리지의 논리 볼륨을 블록 단위로 액세스할 수 있는 스토리지  

* 파이버 채널(FC), iSCSI와 같은 전용 프로토콜 사용  

* 서버와 스토리지가 데이터를 교환할 때의 오버헤드가 적어 빠른 데이터 전송이 가능  

### 파일 스토리지  

* 파일 단위로 저장 액세스가 가능하며, 파일 공유 기능을 갖춘 스토리지  

* Windows - SMB(Server Message Block), CIFS(Common Internet File Sysstem), Linux, Unix - NFS(Network File System), NAS(Network Attached Storage)  

### 오브젝트 스토리지  

* 데이터를 객체 단위로 처리  

* HTTP 프로토콜 기반의 REST(Representational State Transfer) 형식의 API를 사용  

* 오브젝트 스토리지는 쉽게 용량을 늘릴 수 있으며 저장할 수 있는 데이터의 크기와 수에 제한이 없음  

* 갱신 빈도가 적은 데이터나 대량의 데이터를 저장하고 장기 보존하는 용도로 이용됨  

## 네트워크 가상화 기술  

### VLAN(Virtual LAN)  

* 하나의 물리적인 네트워크를 여러 개의 논리적인 네트워크로 분할하는 기술  

* 조직 단위로 네트워크를 나누면 조직 안에 한정된 데이터를 전송할 수 있음  

### VPN(Virtual Private Network)  

* 인터넷과 같은 불특정 다수가 사용하는 네트워크에 가상으로 전용선과 같은 사설망을 연결하는 기술  

* 온프레미스 시스템이 인터넷을 통해 VPN에 연결할 때 IPsec라는 프로토콜 사용  

* IPsec를 이용해 통신 시 통신 거점 인증, 통신 데이터의 암호화가 이루어져 거점 간의 안전한 통신이 구현됨  

### NFV(Network Functions Virtualization)  

* 네트워크 기능을 소프트웨어로 구현하여 가상 서버 위에 구축하는 기술  

* 네트워크 장비의 수요 및 구성 변경 등에도 유연하게 대응 가능  

## SDN(Software Defined Networking)  

* 서버 가상화 및 클라우드는 네트워크 트래픽의 급속한 증감 및 경로 변경을 초래하므로, 이에 대응하기 위해 네트워크 증설과 변경, 운영의 자동화가 큰 과제  

### SDN 개요  

* 통신의 전송 기능(데이터 플레인)과 제어 기능(컨트롤 플레인)을 분리하여, 제어 기능을 컨트롤러에 논리적으로 집중시켜서 데이터의 흐름을 소프트웨어로 정의하는 것  

* SDN의 등장 이후로 스위치, 라우터 같은 네트워크 장비는 OS, 미들웨어, 애플리케이션이 통합된 수직 통합형 아키텍처에서 각 기능 레이어를 분리하고, 개방적인 인터페이스로 연결하는 아키텍처로 변화하고 있음  

* 네트워크 기동 상황 및 운영에 맞추어 소프트웨어로 유연하게 데이터 전송 경로를 변경할 수 있게 될 것으로 기대됨  

* 서비스 중지 없는 라이브 마이그레이션 가능  

## 하이퍼 컨버지드 인프라  

### 컨버지드 인프라란  

* 서버, 네트워크, 스토리지, 소프트웨어(하이퍼바이저 및 운영 관리 도구) 등을 하나의 패키지에 통합한 제품을 뜻하며, 수직 통합 시스템이라고 부름  

* 안정적인 가동 가능, 안정화된 구축 방법이 문서화되어 있어 단기간에 도입 가능  

### 하이퍼 컨버지드 인프라란

* 소프트웨어 기반의 서버와 네트워크, 스토리지 등의 구성 요소가 통합된 제품  

* 공유 스토리지 대신, 여러 개의 서버를 통합하여 각 서버에 내장된 스토리지로 가상 공유 스토리지를 구축 가능  

*컨버지드 인프라처럼 하나의 업체 또는 벤더로부터 지원을 받게 되므로 전체를 하나의 시스템으로 관리 가능  

## API(Application Programming Interface)  

* 프로그램이 가진 기능이나 리소스를 외부의 다른 프로그램이 호출하여 이용하기 위한 명령이나 함수, 데이터 형식 등을 정한 규약  

### API를 통해 외부 프로그램으로 클라우드 서비스 조작  

* 가상 서버의 생성과 정지 같은 조직이 사람의 손을 거치지 않고 프로그램으로 직접 제어 가능  

### API를 사용한 서비스 연계, 시스템 구축 및 운영 관리 자동화  

* 서드 파티 프로그램이 보유한 운용 관리 기능 및 보안 기능 등을 API를 통해 연계 가능  

* 클라우드 관리 플랫폼을 이용하여 여러 개의 클라우드 서비스를 API로 중앙에서 관리하는 사례가 늘어남  

## 클라우드 네이티브 아키텍처  

### 마이크로 서비스 아키텍처  

* 하나의 애플리케이션을 작은 서비스의 집합체로 구현하는 방법  

* 각각의 서비스들이 API와 같은 간단한 방법으로 연계하여 동작하는 것을 뜻함  

* 컴포넌트 별로 개발함으로써 서비스 개발의 신속성이 강회됨  

* 필요할 때 필요한 기능을 가진 컴포넌트의 추가와 교체 등이 가능하게 됨  

### Monolythic -> MSA(Polyglot한 방식)  

* Monolythinc : 빌드, 개선을 자주 못함  

* MSA를 이용한 클라우드 네이티브 아키텍처 : 빌드와 개선을 자주 할 수 있음  

* polyglot한 방식 : 중앙 관리가 최소화 되며 각 언어의 장점을 활용하여 서비스를 개발 가능해짐  

### 서버리스 아키텍처  

* Fully Managed(환경 구축과 보안 패치 적용, 백업의 생성 등을 모두 클라우드 사업자가 수행함)한 것이라면, 사용자는 서버의 존재를 전혀 의식하지 않고 응용 프로그램을 실행할 수 있게 되는 아키텍처  

## 개발과 운영의 통합(DevOps)  

### DevOps란  

* Development(개발) 및 Operation(운영)이 함께 협력하여 완성도높은 소프트웨어를 더욱 신속하게 만들어내는 문화를 뜻함  

### DevOps가 주목 받게된 배경  

* 서비스의 전체 그림 안에서 결정된 부분을 먼저 개발해서 배포하는 스피드 중시형  

* 애플리케이션 개발 운영환경을 지원하는 PaaS 서비스가 완비되어 가능해짐  

* 많은 수의 서버를 소규모 인원으로 운용할 수 있게 되어, 운영 부서에서 개발 부서로 리소스가 이전되는 움직임이 두드러지고 있음 -> Two Pizza Team  

---

## Host OS형 가상화 실습  

### VirtualBox  

* H/W 가상화 소프트웨어  

### Vagrant  

* OS에 대한 프로비저닝 도구로, 주로 가상머신을 생성하고 관리할 때 사용  

* 사용자의 요구에 맞게 Hostname, IP, Service Install 등 다양한 환경을 미리 설정하고 사용자가 원할 시 해당 시스템을 즉시 사용할 수 있도록 해주는 Provisioning 도구  

```shell
# 자주 사용하는 vagrant 명령어
## 프로비저닝 스크립트 생성
vagrant init
## vagrantfile 읽어 프로비저닝 진행
vagrant up
## 호스트 종료
vagrant halt
## 호스트 삭제
vagrant destroy
## 호스트 접속
vagrant ssh
## 호스트 설정 변경 적용
vagrant provision
```  

* **vagrant로 새로운 프로비저닝을 수행하려면 기존 vagrant를 halt, destroy하고 새로운 디렉토리에서 vagrant init 해야함**
  
```shell
mkdir vagrant && cd vagrant
# vagrant 초기화 -> Vagrantfile 생성
vagrant init
# 초기화 파일 수정
code Vagrantfile
# vagrant vscode plugin 설치
## Vagrant, Vagrantfile Support
# vagrant cloud에서 설치하고자 하는 os의 Vagrant box 이미지 검색
# Vagrantfile을 바탕으로 프로비저닝 진행
vagrant up
# 상태 확인
vagrant status
# vm 접속
vagrant ssh
# 필요한 패키지 설치
sudo yum install nodejs
# vm 접속 종료
exit
```

---

## 발생한 이슈  

* 문제  
**vagrant up 하여 vm 프로비저닝 하는 과정 중에 네트워크 인터페이스를 고르면 충돌하는 문제가 발생**  
* 해결  
**bcdedit /set ypervisorlaunchtype off, Windows 기능 켜기/끄기의 hyper-v, wsl 기능 끄기**를 통해 해결  

* 원인  

1. virtual box와 hyper-v가 충돌하였기에 발생하는 문제  

2. VirtualBox 및 VMware Workstation 등은 레벨 2 하이퍼바이저  

3. Hyper-V를 활성화하면 Windows 10 호스트가 가상 컴퓨터가 됨  

4. 가상 머신에서 Intel VT-X 명령어에 더 이상 액세스 할 수없고 호스트 만 액세스 할 수 있기 때문  

---

### Nginx on Centos7(Node01 -> jenkins-server)  

* What?  
웹 서버 소프트웨어로 가벼움과 높은 성능을 목표로 함  

클라이언트로부터 요청이 발생했을 때 요청에 맞는 정적파일을 보내주는 웹서버 역할 수행  

일반적 HTTP 웹서버 역할 외에도 proxy, reverse proxy 서버의 역할 또한 가능  

Apache가 다중 처리 모듈(Multi Processing Module)을 사용하는 반면 Nginx는 Event driven 방식으로 구동  

* How?  

```shell
sudo passwd
sudo yum update

# yum에게 nginx정보 알려주기
sudo vi /etc/yum.repos.d/nginx.repo
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1

sudo yum install -y nginx

# 방화벽 on
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports

# 포트 변경
sudo vi /etc/nginx/conf.d/default.conf

systemctl start nginx
systemctl enable nginx
```  

### Nodejs on Centos7(Node02 -> tomcat-server)  

* What?  
확장성 있는 네트워크 애플리케이션 개발에 사용되는 소프트웨어 플랫폼  

자바스크립트에서 외부에서 실행할 수 있는 Runtime 환경을 Chrome V8 엔진을 제공하여 여러 OS에서 실행할 수 있게 실행 환경을 제공하는 것  

* How?  

```shell
sudo passwd
sudo yum update

curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -

sudo yum install -y epel-release
sudo yum install nodejs -y

# 방화벽 on
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports

mkdir work && cd work
npm init
npm install express --save

vi package.json # scripts/"start": "node app.js", 추가
vi app.js #깃헙의 파일 복사
```  

### Apache on Centos7(Node03 -> docker-server)  

* what?  
아파치 소프트웨어 재단에서 관리하는 HTTP 웹 서버이며, tomcat + java가 설치됨  

* How?  

```shell  
yum install -y httpd

# 포트 포워딩
sudo systemctl enable firewalld
sudo systemctl start firewalld
sudo firewall-cmd --permanent --zone=public --add-port=8080/tcp
sudo firewall-cmd --reload
sudo firewall-cmd --list-ports

# 포트 변경
sudo vi /etc/httpd/conf/httpd.conf

systemctl enable httpd
systemctl start httpd
```
