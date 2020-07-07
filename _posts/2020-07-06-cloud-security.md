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

### 시스템 구성이 클라우드 서비스에 의존한다.  

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

## 3장마저하기!

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
