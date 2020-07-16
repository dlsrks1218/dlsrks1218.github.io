---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 3일차"
date: 2020-07-16 20:10:11 -0400
background: '/img/posts/06.jpg'
---

## 리눅스  

### 리눅스 프로세스 확인 명령어  

```shell
# PID 확인
netstat -ntlp
# process kill
kill -9 <PID>
```  

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

* 도커에서 사용하는 Domain Specific Language(도메인 정의 언어)  

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
# 컨테이너 혹은 이미지에 대한 정보 확인
docker inspect <컨테이너 or 이미지 ID>
```

### Dockerfile 생성 실습  

* docker build -t [생성할 이미지명:태그명] [Dockerfile의 위치(.)] - Dockerfile을 바탕으로 이미지를 생성  

* docker build -t [생성할 이미지명:태그명] -f [Dockerfile 이름] [Dockerfile의 위치(.)] - 현재 디렉토리(.)에서 Dockerfile 이름이 아닌 별도의 도커파일로 빌드하고자 할때 **-f**옵션 사용  

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

* 이미지 생성 과정에서 수행됨  

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

3.ADD [추가할 파일] [파일이 추가될 위치] : 호스트의 파일을 컨테이너에 추가하는 명령어  

* ADD명령어 내부적으로 버그가 존재해 복사가 필요할 때는 COPY를 사용 권장  

```shell
# >(리다이렉트) 명령어를 통해 내용 추가
echo 'Hello Docker' > test.txt

vi Dockerfile2
FROM ubuntu:16.04
ADD test.txt ~/

# 새로운 도커파일로 이미지 생성
docker build -t addtest:1.0 -f Dockerfile2 .
# 생성한 이미지로 컨테이너 생성
docker run --rm -it --name addtest addtest:1.0 /bin/bash
```

4.ENTRYPOINT [컨테이너 실행 후 처음 실행될 Shell Script]  

```shell
vi run.sh
echo 'Hello, Docker'

vi Dockerfile5
FROM centos:latest
COPY ./run.sh /
# root 사용자에게는 실행권한, 그룹 사용자와 다른 사용자는 읽기 권한만 부여
# RUN chmod +744 run.sh
# 모든 사용자에게 실행권한을 부여
RUN chmod +x /run.sh
# 해당 컨테이너에서 CMD 이하의 명령을 계속 수행함 -> 죽지 않는다
CMD ['ping', '127.0.0.1']
# 도커파일 빌드 -> 이미지 생성
docker build -t test5:1.0 -f Dockerfile5 .
# ENTRYPOINT에 명시된 sh파일을 실행하고 컨테이너 종료
docker run --rm -d --name test5 test5:1.0
```

5.CMD ['명령어', '인자들']  

* Dockerfile의 가장 마지막에 와야함  

* 컨테이너가 생성된 후 수행 될 명령어  

6.WORKDIR [디렉토리] - 작업 영역 변경  

* 루트 디렉토리는 보안 문제가 발생할 수 있으므로 다른 디렉토리로 변경  

### Nodejs 실행 및 도커파일 생성  

```shell
# nodejs 리포지토리 추가를 위한 설치
yum install epel-release
# nodejs 패키지 설치
yum install nodejs
npm init
# start와 dependencies 추가
vi package.json
{
  "name": "nodejs",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node index.js"
  },
  "dependencies": {
    "express": "*"
  },
  "author": "",
  "license": "ISC"
}

vi index.js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('How are you doing?');
});

app.listen(8080, () => {
  console.log('Listening on port 8080')
});

# 필요한 패키지(dependencies) 설치
npm install
# node index.js
npm start

# 용량이 매우 작은 alpine 리눅스에 nodejs가 설치된 이미지를 베이스로 함
vi Dockerfile
FROM node:alpine
WORKDIR /home/node
COPY ./package.json ./package.json
COPY ./index.js ./index.js
RUN npm install
CMD ["npm", "start"]

docker build -t myweb:1.0 .
docker run --rm --name myweb myweb:1.0

# 도커 허브에 올리기 위해 내 아이디 태그 추가
docker tag myweb:1.0 dlsrks1218/myweb:1.0
```

### 공유 폴더(volume)  

* 컨테이너 업데이트 시 데이터를 유지하기 위해 필요함  

* 생성한 이미지를 다른 서버에 배포해 사용하고자 할때 수정한 코드를 허브를 통해 푸시하고 풀 받으면 비효율적임  

* 따라서 Volume Mount(Shared Drive)를 만들어 코드 수정된 결과를 재배포 없이 다른 서버에서 사용 가능  

* 지속적으로 유지해야하는 데이터베이스 파일과 같은 경우(MySQL 컨테이너), 볼륨 마운트 기능으로 데이터를 보존해야함  

```shell
mkdir datadir && cd datadir
# 볼륨 마운트 없이 컨테이너 실행 -> 생성한 데이터가 사라짐
docker run --rm -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql mysql:5.7
# 볼륨 마운트 하면 아래 생성한 데이터가 유지됨
docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true --name mysql -v /my/datadir:/var/lib/mysql mysql:5.7
# mysql 컨테이너 접속 및 mysql 실행
docker exec -it mysql mysql -u root -p
# 아래 mysql 스크립트 실행
create database mydb;
use mydb;
create table member(id int, name varchar(10));
insert into member(id, name) values(1, 'admin');
select * from member;
mysql> exit
# 내가 만든 mydb가 존재하는 디렉토리
cd /var/lib/mysql
```

### 퀴즈  

* 퀴즈 1  

```shell
vi Dockerfile3
FROM centos:latest
RUN mkdir /mydata
COPY ./test.txt /mydata
# ADD test.txt /mydata

# 새로운 도커파일로 이미지 생성
docker build -t test3:1.0 -f Dockerfile3 .
# 생성한 이미지로 컨테이너 생성
docker run --rm -it --name test3 test3:1.0 /bin/bash
# 생성한 파일 내용 확인
cat /mydata/test.txt
```  

* 퀴즈 2  

```shell
vi index.html
<h1>
hello docker
</h2>

vi Dockerfile4
FROM nginx:latest
COPY /root/work/test/index.html /usr/share/nginx/html
RUN echo "nginx on"

docker build -t mywebserver:1.0 -f Dockerfile4 .
docker images

docker run --rm -d -p 80:80 --name mywebserver mywebserver:1.0

curl -X GET http://localhost
```  

* 퀴즈 3  

```shell
vi Dockerfile6
FROM nginx:latest
RUN echo "nginx on"

docker build -t lab4:1.0 -f Dockerfile6 .

docker run --rm -d -p 80:80 -v /root/work/html:/usr/share/nginx/html --name lab4 lab4:1.0
```

## 이슈  

### nginx 포트는 80이다, 8080하지마라 멍충아  
