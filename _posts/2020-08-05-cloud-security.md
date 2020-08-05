---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "클라우드 AWS 2일차"
date: 2020-08-05 20:10:11 -0400
background: '/img/posts/04.jpg'
---

## AWS  

### IAM(Identify and Access Management)  

* 추후 프로젝트 수행 시 팀별로 루트 계정을 부여 받고 팀원들에게 IAM 계정을 만들고 권한을 부여하여 배분  

### EC2(Elastic Compute Cloud)  

* vpc(virtual private cloud) : AWS에서 인스턴스의 가상 네트워크  

* 스토리지 추가에서 볼륨 유형 중 프로비저닝된 IOPS SSD 선택지는 범용보다 10배 이상 비싸므로 사용 지양할 것(퍼포먼스가 높게 요구될때만 사용)  

### S3(Simple Storage Service)  

* 정적인 파일들을 보관하는 곳, 파일 서버와 같음  

* 서비스 종류 : S3 Standard, S3 Standard-IA, S3 Glacier(저장 기간이나 용량 등과 같은 옵션이 다름)  

* AZ(Avaiability Zone, 가용 영역)  

  * 리전 내에 격리된 위치, 개별 데이터 센터  

  * 물리적으로는 떨어져 있지만, 논리적으로는 연결 되어 있음  

  * 1개의 리전당 2개 이상의 AZ가 존재  

* 실습  

```txt
버킷 만들기
1. 이름 및 지역 - 버킷 이름 설정, 자신의 리전에 맞는 곳에 버킷 생성
2. 옵션 구성 - pass
3. 권한 설정 - 기본으로 모든 액세스가 차단 되어있음, 일단 두고 넘어감
4. 검토

폴더 업로드
1. 파일 선택 drag & drop
- REST API - PUT(업로드, 무료), GET(다운로드, 유료)
<스토리지 클래스>
  스탠다드  : 자주 사용하는 데이터
  스탠다드-IA	: 자주 사용하지 않는 데이터(30에 한 번 정도 액세스)
  Glacier : (90일에 한 번 정도 액세스), 추후 다시 액세스 할때 오래 걸리고 비쌈
2. 권한 설정 pass
3. 검토
```

* s3에 html파일 올려두고 서비스하거나, ec2를 통해 웹서버 띄우고 s3의 html파일을 연결해서 서비스도 가능  

  * s3앞에 캐시 서버를 두고 기존에 요청된 건에 한해 클라이언트의 요청을 빠르게 응답, 하지만 원본 데이터의 변화에 대해 캐시 서버는 모르기 때문에 변화가 없는 경우에 사용하는게 좋음  

  * 빈번하게 변화가 있는 경우 ec2 + 웹서버 등과 같은 구축을 통해 서비스 하는 것이 좋음  

* 버킷을 지우려면 내부에 오브젝트를 삭제, 비우기한 다음 지워야함  

* 버킷 내의 오브젝트를 퍼블릭 액세스 가능케하려면 버킷 부터 퍼블릭 액세스 허가 시켜준 후 오브젝트의 권한을 줘야함  

* 버킷에서 속성 / 정적 웹 사이트 호스팅 사용하면 버킷으로 웹사이트 호스팅 가능  

* 팁  

  1. A로 시작하는 파일, B로 시작하는 파일이 존재할 때, 가능한 한 폴더를 나누어서 사용할 것  

  2. 자주 사용하는 파일은 같은 폴더명으로 정리해두기  

### CloudFront  

클라우드 서비스 앞에 존재하여 정적인 컨텐츠와 같이 미리 다운된 컨텐츠에 대한 요청을 컨텐츠에 직접 액세스 하지않고 캐시 서버의 데이터로 응답 해주는 기능 제공  

* 단독으로 작동하지 않고 위 같은 서비스들 앞단에 존재함  

* Cache - 24시간 캐시 서버에 유지됨, 변경을 반영하려면 Invalidations를 활용해 캐시 삭제하면 됨  

* CDN(Contents Delivery Network) - 클라이언트가 위치한 인근 서버에서 캐시 기능을 반환  

* CloudFront Distributions  

  * Original Domain Name에 내 EC2 퍼블릭 DNS 주소 넣어주기, 열어놓은 포트가 8000이라 HTTP Port를 8000으로 바꿔주기  

  * 생성 후 ID를 클릭하고 General 탭의 Domain Name이라는 곳의 값(접속할 CloudFront 주소)으로 접속
  
  * <dotcom-tools.com> : 웹페이지 속도 테스트

  * 접속하여 속도 테스트 시 훨씬 빠름, 하지만 오리지널 데이터는 변하지 않고 캐시 서버에 있는 데이터로 응답  

  * Invalidations 탭 : 기존의 캐시를 삭제 -> 실제 데이터 소스에 액세스하여 변경사항을 반영함  

  * Edge Locations : CDN 서비스를 지역적으로 모아둔 것  

    * General 탭에서 edit > Edge Locations를 설정 가능  

```shell
# ec2 접속 & 간단한 nodejs 앱(3초 딜레이) 띄우기
$ vi app.js
const express = require('express');
const app = express();
const port = process.env.PORT || 8000;

app.get('/', (req, res) => {
  // 3초의 딜레이를 주어 결과 확인
  setTimeout(function() {
    res.send('Hello, world!' + new Date());
    return console.log(new Date());
  }, 3000);
  // res.send('Hello, World!' + new Date());
});
app.listen(port, () => console.log(`Example app listening on port ${port}!`));

$ npm init
$ vi package.json -> scripts에 "start": "node app.js" 추가  
$ npm install express
$ cat package.json
$ npm start

# 3초 지나기 전에 CloudFront가 먼저 응답하기 실습
```

---

## 용어  

### 웹 서버  

클라이언트에서 HTTP 프로토콜로 요청을 받고 정적인 파일들을 응답으로 전달  

* nginx, Apache, IIS 등의 제품이 존재  

### 웹 애플리케이션 서버(WAS)  

클라이언트의 요청에 대해 코드 실행을 통해 동적인 응답을 만들어주는 역할  

* 배포한 코드를 프로세스로 실행시키고, 해당 프로세스에 클라이언트의 요청을 넘겨줌  

* 서버 자원을 최적으로 사용하기 위해 프로세스의 수나 프로세스의 메모리를 조절  

* Phusion Passenger, Apache Tomcat, JBoss 등의 제품이 존재  

### ACL(Access Control List)  

오브젝트에 대한 액세스 권한이 부여 된 사용자 또는 시스템 프로세스와 지정된 오브젝트에 허용되는 조작을 지정  
