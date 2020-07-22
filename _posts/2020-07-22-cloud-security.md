---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "데이터베이스 1일차"
date: 2020-07-22 20:10:11 -0400
background: '/img/posts/04.jpg'
---

## MySQL 컨테이너 복제(Master 1, Slave 2) 마무리  

```shell
cd /root/work/db
docker build --no-cahce -t tododb .
docker tag tododb manager:5000/example/tododb
docker push manager:5000/example/tododb
curl -X GET http://manager:5000/example/tododb
docker run -d -p 3306:3306 -e MYSQL_ALLOW_EMPTY_PASSWORD=true -e MYSQL_MASTER=true manager:5000/example/tododb
docker logs <에러 발생한 컨테이너 ID>
# server-id가 부여가 되지 않음, mysql password 지정 안됨, master가 아닌 slave로 실행되어 master가 지정안됨
vi ./etc/mysql/mysql.conf.d/mysqld.cnf
server-id=1

docker stack deploy -c todo-mysql.yml todo-mysql
docker services todo-mysql
docker service ps todo-mysql

#master db에 접속하여 init-data.sh를 실행하면 slave db들이 변경사항을 따라함
cd /usr/local/bin
init-data.sh
show databases;
use tododb;
show tables;
```

## 데이터베이스 시스템  

### 트렌드  

기존 : 모든 데이터베이스를 통합 관리  

현재 : 업무 별로, 애플리케이션 별로 데이터베이스를 분리하여 저장, 사용  

### 특징  

* 실시간 접근성  

* 계속적인 변화  

* 동시 공유  

* 내용에 따른 참조 : 데이터는 물리적인 위치가 아닌 데이터 값에 따라 참조됨  

* 데이터 모델 / DBMS(데이터 정의) / 데이터베이스(데이터 저장)  

### 정보 시스템  

* 파일 시스템 : 데이터를 파일 단위로 파일 서버에 저장LAN을 통해 파일 서버에 연결, 동시에 파일을 다루기 때문에 데이터의 일관성이 훼손될 수 있음  

* 데이터베이스 시스템 : DBMS를 도입하여 데이터를 통합 관리하는 시스템, DBMS 서버가 파일을 다루며 데이터의 일관성 유지, 복구, 동시 접근 제어 등의 기능을 수행, **무결성(완전한 수명 주기를 거치며 데이터의 정확성과 일관성을 유지하고 보증하는 것)** 유지  

* 웹 데이터베이스 시스템 : 웹 브라우저를 통해 어디서든 접근 가능하도록 함  

* 분산 데이터베이스 시스템 : 여러 곳에 분산된 DBMS 서버를 연결하여 운영하는 시스템, 대규모 응용 시스템에 사용됨  

### 데이터베이스 언어  

* DDL(definition) : 데이터 정의어  

* DML(manipulation) : 데이터 조작어  

* DCL(control) : 데이터 제어어  

### DBMS 기능  

* 정의(Definition) - 데이터의 구조를 정의하고 데이터 구조에 대한 삭제 및 변경 기능을 수행함  

* 조작(Manipulation) - 데이터를 조작하는 소프트웨어(응용 프로그램)가 요청하는 데이터의 삽입, 수정, 삭제 작업을 지원함  

* 추출(Retrieval) - 사용자가 조회하는 데이터 혹은 응용 프로그램의 데이터를 추출함  

* 제어(Control) - 데이터베이스 사용자를 생성하고 모니터링하며 접근을 제어함. 백업과 회복, 동시성 제어 등의 기능을 지원함  

### DMBS 장점  

중복 제거, 일관성, 독립성, 무결성 유지, 관리 기능, 개발 생산성, 데이터 표준 준수 용이  

### 데이터 모델  

계층 데이터 모델, 네트워크 데이터 모델, 객체 데이터 모델, **관계 데이터 모델**, **객체-관계 데이터 모델** -> ORM을 통해 객체-관계 데이터 모델링 수행  

### 데이터베이스의 개념적 구조(3단계)  

DB -> 내부 스키마(DMBS의 관점) -> 개념 스키마(전체 데이터) -> 내부 스키마(사용자가 보는 데이터)  

* 내부 스키마 : 물리적 저장 장치에 데이터베이스가 실제로 저장되는 방법의 표현, 인덱스, 레코드의 배치 방법, 데이터의 압축 등에 관한 사항이 포함  

* 개념 스키마 : 전체 DB를 정의 통합 조직별 하나만 존재  

* 외부 스키마 : 프로그래머가 접근하는 계층으로 하나의 논리적인 부분을 의미, 여러개 존재 가능, 뷰의 개념  

* **개념적으로 엔티티끼리 N:M 관계를 가질 수 있지만 실제 구축 시 1:N 두개로 쪼개어야 함**  

---

## 관계 데이터 모델  

### 릴레이션  

행과 열로 구성된 테이블  

### 용어  

|릴레이션 용어|같은 의미|파일 시스템 용어|
|------|---|---|
|relation|table|file|
|tuple|row|record|
|attribute|column|field|
|schema|intension|header|
|instance|extesnion|data|

### 릴레이션 특징  

* 릴레이션 내의 중복된 튜플은 허용하지 않는다  

* 속성과 튜플의 순서는 상관없다  

* 속성은 단일 값을 가진다  

* 속성은 서로 다른 이름을 가진다

### 키  

* 슈퍼키 : 튜플을 유일하게 식별할 수 있는 하나의 속성 혹은 속성의 집합  

* 후보키 : 튜플을 유일하게 식별할 수 있는 속성의 최소 집합  

* 기본키(Primary Key) : 여러 후보키 중 하나를 선정하여 대표로 삼는 키  

* 외래키(Foreign Key) : 다른 릴레이션의 기본키를 참조하며 관계 데이터 모델의 특징인 릴레이션을 정의  

  * 자신의 기본키를 참조할 수도 있음  

### 시험내기 딱 좋은 무결성 제약조건  

* **데이터 무결성(Integrity)**  

  * 도메인 무결성 제약조건(값이 가져야 할 범위를 지켜야한다)
  도메인 제약(domain constraint)이라고도 하며, 릴레이션 내의 투플들이 각 속성의 도메인에
지정된 값만을 가져야 한다는 조건. SQL 문에서 데이터 형식(type), 널(null/not null), 기본
값(default), 체크(check) 등을 사용하여 지정할 수 있음  

  * 개체 무결성 제약조건(기본키가 유일해야한다)
  기본키 제약(primary key constraint)이라고도 함. 릴레이션은 기본키를 지정하고 그에 따른
무결성 원칙, 즉 기본키는 NULL 값을 가져서는 안 되며 릴레이션 내에 오직 하나의 값만
존재해야 한다는 조건임  

  * 참조 무결성 제약조건(다른 릴레이션 값을 가져올때는 그 테이블에 존재하는 값만 가져와야한다)
  외래키 제약(foreign key constraint)이라고도 하며, 릴레이션 간의 참조 관계를 선언하는
제약조건임. 자식 릴레이션의 외래키는 부모 릴레이션의 기본키와 도메인이 동일해야 하며,
자식 릴레이션의 값이 변경될 때 부모 릴레이션의 제약을 받는다는 것임(옵션 : RESTRICTED(부모 릴레이션 삭제 시 자식 릴레이션에 영향 X), CASCADE(부모 릴레이션 삭제시 자식 릴레이션도 삭제), DEFAULT, NULL)  

---

## 관계 대수  

릴레이션에서 원하는 결과를 얻기 위해 수학의 대수와 같은 연산을 이용하여 질의하는 방법을 기술한 언어  

### 연산자  

* Selection - 조건에 맞는 튜플을 찾는 연산자  

* Projection - 제시된 속성들을 추출하기 위한 연산자  

* Join - 두 릴레이션의 공통 속성을 기준으로 결합하는 연산자  

  * degree(차수) : 두 릴레이션의 공통 속성의 갯수  

  * cardinality(기수) : 두 릴레이션의 기수의 곱  

* Natural Join - Join 결과에서 중복 속성 제거  

* Outer Join - Natural Join시 Join에 실패한(나오지 않은) 튜플을 모두 보여주되 값이 없으면 NULL 채워 반환  

  * 모든 속성을 보여주는 기준이 되는 릴레이션 위치에 따라 Left, Right, Full

* Cartesian Product - 두 릴레이션을 연결시켜 하나로 합칠 때 사용하는 연산자  

  * degree(차수) : 두 릴레이션의 차수 합  

  * cardinality(기수) : 두 릴레이션의 기수의 곱  

---

## SQL 기초  

데이터베이스에서 데이터를 추출하여 문제를 해결하는 Structured Query Language  

* DDL : CREATE ALTER DROP  

* DML : SELECT INSERT UPDATE DELETE  

  * CRUD : Create, Read, Update, Delete

* DCL : GRANT REVOKE  

### host에서 도커 mysql 접속 실습

```shell
# todo-mysql.yml에 마스터 ports 3306:3306 포워딩
# manager, node 1,2,3에 수행
yum install -y epel-release mysql net-tools
mysql --version
# master가 설치된 노드 ip 주소로 접속
# mysql -h <master IP 주소> -u gihyo -p gihyo
# 루트계정으로 접속해 아래 두 스크립트 실행
mysql -h <master IP 주소> -u root -p gihyo
# <https://github.com/joneconsulting/db/tree/master/script/demo_hr.sql> datagrip에서 수행
# Job_Grades -> job_grades
# <https://github.com/joneconsulting/db/tree/master/script/demo_madang.sql> datagrip에서 수행
# 루트 계정으로 DB 생성 후 각 디비 접속은 각자의 connection을 만들어 접속할 것

# DB HOST : 127.0.0.1
# DB PORT : 13306 or 23306 or 33306
# DB Connection : hr madang <- root
#                 tododb <- gihyo
```

## 이슈  

### ubuntu 버전마다 sh 파일 내에 return 명령어가 안먹어서 exit로 바꾸어야 하는 상황이 있음  

### todo-mysql.yml에서 마스터와 그를 복제한 슬레이브 모두 3306 포트를 열어 포워딩하면 문제가 발생하므로 일단 마스터만 포트 포워딩  
