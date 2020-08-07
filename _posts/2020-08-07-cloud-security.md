---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "클라우드 AWS 4일차"
date: 2020-08-07 20:10:11 -0400
background: '/img/posts/01.jpg'
---

## Elastic Load Balancing  

* 트래픽 분산은 원래 L4 스위치(라우터)와 같은 장비가 수행하는 역할  

* 위 같은 장비를 대신하여 클라이언트의 요청을 로드밸런서가 직접 받고 , 관리하는 서버들에게 요청을 골고루 전달하는 역할을 수행  

* 로드 밸런서도 일종의 서버지만 AWS에서 로드 밸런서 기능을 수행하는 서버를 내부적으로 관리하기에 SSH로 직접 접속은 불가  

* 관리하는 서버 중 정상적으로 동작하고 있는 서버에만 요청을 전달(전달 전에 health check를 수행)  

* 로드 밸런서는 신호(ex - health check)를 주기적으로 보내고 일정 시간 동안의 평균을 구해 서버의 상태를 판단  

* **로드 밸런서는 Target Group에 속한 Auto Scaling Group에 의해 만들어진 인스턴스들에 대해 Round Robin 방식으로 부하를 분산**  

### 대상 그룹(Target Group)  

로드 밸런서가 요청을 전달할 서버들을 묶어둔 개념적인 그룹  

* 로드 밸런서가 요청을 보낼 인스턴스들을 더 쉽게 관리하기 위해 만든 기술  

### 로드 밸런서 생성 실습  

* 로드밸런서 생성 후 인스턴스를 로드밸런서에 바로 Target Group에 포함시킬 수 있지만 Auto Scaling Group을 Target Group에 넣을 것  

  * Auto Scaling Group에서 로드 밸런싱 메뉴 편집에서 로드 밸런싱 대상 그룹  

```txt
구성 > 유형 - Application Load Balancer(HTTP, HTTPS)  
구성 > 포트 - 80->8000
구성 > 가용 영역 a,b,c(2~3개 정도 선택)
  * 인터넷 경계 Load Balancer의 경우 AWS에서 노드의 IPv4 주소를 할당합니다. 내부 Load Balancer의 경우 서브넷 CIDR에서 IPv4 주소가 할당
  * vpc가 기존에 사용하던 ec2가 속한 vpc인지 체크할 것
  * 실무에서는 HTTP대신 HTTPS를 사용해야 함
보안 그룹 구성
라우팅 구성 > 이름, 포트, 상태검사 경로

1. ASG에 인스턴스 띄우고, 노드js App 실행
2. ASG 메뉴 중 로드밸런서에서 대상그룹 추가
3. 로드밸런싱 메뉴에서 타겟 그룹 가서 활동에 헬스체크 확인

로드밸런서에게 요청 보내도 auto scaling 그룹에 의해 생성된 ec2가 응답함 - round robin방식으로 돌아가면서 요청에 응답

서버중 하나가 고장났을 때 부하가 건강한 서버로 가는지 테스트
* target group의 health check > health
* 502 bad gateway를 처음에 보내지만 몇번 요청 반복해 고장남을 확인하면 건강한 서버로만 요청을 보냄
```

* 라운드 로빈 스케줄링(Round Robin Scheduling, RR)은 시분할 시스템을 위해 설계된 선점형 스케줄링의 하나로서, 프로세스들 사이에 우선순위를 두지 않고, 순서대로 시간단위(Time Quantum/Slice)로 CPU를 할당하는 방식의 CPU 스케줄링 알고리즘  

* 삭제 후 재 실습(p73)

```txt
1. auto scaling group 가서 최소용량 0으로 인스턴스 삭제
2. auto scaling group > 타겟 그룹 해제
3. 로드 밸런서 삭제
4. 타겟 그룹 삭제
5. auto scaling group 삭제

0. auto scaling group 생성
1. auto scaling group 가서 다시 인스턴스 on
2. 로드밸런서 생성
3. auto scaling group > 로드밸런서에 타겟그룹 설정
4. 타겟 그룹


환경이 구성된 인스턴스 종료 - 이미지 생성 - AMI 생성 - 시작 템플릿 생성 - AutoScalingGroup 생성 및 인스턴스 시작- 로드밸런서 생성 - AutoScalingGroup에서 타겟 그룹 지정 - 헬스 체크
```

### HTTP 프로토콜  

모든 HTTP 응답 코드는 5개의 클래스(분류)로 구분된다. 상태 코드의 첫 번째 숫자는 응답의 클래스를 정의한다. 마지막 두 자리는 클래스나 분류 역할을 하지 않는다  

* **Request(요청)**  

* 요청 방식 : OPTIONS, GET, HEAD, POST, PUT, DELETE, TRACE, CONNECT, PATCH  

* 일반적인 브라우저는 GET, POST 요청만 가능  

  * **GET** : 페이지, 리소스 정보 요청  

  * **POST** : 클라이언트의 요청을 서버로 보낼 때 사용  

* 나머지 메소드들은 프로그래밍으로 처리해야 함  

  * PUT(서버의 리소스 정보 변경)  

  * DELETE(서버의 리소스 값을 삭제)  

* **Response(응답)**  

  * Status Code  

    * 상태 코드마다 호출해야하는 양식이 정해져 있음  

    * 100번대(정보) : 요청을 받았으며 프로세스를 계속한다

    * 200번대(성공) : 성공적으로 요청을 처리했다  

    * 300번대(리다이렉션 완료) : 내부적으로 처리되는 코드, 요청 완료를 위해 추가 작업 조치가 필요  

    * 400번대(클라이언트 오류) : 클라이언트로부터 오류가 발생(ex - 잘못된 요청 방식 등)  

    * 500번대(서버 오류) : 서버가 명백히 유효한 요청에 대해 충족을 실패  

## 배포 과정  

### 무중단 배포  

* **현재 위치 배포 추후 추가**  

* 블루 그린 배포  

  무중단 배포 기법의 하나로 동일한 사양이지만 다른 버전, 즉 blue, green(구 혹은 신) 버전의 그룹을 준비하여 서비스하던 블루 그룹을 종료하기 전 로드 밸런서가 그린 그룹에 요청을 보내도록 한다.

  * 장점  
  
    * 구/신 버전이 동시에 떠 있는 시간을 매우 짧게 처리 가능  

    * 이전 버전으로 돌아가기가(롤백) 수월  

    * 배포 과정에서 서비스되는 인스턴스의 수가 줄지 않아 요청량을 처리하는 데서 오는 장애의 부담이 없다  

  * 블루에 해당하는 이미지 생성 -> 스케일링 작업(ASG blue) -> 로드 밸런서(Target Group)에 추가  

  * 그린에 해당하는 이미지 생성 -> 스케일링 작업(ASG green) -> 로드 밸런서(Target Group)에 추가  

* **카나리 배포 추후 조사**  

```node
# node에서 쿼리문은 한줄로 쓸것
# 잘 되면 종료 -> 이미지 생성 -> 템플릿 생성 -> jonghyun-demo-beta(이름, 설명)

const express    = require('express');
const mysql      = require('mysql');
const dbconfig   = require('./config/database.js');
const connection = mysql.createConnection(dbconfig);

const app = express();

// configuration =========================
app.set('port', process.env.PORT || 8000);

app.get('/', (req, res) => {
    var ip = req.headers['x-forwarded-for'] ||
                req.connection.remoteAddress ||
                req.socket.remoteAddress ||
            (req.connection.socket ? req.connection.socket.remoteAddress : null);
    console.log(ip);
    res.send('Hello, New World! : ' + ip + "," + new Date());
});

// API db query1 =========================
app.get('/users', (req, res) => {
  connection.query('SELECT * from Users', (error, rows) => {
    if (error) throw error;
    console.log('User info is: ', rows);
    res.send(rows);
  });
});

// API db query2 =========================
app.get('/users/:id', (req, res) => {
  connection.query('select * from Users where id=\'' + req.params.id + '\'', (error, rows) =>{
    if(error) throw error;
    console.log('user detail info is : ', rows);
    res.send(rows);
  });
});

// API healthcheck =======================
app.get('/health', (req, res) => {
  res.status(200).send();
});

// Console Log ===========================
app.listen(app.get('port'), () => {
  console.log('Express server listening on port ' + app.get('port'));
});
```

### 퀴즈  

```txt
https://github.com/joneconsulting/simple-nodejs.git에서 본인 github 계정으로 코드 복사
AWS RDS 서비스 생성
simple-nodejs/db/createTable.txt 참조
AWS EC2 인스턴스 생성
simple-nodejs/ver1.0 프로젝트를 포함하는 EC2 인스턴스 생성 (version 1.0)
Port 8000으로 실행
RDS와 연동
AWS Auto Scaling 생성 (BLUE)
최소 1개, 최대 2개의 인스턴스가 유지될 수 있도록 Auto Scaling 설정
AWS Elastic Load Balance 생성
S3에 index.html 페이지를 생성
simple-nodejs/static/index.html 참조
Load Balancer로 이동할 수 있는 링크 수정
AWS EC2 인스턴스 생성
simple-nodejs/ver2.0 프로젝트를 포함하는 EC2 인스턴스 생성 (version 2.0)
Port 9000으로 실행
RDS와 연동
AWS Auto Scaling 생성 (GREEN)
최소 1개, 최대 2개의 인스턴스가 유지될 수 있도록 Auto Scaling 설정
AWS Elastic Load Balance에서 버전 업그레이드
Verion 1.0에서 Verion 2.0으로 업그레이드 배포
```
