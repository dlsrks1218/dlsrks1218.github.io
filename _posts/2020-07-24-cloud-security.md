---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "데이터베이스 3일차"
date: 2020-07-24 20:10:11 -0400
background: '/img/posts/01.jpg'
---

## 데이터베이스 ch04 SQL 고급  

### SQL 내장 함수  

* 숫자 함수 - ABS, ROUND(반올림), CEIL(올림), FLOOR(버림), POWER(제곱), SIGN(음수는 -1, 양수는 1, 0은 0) 등  

  * FROM Dual -> 임시 테이블  

* 문자 함수 - CONCAT, SUBSTR(지정한 길이만큼의 문자열을 반환하는 함수), LOWER, UPPER, TRIM(공백 제거), REPLACE(원본, src, dest), RPAD, LPAD(채우기) 등  

* 날짜 시간 함수 - DATE_FORMAT(날짜형을 문자형으로 변환), CURRENT_DATE, DATE, DATEDIFF, ADDDATE, NOW(MySQL 함수) 등  

* 집계 함수 - AVG, COUNT, MAX, MIN, STD, STDDEV, SUM  

```sql
-- 고객별 평균 주문 금액을 백원 단위로 반올림한 값을 구하시오 (use madang;)
select ROUND(SUM(saleprice) / COUNT(*), -2), custid
from Orders
group by custid

-- 성, 이름 따로 출력
select SUBSTR(name,1, 1) '성', SUBSTR(name, 2, 2) '이름' from Customer;
-- 이름에서 성을 기준으로 그룹핑하여 성씨 별 사람 수 구하기
select SUBSTR(name,1, 1) '성', COUNT(name) '인원' from Customer group by SUBSTR(name,1, 1);
-- 주문 날짜 10일 뒤를 확정으로 함
select orderid '주문 번호', ADDDATE(orderdate, INTERVAL +10 DAY) '확정' from Orders
-- 마당서점이 2014년 7월 7일에 주문받은 도서의 주문번호, 주문일, 고객번호, 도서번호를 모두 보이시오. 단, 주문일은 '%Y-%m-%d' 형태로 표시한다.
select orderid 주문번호, STR_TO_DATE(orderdate, '%Y-%m-%d') 주문일자, custid 고객번호, bookid 도서번호
from Orders
where orderdate=DATE_FORMAT('2014-07-07', '%Y%m%d');
-- 출력 Y->y, m->M 하면 영문으로 표기됨
select orderid 주문번호, DATE_FORMAT(orderdate, '%y-%M-%d') 주문일자, custid 고객번호, bookid 도서번호
from Orders
where orderdate=DATE_FORMAT('2014-07-07', '%Y%m%d');
-- NULL값을 대체, 추후 통계내거나 할 때 널값 처리하기 위해 사용
select name, IFNULL(phone, '연락처 없음') '전화번호'
from Customer
```  

---

### 부속 질의(Subquery)  

하나의 SQL문(Mainquery) 안에서 다른 SQL문(Subquery)이 중첩된(nested) 질의  

* 다른 테이블에서 가져온 데이터로 현재 테이블에 있는 정보를 찾거나 가공할 때 사용

* 보통 데이터가 대량일 때 데이터를 모두 합쳐서 연산하는 조인보다 필요한 데이터만 찾아서
공급해주는 부속질의가 성능이 더 좋음  

* SELECT절 -> 스칼라 부속질의(scalar subquery) : subquery의 결과가 하나(단수)인 것, 일반적으로 SELECT 절, UPDATE SET 안에서 사용  

  * 복수일 때는 IN 키워드 사용  

```sql
-- 고객 별로 도서 구매 금액 구하기
select Orders.custid, name, sum(saleprice)
from Orders, Customer
where Orders.custid=Customer.custid
group by Orders.custid;
-- 부속 질의로 변경
select O.custid, (select C.name from Customer C where C.custid=O.custid) name, sum(saleprice)
from Orders O
group by O.custid;
-- Orders 테이블에 각 주문에 맞는 도서이름을 입력하시오
select *, (select bookname from Book where Book.bookid=Orders.bookid) '책제목' from Orders
```

* FROM 절 -> 인라인 뷰(inline view) : 테이블 이름 대신 인라인 뷰 부속질의를 사용하면 보통의 테이블과 같은 형태로 사용할 수 있음, FROM절에서 사용, 가상의 테이블인 뷰 형태로 제공  

```sql
-- 고객번호가 2 이하인 고객의 판매액을 보이시오(고객이름과 고객별 판매액 출력)
select c.name, sum(o.saleprice) 'total'
from Customer c, Orders o
where c.custid=o.custid and c.custid <= 2
group by c.name
-- 인라인 뷰 사용
select c.name, sum(o.saleprice) 'total'
from (select custid, name from  Customer where custid <= 2) c, Orders o
where c.custid=o.custid
group by c.name
```  

* WHERE절 -> 중첩질의(nested subquery)  

  * 비교(=, >, <, >=, <=)  

  * 집합(IN, NOT IN)  

  * 한정(ALL, SOME)  

  * 존재(EXISTS, NOT EXISTS)  

---

### 뷰  

하나 이상의 테이블을 합하여 만든 **가상의 테이블**  

* 장점  

  * 속도가 더 빠름  

  * 편리성 및 재사용성 - 자주 사용되는 복잡한 질의를 뷰로 미리 정의 해놓을 수 있음  

  * 보안성 - 원본 테이블 값을 직접 사용하지 않고 필요한 값만 사용하기 때문에  

  * 독립성 - 필요한 데이터만 가공해서 보여줄 수 있기 때문에  

* 특징  

  * 원본 데이터 값에 따라 같이 변함  

  * 독립적인 인덱스 생성이 어려움  

  * 삽입, 삭제, 갱신 연산에 많은 제약이 따름  

```sql
create view vBook as SELECT * FROM Book WHERE bookname like '%축구%';
select * from vBook;

CREATE OR REPLACE VIEW vOrders (orderid, custid, name, bookid, bookname, saleprice, orderdate) as
    select o.orderid, o.custid, c.name, o.bookid, b.bookname, o.saleprice, o.orderdate from Orders o, Customer c, Book b where o.custid=c.custid and o.bookid=b.bookid
select * from vOrders where name = '박지성';

create view vCustomer as select custid, name, address, phone from Customer where address like '대한민국%';
create or replace view vCustomer(custid, name, address) as select custid, name, address from Customer where address like '영국%';
select * from vCustomer;

-- 고객 별 주문합계를 나타내는 뷰(vOrderTotal) 생성하기
create or replace view vOrderTotal as
select name, sum(saleprice) as 'total' from Customer, Orders where Customer.custid=Orders.custid group by Customer.custid order by total desc;
select * from vOrderTotal;

drop view <뷰이름>;
```

## 퀴즈  

### 3번  

```shell
use madang;
# 판매가격이 20,000원인 도서의 도서번호, 도서이름, 고객이름, 출판사, 판매가격을 보여주는 highorders 뷰를 생성하시오.
create view highorders as
    select o.bookid, b.bookname, c.name, b.publisher, o.saleprice
    from Orders o, Customer c, Book b
    where saleprice = 20000 and o.custid=c.custid and o.bookid=b.bookid;
select * from highorders;
# highorders 뷰를 변경하고자 한다. 판매가격 속성을 삭제하는 명령을 수행하시오. 삭제 후 (2)번 SQL 문을 다시 수행하시오.
create or replace view highorders as
    select o.bookid, b.bookname, c.name, b.publisher
    from Orders o, Customer c, Book b
    where saleprice = 20000 and o.custid=c.custid and o.bookid=b.bookid;
select * from highorders;

# 모듈 프로젝트 때 사용할 것
use hr;
# 팀장(MGR)이 없는 직원의 이름을 보이시오.
select CONCAT(first_name, ' ', last_name) '팀장이 없는 부서 직원 이름' from employees where manager_id is null;
# 사원의 이름과 부서의 이름을 보이시오(조인/스칼라 부속질의 사용).
## 조인, 스칼라 부속질의
select CONCAT(e.first_name, ' ', e.last_name) '이름',
       (select department_name from departments d where d.department_id=e.department_id) '부서이름'
from employees e;
# ‘Seattle’에 근무하는 사원의 이름을 보이시오
## 조인, 인라인 뷰, 중첩질의, EXISTS (중에 편한것 골라서 만들기)
select CONCAT(e.first_name, ' ', e.last_name) '이름', l.city
from employees e, departments d, locations l
where e.department_id=d.department_id and d.location_id = (
    select location_id
    from locations
    where city like '%Seattle%'
    );
# 평균보다 급여가 많은 직원의 이름을 보이시오.
SELECT CONCAT(first_name, ' ', last_name) '이름'
FROM employees
WHERE salary > (
SELECT AVG(salary) FROM employees);
# 자기 부서의 평균보다 급여가 많은 직원의 이름을 보이시오(상관 부속질의 사용).
select CONCAT(e.first_name, ' ', e.last_name) '이름'
from employees e join departments d on e.department_id=d.department_id
where salary > (
    select avg(salary) from employees where employees.department_id=e.department_id
);
```

---

### 인덱스  

* 정의 : 도서의 색인이나 사전과 같이 데이터를 쉽고 빠르게 찾을 수 있도록 만든 데이터 구조  

* 특징  

  * 테이블에서 한 개 이상의 속성을 이용하여 생성  

  * 빠른 검색, 효율적 레코드 접근이 가능  

  * 순서대로 정렬된 속성과, 데이터 위치만 보유하므로 테이블보다 작은 공간 차지  

  * 저장된 값들은 테이블의 부분 집합이 됨  

  * B-tree 형태 구조가 일반적임  

  * 데이터의 수정, 삭제 등 변경 발생 시 인덱스의 재구성이 필요  

* 종류(MySQL)  

  * 클러스터 인덱스(Cluster Index)  

    * 테이블 생성시 기본키를 지정하면 기본키에 대해 클러스터 인덱스 생성  

    * Primary Key를 지정 안하면 먼저 나오는 UNIQUE 속성에 대해 클러스터 인덱스 생성  

    * PK나 Unique 속성이 없는 테이블은 MySQL이 자체 생성한 행번호로 클러스터 인덱스 생성  

  * 보조 인덱스(Secondary Index)  

    * 클러스터 인덱스가 아닌 모든 인덱스를 칭함  

    * 보조 인덱스의 각 레코드는 (보조 인덱스 속성과 기본키 속성 값)을 가진다  

    * 보조 인덱스 검색 -> 기본키 속성 값 찾기 -> 클러스터 인덱스로 가서 해당 레코드 찾기  

* 인덱스 생성시 고려할 사항  

  * WHERE 절에 자주 사용되어야 함  

  * 조인에 자주 사용되어야 함  

  * 단일 테이블에 인덱스가 많으면 느려짐(테이블 당 4~5개)  

  * 속성이 가공되는 경우(데이터의 수정, 삭제 등 변경 발생) 사용하지 않음  

  * 속성의 선택도가 낮을 때 유리함(속성의 모든 값이 다른 경우)  

---

### 데이터베이스 프로그래밍  

* 방법 : SQL 전용 언어 사용, 일반 프로그래밍 언어에 SQL 삽입, 웹 프로그래밍 언어에 SQL 삽입 등  

* 데이터베이스 연동 파이썬 프로그래밍  

  * DBMS - MySQL / DB - hr  

  * Programming Language - Python -> Application  

* Module(Connection - [ip/id/pw/port])  

  * Connection 갯수가 한정 되어있기 때문에 사용이 끝나면 close() 해주어야 함 -> **Connection Pool(접속 가능한 사용자 수를 의미함)**  

* SQL중 DML(CRUD)을 주로 사용할 예정  

  * INSERT, UPDATE, DELETE 의 결과는 숫자  

  * SELECT의 결과는 결과 집합  

---

### 데이터 모델링  

현실 세계에 존재하는 정보를 데이터베이스에 저장하기 위해 도식화 하는 것  

DB 설계 시 고민할 것  

1. 공간(엔티티) 도출 방법  

2. 기본 키 설정  

3. 중복의 제거(정규화)  

* Entity : 데이터가 저장되는 공간  

* 개념적 모델링(E-R 다이어그램) - **개념적 스키마(사용자 식별, 디비 용도 식별, 요구사항 수집 및 명세, 엔티티 도출, ER 다이어그램 작성)**  

* 논리적 모델링(관계 데이터 모델 ex : 테이블1(속성1, 속성2, 속성3)) - **논리적 스키마**  

* 물리적 모델링 - **물리적 스키마(DB 개체 정의, 테이블 및 인덱스 등 설계)**  

데이터베이스 생명주기  

* 요구사항 수집 및 분석(해당 분야에 대한 도메인 지식이 필요) -> 설계 -> 구현 -> 운영 -> 감시 및 개선  

개념적 모델링  

* 요구사항을 수집하고 분석한 결과를 토대로 업무의 핵심적 개념을 구분하고 전체적인 뼈대를 만드는 과정  

* 엔티티를 추출하고 각 개체들 간의 관계를 정의하며 ER 다이어그램을 만드는 과정까지를 말함  

논리적 모델링  

* 개념적 모델링에서 만든 ER 다이어그램을 사용하려는 DBMS에 맞게 매핑하여 실제 데이터베이스로 구현하기 위한 모델을 만드는 과정  

  * 예시 : 도서(도서번호, 도서이름, 출판사이름, 도서단가)  

물리적 모델링  

* 논리적 모델을 실제 컴퓨터와 저장 장치에 저장하기 위한 물리적 구조를 정의하고 구현하는 과정  

* DBMS의 특성에 맞게 저장 구조를 정의해야 데이터베이스가 최적의 성능을 낼 수 있음  

ER 모델(Entity-Relationship Model)  

* 개체(Entity) : 정보를 가지고 있는 독립석인 실체, 개체의 특성을 나타내는 속성에 의해 식별됨, 개체끼리 서로 관계를 가짐  

* E-R Diagram : 개체와 개체 간의 관계를 표준화된 그림으로 나타낸 것  

* 관계 차수  

  * 직원과 프로젝트가 1:N의 관게이다  

  * 직원은 여러 프로젝트를 가질 수 있다  

  * 프로젝트는 한 직원에 의해서만 수행된다  

* 개체 : 직사각형, 관계 - 마름모, 속성 - 타원  

* 속성(attribute) : 개체가 가진 성질  

  |개체 타입|속성|
  |------|---|---|
  |도서|도서이름, 출판사, 도서단가|  

  * 복합속성 : 속성은 하위속성을 가질 수 있다  

* 관계(relationship) : 개체 사이의 연관성을 나타내는 개념  

* 참조 키 관계(참조 무결성) -> 데이터를 주는 쪽 : 부모 테이블 / 데이터를 받아 쓰는 쪽 : 자식 테이블(반드시 존재하는 데이터를 참조해야 한다)  

* 관계 타입 : [1:1, 1:N, N:M]  

  * RDBMS에서는 N:M 관계를 만들 수 없기에 1:N 두 개로 쪼개야함  

IE 표기법

* 식별 관계 : 값을 가져오는 부모 테이블의 값이 자식 테이블의 키값으로 사용되면 식별관계  

* 비식별 관계 : 값을 가져오는 부모 테이블의 값이 자식 테이블의 키값이 아닌 도메인 값으로 사용되면 비식별관계(점선으로 표시)  

* 1:N 관계 : N쪽이 새발로 표시됨  

* --O- : 최소 참여가 0, 0명 이상 참여, Optional, Zero, 비식별관계  

* --1- : 최소 참여가 1, 1명 이상 참여, Mandatory  

---

### MySQL Workbench로 E-R 다이어그램 그리기 실습  
