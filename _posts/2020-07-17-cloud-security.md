---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 4일차"
date: 2020-07-17 20:10:11 -0400
background: '/img/posts/01.jpg'
---

## Docker  

### 실습  

* 교재 pg 159~160 실습, 우분투 이미지에 엔진엑스 설치  

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

curl -X GET http://localhost
```

* **CentOS에 도커 설치 후 레지스트리 이미지 풀**  

```shell
# 매니저 노드에 수행
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

# 레지스트리 이미지 풀
docker pull registry
# 레지스트리 컨테이너 실행
docker run --rm -d -p 5000:5000 --name registry registry
# 생성된 레지스트리 확인
curl -X GET http://localhost:5000/v2/_catalog
```

* 추후 문제 낼 수 있는 것  

  * 우분투 혹은 센트os 이미지에서 nginx 혹은 nodejs를 설치해보세요  

## 이슈  

### ubuntu 이미지 베이스로 nginx 설치시 index.html이 존재할 루트 디렉토리가 /usr/share/nginx/html 이 아니라 /var/www/html 이므로 도커파일 작성시 주의할 것  
