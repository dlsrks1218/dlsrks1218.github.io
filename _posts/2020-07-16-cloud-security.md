---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 3일차"
date: 2020-07-16 20:10:11 -0400
background: '/img/posts/06.jpg'
---

## Docker  

### 도커 이미지  

* 하나의 소프트웨어, 혹은 OS에 필요한 파일을 여러 Layer로 쪼개어 가지고 있음  

* 이미지를 실체화하여 인스턴스(컨테이너)로 만들어야 이미지를 실행 및 사용 가능함  

* 이미지가 로컬에 존재하지 않으면 기본으로 지정된 레지스트리인 도커 허브 사이트에서 받아옴(pull)  

* (docker pull) + docker create + docker start = docker run  

* 컨테이너 id, name(이름 지정 가능) -> 32bit의 **유니크**한 이름으로 할당  

* **--rm 옵션을 넣어서 컨테이너를 생성하려고 했으나 문제 발생해 생성되지 않으면 에러를 확인할 수 없기 때문에 주의하고 사용해야함**

### 도커 이미지 생성  

* 도커 이미지는 Base Image를 바탕으로 파일을 추가하여 새로운 이미지를 만들 수 있음  

* 컨테이너의 상태를 그대로 이미지로 저장  

### Dockerfile  

* Domain Specific Language(도메인 정의 언어)  


## 실습  

### -it(interactive, tty) 입력모드를, 터미널에서 여는 옵션, 명령어 수행  

```shell
# 오류 발생 우분투에는 /bin/cal 명령어 없음
docker run -it --name test1 ubuntu:latest /bin/cal
# 오류 확인
docker ps -a
docker logs
# 명령어 전달 결과 출력
docker run -it ubuntu:latest /bin/date
>> Thu Jul 16 01:33:34 UTC 2020
# 사용안하는 모든 리소스 삭제
docker system prune
```  

### --volume=[호스트의 디렉토리]:[컨테이너의 디렉토리], -v : 호스트와 컨테이너의 디렉토리 공유하는 옵션  

```shell
# HostOS - CentOS에서 디렉토리 생성
mkdir html
cd html && vi index.html # 임의의 내용 작성
# 호스트의 디렉토리 내용을 컨테이너의 디렉토리에 마운트
# 컨테이너가 삭제되어도 내용은 호스트에 존재하므로 내용 유지 가능
docker run --rm -d -p 80:80 -v /root/html:/usr/share/nginx/html --name volume_test dlsrks1218/nginx:v1.0
# 컨테이너 상태(사용중인 리소스 등) 확인
docker stats <컨테이너 id or image>
```

### Dockerfile 생성 실습  

* docker build -t [생성할 이미지명:태그명] [Dockerfile의 위치] - Dockerfile을 바탕으로 이미지를 생성  

1.FROM [base 이미지]  

```shell
mkdir work && cd work
# Dockerfile 작성
vi Dockerfile
FROM ubuntu:16.04
# 현재 디렉토리(.)에서 Dockerfile을 사용해 fromtest라는 이름, 1.0이라는 태그의 이미지를 만들것이다
docker build -t fromtest:1.0 .
# -f 로 사용할 파일 형태를 지정해줄수도 있음
docker build -t fromtest:1.0 -f ./Dockerfile
# 컨테이너 생성
docker container create fromtest:1.0
# 생성된 컨테이너 시작
docker start <컨테이너 ID>
# 추가적인 명령어가 없으므로 Exited 상태가 되는것이 맞음
docker container prune
```  

2.RUN [bash shell 명령어 추가]  

```shell
vi Dockerfile
# FROM ubuntu:16.04
RUN mkdir /mydata
RUN echo "Hi, there."
# 빌드
docker build -t runtest:1.0 .
# run하면 명령어를 위 RUN이하 명령어를 수행하지만 다음 실행할 동작이 없어 종료됨
docker run --rm --name runtest runtest:1.0
# 실행할 동작 추가
docker run --rm -it --name runtest runtest:1.0 /bin/bash
```  

3.ADD [추가할 파일] : 호스트의 파일을 컨테이너에 추가하는 명령어  

```shell
# >(리다이렉트) 명령어를 통해 내용 추가
echo 'Hello Docker' > test.txt

vi Dockerfile2
FROM ubuntu:16.04
ADD test.txt

# 새로운 도커파일로 이미지 생성
docker build -t addtest:1.0 -f Dockerfile2 .
# 생성한 이미지로 컨테이너 생성
docker run --rm -it --name addtest addtest:1.0 /bin/bash

```

* 막간 퀴즈  

```shell
vi Dockerfile3
FROM centos:latest
RUN mkdir /mydata
ADD test.txt /mydata

# 새로운 도커파일로 이미지 생성
docker build -t test3:1.0 -f Dockerfile3 .
# 생성한 이미지로 컨테이너 생성
docker run --rm -it --name test3 test3:1.0 /bin/bash
# 생성한 파일 내용 확인
cat /mydata/test.txt
```  

