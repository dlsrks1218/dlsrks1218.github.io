---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "그림으로 배우는 클라우드"
date: 2020-07-06 20:10:11 -0400
background: '/img/posts/02.jpg'
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
