---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "데이터베이스 4일차"
date: 2020-07-27 20:10:11 -0400
background: '/img/posts/02.jpg'
---

## E-R 다이어그램 퀴즈  

```shell
### 푸드코트  

- 우리가족(고객) 각자 음식주문기를 통해 반드시 하나 이상의 음식메뉴를 선택할 수 있다.  
- 우리가족(고객) 각자 선택한 음식주문 메뉴에는 하나 이상의 메뉴를 포함한 주문일 수 있다.
- 각각의 음식메뉴 주문은 반드시 한 명의 우리가족(고객)에 속해 있다.
- 우리가족(고객) 각자는 하나 이상의 음식메뉴를 주문할 수 있다.
- 우리가족(고객) 각자가 주문한 음식메뉴는 반드시 하나의 음식코너에서 조리하여 제공한다.
- 각각의 음식코너는 하나 이상의 음식메뉴를 보유할 수 있다.

### 학사관리
- 학생, 교수, 강좌, 학과, 수강내역에 대한 Entity및 Relationship을 도출
- 각 항목에 들어갈 속성은 임의로 설정 (ex, 학과 (학과번호, 학과명, 전화번호, 위치)
```

## 데이터베이스 복제  

교재 153 pg  

### 레플리케이션  

중규모 이상 시스템에서는 반드시 필요함  

* Master  

  * 실제 데이터에 대한 조작, 정의, 제어를 수행한다  

  * 마스터에 문제가 생기면 슬레이브가 대신 역할을 수행한다  

* Slave  

  * 마스터를 복제하여 같은 상태를 유지하도록 업데이트 한다  

  * 변경사항 또한 반영한다  

  * 조회를 주로 담당하는 Slave를 다중화하여 사용자의 부하를 분산 처리한다  

* 마스터와 슬레이브는 1:1부터 N:M까지 다양함  

### 실습  

기존 스택을 사용하지 않고 리눅스에 바로 mysql 설정 후 복제하기 위해 모든 노드를 비워 둠  

* Master : node1  

* Slave : node2, node3  

* CentOS에 MySQL 설치하기  

```shell
** mysql 설치
1. centos mysql 설치
    $ su -
    $ yum -y install http://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm
    $ yum -y install mysql-community-server
2. mysql 시작
    $ systemctl enable mysqld
    $ systemctl start mysqld
3. 로그인
    $ cat /var/log/mysqld.log 확인 <- 랜덤하게 만들어진 패스워드를 확인
    $ cat /var/log/mysqld.log | grep generated
    $ mysql -u root -p <- 위 패스워드로 로그인
        node1 <- nav>:bzvr1uB
        node2 <- q#Q0:btBHLl=
        node3 <- hKFsMaxC,8e!
4. You must reset your password!
    $ vi /etc/my.cnf
    ## password policy (비밀번호 최소 길이 : 4로 설정)
    validate_password_policy=LOW
    validate_password_length=4
    $ systemctl restart mysqld

    mysql> alter user 'root'@'localhost' identified by 'root';
    mysql> SHOW VARIABLES LIKE 'validate_password%';
```

* 설치한 mysql replicate  

```shell
** Replicate
Master DBMS 역할 :
웹서버로 부터 데이터 등록/수정/삭제 요청시 바이너리로그(Binarylog)를 생성하여 Slave 서버로 전달하게 됩니다
(웹서버로 부터 요청한 데이터 등록/수정/삭제 기능을 하는 DBMS로 많이 사용됩니다)

Slave DBMS 역할 :
Master DBMS로 부터 전달받은 바이너리로그(Binarylog)를 데이터로 반영하게 됩니다
(웹서버로 부터 요청을 통해 데이터를 불러오는 DBMS로 많이 사용됩니다)

[Master 서버 DB, 계정정보]
IP : 192.168.56.11(Master)
DataBases : repl_db
ID : user1
PW : test1234

[Replication 계정 정보] <- Master의 데이터를 가져가기 위한 계정
IP : 192.168.65.11 - (Master)
ID : repl_user
PW : test4567
- Master 서버에 데이터를 Slave 서버로 복제하기 위해선 MySQL 계정이 필요합니다
- MySQL root 계정으로 사용하는것은 보안상 좋지 않기 때문에 복제계정을 생성하는것이 좋습니다

** Master 설정
1. MySQL DB, 계정 생성 및 권한 설정
    mysql> create database repl_db default character set utf8;
    mysql> create user user1@'%' identified by 'test1234';
    mysql> grant all privileges on repl_db.* to user1@'%' identified by 'test1234';
2. Replication 계정 생성
    mysql> grant replication slave on *.* to 'repl_user'@'%' identified by 'test4567';
3. 테스트 테이블 생성, 데이터 추가
    mysql> use repl_db;
    mysql> create table member(id int auto_increment primary key, name varchar(20));
    mysql> insert into member(name) values('AAA');
    mysql> insert into member(name) values('BBB');
    mysql> insert into member(name) values('CCC');
    mysql> select * from member;
4. MySql 설정 - my.cnf
    $ vi /etc/my.cnf (macos vi /usr/local/etc/my.cnf)
        log-bin=mysql-bin
        server-id=1
5. MySQL 재시작
    $ service mysqld restart
6. Master 서버 정보 확인
    mysql> show master status;
    +------------------+----------+--------------+------------------+-------------------+
    | File             | Position | Binlog_Do_DB | Binlog_Ignore_DB | Executed_Gtid_Set |
    +------------------+----------+--------------+------------------+-------------------+
    | mysql-bin.000001 |      154 |              |                  |                   |
    +------------------+----------+--------------+------------------+-------------------+
    <- 해당 파일과 포지션의 뒤부터 Slave에서 복제 진행
7. 위까지 Master에서 진행한 상황을 백업하여 Slave로 수동으로 보내주기 실습(어떻게 돌아가는지 보기 위하여) - mysqldump 사용
    mysql> mysqldump -u root -p repl_db > repl_db_backup.sql
    $ scp repl_db_backup.sql root@192.168.56.12:/home/vagrant # secure copy로 덤프파일 node2에 전송
    # $ scp repl_db_backup.sql root@192.168.56.13:/home/vagrant # secure copy

** Slave 설정
IP : 192.168.56.12(Slave), 192.168.56.13(Slave)
1. MySQL DB, 계정 생성 및 권한 설정
    # 위 덤프파일을 받아 열어보면 디비 생성에 관한 파트는 없으므로 직접 생성할것
    mysql> create database repl_db default character set utf8;
    mysql> create user user1@'%' identified by 'test1234';
    mysql> grant all privileges on repl_db.* to user1@'%' identified by 'test1234';
    mysql> grant all privileges on repl_db.* to user1@'%' with grant option; (8.0버전에서);

2. MySQL 설정 - my.cnf
    $ vi /etc/my.cnf (maxos vi /usr/local/etc/my.cnf)
        [mysqld]
        server-id=2
        replicate-do-db='repl_db'
3. MySQL 복원
    Master DBMS에서 복제할 데이터베이스를 dump하여 복원 (create database repl_db)
    # 덤프파일 임포트하여 repl_db로 넣겠다
    # $ mysqldump -u root -p --databases repl_db repl_db < /home/vagrant/repl_db_backup.sql
    $ mysql -u root -p --force --database repl_db < /home/vagrant/repl_db_backup.sql
4. MySQL 재시작
    $ service mysqld restart (or systemctl restart mysqld)
5. Master 서버로 연결하기 위한 설정
    mysql> change master to
    master_host='192.168.56.11', # MASTER 서버 IP (또는 macos의 IP와 port forwarding 이용)
    master_user='repl_user',
    master_password='test4567',
    master_log_file='mysql-bin.000001',
    master_log_pos=154;  # MASTER STATUS에서 position 값
    (master_port=13306; # 필요시 포트 번호 지정. 예, macos 에서 centos)
6. MySQL 재시작
    $ service mysqld restart (or systemctl restart mysqld)
7. 확인
    mysql> show slave status # Slave_IO_Running 확인
    mysql> show slave status # grep Slave_SQL_Running 확인
```

```shell
** Replicate 상태 확인
1. Master 서버 상태 보기
    mysql> show processlist\G;
2. Slave 서버 상태 보기
    mysql> show processlist\G;
    mysql> show slave status\G;
        Read_Master_Log_Pos:    <- Master의 Log position과 일치 해야 함
        Slave_IO_Running:       <- Yes 이어야 함
        Salve_SQL_Running:      <- Yes 이어야 함
3. Salve_SQL_Running: No 일 경우
    mysql> STOP SLAVE;
    mysql> SET GLOBAL SQL_SLAVE_SKIP_COUNTER = 1;
    mysql> START SLAVE;
    mysql> SHOW SLAVE STATUS \G
    $ grep mysql /var/log/syslog
4. Last_Error: 발생 시
    $ vi /etc/my.cnf
        [mysqld]
        slave-skip-errors=all 
    $ service mysqld restart 
```

## 이슈  

### MySQL에서 E-R 다이어그램 그려줄 때 N:M의 관계는 표시 불가이므로 무조건 테이블로 표현해주기  

### E-R 다이어그램 작성시 1:N 관계는 반드시 테이블로 따로 표시할 필요는 없으나 명시적인 효과를 주기 위해 따로 빼 주는 것도 좋다  

### 모델링은 정답은 없다 -> 부가 정보에 따라서 존재의 필요성을 증명하고 그에 따라 모델링 진행하면 됨 -> NULL값은 배제할 것, 테이블의 업데이트 시기를 고려해 모델링할 것  

### 도커 컴포즈로 생성한 컨테이너끼리 (mysql, python) 접속하려면 pymysql에서 connect()함수의 인자로 호스트 이름을 지정하여야 하며 이때 컨테이너의 이름을 인자로 제공한다  
