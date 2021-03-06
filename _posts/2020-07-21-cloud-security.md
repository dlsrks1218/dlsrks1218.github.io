---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "1차 특강"
date: 2020-07-21 20:10:11 -0400
background: '/img/posts/03.jpg'
---

## 클라우드 보안  

아주대학교 사이버보안학과 박춘식 교수(blog.naver.com/cspark14, csp@ajou.ac.kr)  

* 시큐리티는 나부터 지킬 것, 불가능은 없고 어려울 뿐  

* 기술은 이론이 아무리 좋아도 실용성이 없으면 사장됨  

* 키워드 : **블록체인**, **ZKIP**<http://wiki.hash.kr/index.php/영지식_상호_증명>, 엣지 컴퓨팅, IoT, 6G  

### 목차  

1. 정보보호 : 암호  

2. 클라우드 보안  

3. 클라우드 보안 기술 : CASB  

4. 클라우드 보안 인증 제도  

### 정보보호 : 암호  

인터넷 시대의 도래와 함께 정보보호의 필요성이 대두됨, 계획 수립, 보호, 사고 대응(PDCA)  

* 암호화 종류  

대칭키 암호 - AES, SEED - 양쪽이 다 품(PC, Server 등 암호화에 사용)
공개키 암호 - RSA(짧은 메세지) - 나밖에 못 품  
하이브리드 암호 - HTTPS, TLS, SSL  
단방향 해시 함수 - 압축(리버스가 힘듦, 무결성, 변경되지 않음을 입증하기 위함)  
디지털 서명  
메시지 인증 코드  

* 암호 프로토콜 배경  

공개키 암호의 등장(1976), 디지털 서명 -> 단순한 암호 프로토콜 -> 영지식 프로토콜(ZKIP) -> asdf  

포청천(3자)을 이용하는 방안 - 상대방보다 리소스가 많다는 사실만을 입증하고 동의, 3자에 대한 신뢰 문제가 발생  

통신 프로토콜을 이용하는 방안 - 비교 단위를 제한, 서로 진실만을 얘기하는 등 제약을 둔다  

* 전자 현금  

  요구조건 : 안전성, 프라이버시, 재사용 금지, 양도성, off-line, 분할성  

  이중 사용(double spending)을 방지하고자 중앙 집중된 은행의 역할을 네트워크가 대신하고자 하는 블록체인 기술이 각광받게 됨  

* 전자 투표  

* ZKIP(영 지식 증명) : 정보를 제공하지 않고 그 정보를 가지고 있다는 것을 증명, n번 질의 하면 1/2^n의 확률로 맞춤(n이 커질 수록 확률이 0에 수렴한다) - 이론적으로 n회 수행은 좋으나 실용성이 떨어짐  

* NZK(None interactive Z KIP)  

* 블록체인과 암호 (해시 함수, 공개키 암호, 랜덤 수 , 키관리, 암호 프로토콜(비잔틴, 멀티 파티 프로토콜), 영 지식 증명)  

* Quantum Security(양자 암호 - 양자 키 분배, Quantum Key Distribution)  

양자 이론을 이용한 키 분배 시스템, 기존에는 공개 키 형태로 분배해왔음, 대칭 키의 길이를 늘림으로써 약간 해결 가능함, 양자 컴퓨팅이 실용화되면 공개키 암호를 풀 수 있게 됨

* 키 해독은 수 천 개의 키를 동시에 빠르게 시도  

* 양자 암호가 실용화되고 양자 컴퓨터 능력이 증대되면 공개키 암호는 해독됨, 비밀키 암호는 안전하나 키 길이 2배 이상 필요함  

### 클라우드 컴퓨팅  

클라우드 서비스 제공자, **클라우드 서비스 브로커**가 필요, 클라우드 서비스 유저

* 클라우드 컴퓨팅의 제공 형태 및 책임  

On-premise, Infrastructure(IaaS), Platform(PaaS), Softrware(SaaS)  
<- 사용자의 책임이 큼    -   -   -    서비스 제공자의 책임이 큼->  

* 보안 컨설팅을 위해서는 책임 범위와 Compliance(법규준수/ 준법감시/ 내부통제)를 알아야 함  

* 클라우드 컴퓨팅 보안 이슈  

관리자, 서버 위치, 데이터 보관 위치, 접근권, 백업 담당자, 확장성, 보안성, 정보보호팀 관여 유무  

* 클라우드 컴퓨팅 보안 위협과 원칙  

보안 위협 - 서비스 중단, 법 / 규제 준수, 정보 유출, 원치 않은 접근, 서비스에 대한 가시성 부족  
보안 원칙 - 분리와 격리, Defence in Depth  

* 클라우드 이용 사이버 공격  

클라우드 서비스를 공격에 활용하는 사이버 공격(Crimeware as a Service, CaaS - 비용 부담이 적은 클라우드 서비스를 범죄자들이 활용하여 사이버 범죄 가담), 클라우드 서비스를 "해커 종적 은닉 공간" 활용 사이버 공격  

* 사이버 공격의 새로운 표적 '클라우드' - Cryptojacking  

* Meltdown Spectre(실패한 분기 예측으로 인해 메모리 데이터가 관찰 가능한 주변 효과(Side effect)로 의도치 않게 노출되는 취약점)  

* GDPR(유럽연합 일반 데이터 보호 규칙)  

* 클라우드 서비스 주요 이슈  

서비스 안정화, 클라우드 센터 위치, 시큐리티, 개인정보보호, Inter-Cloud 상호운용성(서비스 공급자 락인, 사업자 종속), 거버넌스 결여, Compliance, 소프트웨어 라이선스 문제, 지적재산권/저작권 문제  

* 클라우드 컴퓨팅 보안 환경  

온프레미스 기존 보안 + 클라우드 자체 보안, 소유 -> 이용 개념으로 IT 자원 담당에서 서비스 담당으로 변화  
클라우드 인프라 보안 책임 : CSP, 클라우드 어플리케이션과 데이터 보안 책임 : 기업 고객 -> 앱과 데이터 사용 현황 가시성과 통제 필요  
기존 보안 솔루션을 클라우드 상에서 그대로 사용 곤란  
기업의 기존 경계 보안(perimeter security) 개념으로 미흠  

* 클라우드 Shadow IT  

Shadow IT : 직업이 기업의 관리하에 있지 않은 IT 제품 / 서비스를 무단으로 업무에 이용하는 것  
기업 클라우드 서비스 담당자가 사용자가 사용하는 클라우드 서비스를 인식 및 관리하지 못함  
어떤 데이터를, 어디에서, 누가 접근하는 지 인식 못함(사각 지대 발생)  
정보화 책임자(CIO)와 정보보호책임자(CISO)의 연계 및 협력 미흡으로 클라우드 서비스 사용에 대한 모니터링 및 관리 부족인 Shadow IT 지대 발생  

* 클라우드 환경의 사용자 인증 및 접근제어  

다수의 사용자가 접근하는 클라우드 환경은 인증 및 접근 제어가 필요함  

* 클라우드 환경에서 보안을 구축하는 방식  

SECaaS(Security as a Service) - 클라우드 내의 보안, SaaS의 한 종류로 클라우드를 이용해 보안 솔루션을 서비스로 제공(WAF, 이메일 보안, 안티 바이러스)
CASB(Cloud Access Service Broker) - 온프레미스와 클라우드 사용 보안, 클라우드 서비스 이용자와 CSP 배치, 단일 통제 포인트를 설정해, 인증, 접근 제어, 데이터 유출 방지, 로그 모니터링, 멀웨어 대응 등 크랄우드 리소스에 대한 접근과 보안 정책 적용 서비스  
SDP(Software Defined Periemter) - 클라우드와 IoT간의 네트워크 보안(Zero Trust)  

* SECurity as a Service (**SECaaS**) - 보안 서비스를 클라우드 서비스 형태로 제공하는 모델  

클라우드 서비스 및 클라우드에 저장된 데이터의 보호 서비스, 기존 IT 환경에서 인프라 및 서비스에 대한 정보 보호 서비스  

* **Container Security** 수요 증가  

* SASE(Secure Access Service Edge) - 네트워크와 시큐리티의 기능을 집약한 클라우드 서비스, 구성요소(Secure Web Gateway, CASB 등)

### CASB(Cloud Access Security Broker)

* 클라우드 서비스 이용 시 예상 리스크  

조직의 관리자 허가 없는 외부 서비스 이용 Shadow IT 발생  

* Shadow IT의 대표적인 위협  

정보 유출의 온상, 위험한 이용 규약, 위험한 규격, 멀웨어 감영 경로로써의 웹  

* 주요 기능  

가시성  
데이터 시큐리티  
컴플라이언스  
위협 방어  

### 클라우드 보안 인증 제도  

* 인증 종류  

SOC1, 2, 3(회계 법인) 보고서, ISO 27001:2005 국제표준, CSA STAR, HITRUST, PCI DSS, ISACA Cloud IT Audit, FedRAMP, 한국 클라우드 보안 인증제(K_FedRAMP)  

* 클라우드 서비스 제공자에 대한 감사  

PCIDSS-ASB, SAS70, CSP에 적용, 보안 컴플라이언스 중요 요소, 제3신뢰기관에 의한 보안 인증 제도 필요, **표준 SLA(Service Level Agreement) 계약, 컨설팅에 관련 있음**  

* 클라우드 컴퓨팅 국제 표준  

ISO/IEC 27001, ITU-T X. 1601 등  

* 클라우드 컴퓨팅 보안 국제 표준 - Cloud Security & Privacy in ISO/IEC SC27  

* 클라우드 보안 가이드라인  

NIST(미국 국립표준기술연구소) 자료를 많이 참고할 것  

* 국내 관련 제도  

K-ISMS(한국인터넷진흥원), CC 인증제도, 금융부문 클라우드 컴퓨팅 보안 가이드, 클라우드서비스인증제도, 클라우드개인정보가이드라인, 클라우드보안인증제도  

* 클라우드 보안 인증 제도  

K_FedRAMP - 법적 요구사항 및 공공기관 추가 요구사항 반영, [클라우드 컴퓨팅 발전 및 이용자 보호에 관한 법률] 제 5조에 의한 클라우드 보안인증제 시행  

정의 : 클라우드 컴퓨팅 서비스 사업자가 제공하는 서비스에 대해 정보보호 기준의 준수 여부 확인을 인증기관이 평가하여 사용자가 안심하고 클라우드 서비스를 이용할 수 있도록 지원하는 제도  

퉁제 항목 : **관리적 보호조치, 기술적 보호조치, 물리적 보호조치, 공공기관용 추가 보호조치**  

보안등급 : 표준 등급(IaaS/SaaS), 간편 등급(SaaS), 두 등급 사이 보안 필요 정도와 갱신 기간이 다름  

### 아이디어  

* 기관 내 전자 투표 시스템(암호화 활용)  

---

## 클라우드 중요성 및 사업 방향  

SK 인포섹 김용철 Cloud 담당  

라이센스를 따기 위해 덤프 보는 것 보다 그 속에 실제 사례들을 잘 캐치하라  

약어에 대해 잘 알아둘 것

you can't secure what you don't know  

* 보안 업무 - 컨설팅, 기술 구축, 관제, 운영  

* VDI, 가상 데스크톱 서비스, 보안 솔루션, 멀티 팩터 인증  

* 홈페이지 <- **IT**(서버 <- 네트워크 <- 데이터베이스) : 취약점 분석을 위해서 코드를 알아야 함  

* 운영, 관리, 하드웨어, 컨설팅, IT를 알아야 보안을 알 수 있다  

* 클라우드 키워드 : **효율성**(여러 사람이 함께 일 할 수 있음, 자원 공유), **생산성**  

* 클라우드 시큐리티 키워드 : **공유**, 스니핑(훔쳐보기), 스푸핑  

* **공용을 전용처럼**(Security)  

* time to market : 적시에 시장에 내놓아 큰 이득을 얻는다  

* managed : 타인이 관리를 해주는 -> 매니지드 서비스로 인해 운영 비용이 감소  

* UNIX 시스템 기반으로 규모가 커졌을 뿐 클라우드 또한 같은 계열의 서비스이다  

* **4차 산업 혁명의 Base는 Cloud**, 클라우드 덕분에 IoT, IoE 등이 발전할 수 있었다  

* 대응 방안 - 관리적(보안컨설팅) / 기술적(보안시스템 구축,관제,운영) / 물리적 : (물리보안) - 방법은 같으나 기술이 다름  

* CSP(cloud service provider), CSC(cloud service custormer), CSPM(cloud security posture management)  , CWPP(cloud workload protection platform)  

* 기술 외적인 보안 요소 : CaaS(Crime as a Service) 공급망, 규제, 컴플라이언스  

* CSA(Cloud Security Alliance) - 클라우드 보안 위협을 대응하기 위해서는 고객(CSC), 서비스 제공자(CSP)가 **책임져야할 각자의 영역을 이해해야함**, 컴플라이언스를 알아야함(해도 되는것, 하면 안되는 것)  

* CSA 프레임워크 ver.4(14개의 도메인) 한번 보기  

---

## Secudium 및 보안개발 분야 소개  

SK 인포섹 민대영 PL

이미테이션 게임 보기  

* Secudium - 보안 관제 플랫폼, ML을 보안 이벤트 탐지에 적용  

Big Data/CEP 기반 차세대 관제 플랫폼, N/W 정보 기반 이상징후 탐지 기술, M/L 기반 자동 정오탐 판정 기술  

* 보안 3 요소 : **CIA**(confidentiality 기밀성, Integrity 무결성, availability - authentication 가용성)  

* 보안 이벤트 데이터를 수집, 정규화, 분석/대응, 조치  

* Event/Log - Anomaly/Detect - Response  

* 데이터 저장 : elastic search / kafka / db  

---

## EQST  

SK 인포섹 EQST 김태형 담당(virusenvy@sk.com)  

* 보안을 고려하지 않으면 개발, 운영 모두 못함  

내가 하는 보안, 클라우드 보안이 실무에 투입 되었을 때 직무를 수행할 수 있는가  

방향이 어떤가 고민해보기, 강사가 말해주는 목적을 생각해보기  

기술보다 앞날을 바라보기  

나 자신을 돌아보는 시간 갖기  

---

## 회사 및 직무 소개  

SK 인포섹 인사 총괄 최세진 파트장  

정보보안 - 눈에 보이지 않는 것을 지킨다  

A(A.I) B(Big Data) C(Cloud)  

보안 업계는 환경에 적응을 잘 하는 것이 중요  

보안은 경험을 수반하지 않으면 지식이 아니다  

관제 = 모니터링 + 대책  

교육과정 후 갈 수 있는 갈래 (보안 담당자 - 시간이 좀 필요함, 보안 서비스 업체, 장비와 보안 솔루션 업체)  

사업 분야 - 보안관제서비스, 보안컨설팅서비스, 보안솔루션 & SI, 융합보안, 고객IT서비스  

* 보안관제서비스 - 인텔리전스 제공/빅데이터 분석  

* 보안컨설팅서비스 - ISMS/개인정보보호/모의해킹  

* 보안솔루션 & SI - 보안 전문 솔루션 판매/구축  

* 융합보안 - 물리융합보안컨설팅, 물리보안시스템/실험실 구축(새로 도전하는 분야)  

* 고객 IT 서비스(관련 없음)  

* 회사 인재상(passion - 신입, innovation - 중간, expertise - 나중) - 회사 면접 = 소개팅

  * 나랑 맞는 회사인지 체크, 면접때 회사가 원하는 것을 어필하고, 나의 강점, 잠재력을 보여줄 수 있어야 함  

* 입사 후 교육제도가 잘되어있으므로 활용할 것  

* 교육과정(1기) 후 면접이 존재  

<!-- ### 현재 모집중인 분야  

* Cloud 보안운영
신입/경력
합정역 인근
[담당업무]
- Cloud 보안솔루션 운영 (파견)
- VDI및 VDI내 단발보안 솔루션 운영
(DLP, NAC, PMS 등)
- 클라우드 환경 보안 솔루션 운영
(방화벽, 망연계, 시스템/DB접근제어 등)
[자격요건]
- 유관 경력: 3년 미만 (신입 가능)
[우대조건]
- 정보보안시스템 운영 경험 보유자
- 보안/ 클라우드 관련 경험, 자격증 보유자
- 고객/팀원과 소통 능력이 원활한 자
- Microsoft Excel 활용능력 우수자

* 보안기획 (보안컨설팅)
[담당업무]
- 정보보호 정책 관리 및 개정
- 개인정보보호 업무 수행
(수탁사 점검, 증적 관리, 이슈대응)
- 정보보호 과제 수행 (개선항목 관련)
[자격요건]
- 신입 가능
- 경력직: IT/보안 경력 2년 이상인 자
- 정보보안 관련 Compliance 이해 보유자
- 개인정보보호 안전성 확보조치 이해 보유자
- 보고서 등 문서작성 Skill 보유자
[우대조건]
- ISMS-P 등 인증심사 대응 경험자
- IT/ 보안관련 자격증 소지자
(정보보안/처리기사, CPPG,CISSP,CISA 등)
- 보안 취약점 진단 (인프라, 웹&App) 경험자
- 보안솔루션 구축 및 운영 경험자 -->  
