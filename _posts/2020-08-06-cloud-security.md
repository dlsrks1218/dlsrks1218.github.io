---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "클라우드 AWS 3일차"
date: 2020-08-06 20:10:11 -0400
background: '/img/posts/05.jpg'
---

## RDS(Relational Database Service)  

* Public(보안그룹(Security Group)에 3306 추가)  

* Private 접속 가능(Only 같은 VPC의 EC2를 통해서만 접속)  

  * 보안그룹에 EC2의 보안그룹 등록해야 함

* 데이터베이스 생성, 모니터링, 백업과 복원, 가격  

* EC2에 DB를 설치하는 방식보다 가격이 비싸지만 데이터의 안전 확보, 백업, 모니터링 같은 편리한 기능을 사용할 수 있어 RDS 사용이 좋다  

* IAM 사용자에게 AmazonRDSFullAccess 권한 주기  

* S3에 저장된 스냅샷(백업본)에서 복원도 가능  

### 데이터베이스 생성  

* Amazon Aurora - 빠르고 안전  

* Microsoft SQL Server - Windows Base에서 좋음, 꼭 사용할 필요는 없음  

* PostgreSQL - 기업에서 많이 사용하지만 인스턴스나 계정 관리가 까다로움  

* MariaDB - MySQL과 99.9% 유사하나 완전 오픈소스  

* DB 버전을 선택할 시 마이그레이션(이관) 문제도 고려해야 함  

* root계정은 막아두고 유사한 권한의 계정을 사용하는게 일반적  

* 퍼블릭 액세스 가능 - 예  

  DB 인스턴스를 호스팅하는 VPC 외부의 EC2 인스턴스 및 디바이스가 DB 인스턴스에 연결됩니다. DB 인스턴스에 연결할 수 있는 EC2 인스턴스와 디바이스를 지정하는 하나 이상의 VPC 보안 그룹을 선택해야 합니다.  

* 인바운드 규칙의 소스에 사용 중인 보안그룹 이름을 등록할 것(private ip를 통해 액세스)  

* AWS에서는 DB의 리소스 이름만 사용하고 애플리케이션을 통해서 접근하도록 함(직접 DB 접속 X, 보안성 확보)  

  * API 서버의 함수를 호출하여 리소스 사용 - Micro Service Architecture  

### RDS 생성 및 ec2에서 접근 실습  

```shell
Virtual Private Cloud(VPC) - (vpc-b22bddcf)
Security Group (jonghyun-sg)
admin/dlsrks123
# 기본 생성 시 퍼블릭 액세스 가능여부를 지정할 수 있음
# 퍼블릭 액세스를 허용해주지 않으면 프라이빗 액세스 해야함
# 프라이빗 액세스를 하기위해서 같은 VPC내에서 EC2->RDS 접속을 수행
# EC2에 mysql-client 설치
1. 같은 VPC에 속할 것
2. Security Group에 EC2의 SG을 등록
3. EC2 -> MySQL client 설치
4. mysql -h <rds endpoint> -u admin -p
```

### EC2에서 nodejs + mysql  

```shell
npm install express
npm install mysql
# 1차 테스트
vi index.js
console.log('Hello world')
# 2차 테스트
vi index.js
const mysql      = require('mysql');
const connection = mysql.createConnection({
  host     : 'localhost',
  user     : 'root',
  password : 'test1357',
  database : 'my_db'
});
connection.connect();
connection.query('SELECT * from Users', (error, rows, fields) => {
  if (error) throw error;
  console.log('User info is: ', rows);
});
connection.end();

# 3차 테스트 > db연결 및 api 서버 역할 수행
vi index.js
const express    = require('express');
const mysql      = require('mysql');
const dbconfig   = require('./config/database.js');
const connection = mysql.createConnection(dbconfig);

const app = express();

// configuration =========================
app.set('port', process.env.PORT || 8000);

// API
app.get('/', (req, res) => {
    var ip = req.headers['x-forwarded-for'] || 
                req.connection.remoteAddress || 
                req.socket.remoteAddress ||
            (req.connection.socket ? req.connection.socket.remoteAddress : null);
    console.log(ip);
    res.send('Hello, World! : ' + ip + "," + new Date());
});

// API
app.get('/users', (req, res) => {
  connection.query('SELECT * from Users', (error, rows) => {
    if (error) throw error;
    console.log('User info is: ', rows);
    res.send(rows);
  });
});

app.get('/health', (req, res) => {
  res.status(200).send();
});

app.listen(app.get('port'), () => {
  console.log('Express server listening on port ' + app.get('port'));
});

# 서버 시작시 nodejs 띄우려면 /etc/init.d에 추가하면 됨
```

## 다중 서버 환경 구성  

트래픽에 대응하여 서버에 장애가 생겨도 서비스가 안전하게 돌아갈 수 있게 하기 위해 다중 서버 환경을 구성한다  

### Auto Scaling 그룹  

* 같은 서버, 코드, 사양이어야 함

* 실시간 트래픽 등의 변수를 반영해서 인스턴스의 수를 조정하기 때문에 훨씬 안정적으로 서비스를 운영할 수 있으며 비용도 크게 절감할 수 있음  

* 자동 조정 기준 : 자원 사용량, 시간 등  

* Auto Scaling 그룹을 구성하기 위해선 AMI(Amazon Machine Image)를 이용해 어떤 인스턴스를 띄울지 미리 정의해두는 시작 템플릿을 만들어야함  

* **Auto Scaling 그룹을 지정해둔 이상 ec2 서버를 직접 지우는것은 의미 없고 Auto Scaling 그룹에서 수를 조정해서 삭제해야함**  

* tps(transaction per second)를 판단 기준으로 stress test 수행 - jmeter, coderunner  

```txt
# 현재 인스턴스 종료 -> 스냅샷 -> AMI 생성 -> 시작 템플릿 -> AutoScaling 그룹
1. 인스턴스 종료
2. 이미지 > 이미지 생성 > AMI ID : ami-0e0ee0214f5f85891
3. 시작 템플릿 > 생성 > AMI, 인스턴스 유형, 키 페어, 보안그룹 설정
4. AutoScaling 그룹
  생성 > 시작 템플릿 > 서브넷을 내 ec2가 속한 az중 2~3개 선택 > 로드밸런싱(추후에 추가) > 그룹 크기(스케일링될 서버 갯수 - 유지, 최대, 최소) 및 조정 정책 구성 > 대상 추적 조정 정책(50%)
5. stress 준 뒤 Auto Scaling 그룹 > 활동 > 작업 기록에서 스케일링 작업 생기는지 확인(WaitingForInstanceWarmup라는 작업 생성) -> 인스턴스에서 private ip로 추가된 인스턴스 확인 -> 테스트 마쳤으면 Auto Scaling 그룹 > 세부정보 > 그룹 세부 정보에서 원하는 용량 수정
```

* 지우고 다시 생성 실습  

```shell
1. EC2 삭제(auto scaling group)
2. auto scaling group 삭제
3. 인스턴스 > 시작 템플릿(Launch Templates) > 삭제
4. 이미지 > AMI > 삭제

# 기존 종료한 인스턴스를 기반으로 실습 시작
1. 이미지 생성
2. 
```

```shell
# 실시간 사용량
top
# 메모리 사용량
free -h
# disk 사용량
df -h
# 스트레스 테스트
sudo yum install -y stress
# 10분간 사용중인 cpu에 100%의 스트레스를 주겠다
# AutoScaling 그룹에서 5분 간격으로 판단하기 때문에 10분정도 스트레스 줌
stress --cpu 1 --timeout 600
```
