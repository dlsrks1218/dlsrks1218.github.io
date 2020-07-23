---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "데이터베이스 2일차"
date: 2020-07-23 20:10:11 -0400
background: '/img/posts/05.jpg'
---

## 데이터베이스 ch03 SQL 기초  

RDBMS, NoSQL(KVS(Key-Value Store), Document Base DB, Column Base DB)  

### 관계형 데이터베이스(RDBMS)  

* 데이터 조회가 주 업무  

* 불필요한 데이터는 검색 속도 저하의 요인  

* 데이터를 쪼갤 수 있을 만큼 쪼개면(정규화) 효율이 좋아지며, 데이터가 아주 많으므로 꼭 정규화가 필요함  

### SQL 기능에 따른 분류  

* 데이터 정의어(Data Definition Language) - 테이블의 관계나 구조를 생성하는 데 사용, CREATE, ALTER, DROP  

* 데이터 조작어(Data Manipulation Language) - 테이블에 데이터를 검색, 삽입, 수정, 삭제하는데 사용(CRUD), SELECT, INSERT, UPDATE, DELETE  

* 데이터 제어어(Data Control Language) - 데이터의 사용 권한을 관리하는 데 사용, GRANT, REVOKE 등  

### SQL  

* 정렬  

  * ORDER BY - 기본적으로 DESC 옵션을 주지 않으면 ASC(오름차순) 정렬을 수행  

* 집계 함수

  * SUM, AVG, COUNT, MAX, MIN  

  * GROUP BY - 집계 가능한 항목에 대해서만 적용 가능하다, ASC, DESC로 정렬된 그룹핑 가능  

    * HAVING - 집계가 된 데이터에 대한 조건 추가  

* **Left Outer Join** - 보통 왼쪽 데이터를 기준으로 다 보여주는 경우가 많아 Right Outer Join도 Left로 고쳐서 보여주는 경우가 많다  

```sql
select distinct C.name, B.bookname, B.price
from Book as B, Customer as C LEFT OUTER JOIN Orders as O ON C.custid=O.custid;
```

* **Join**  

```sql
select distinct C.name, B.bookname, B.price
from Book as B, Customer as C JOIN Orders as O ON C.custid=O.custid
where B.price >= 20000;
```

* 부속 쿼리(Sub Query)  

```sql
select bookname, max(price)
from Book
where price=(select MAX(price) from Book)
group by bookname
-- 도서를 구매한 적이 있는 고객의 이름을 검색하시오.
select name
from Customer
where custid in (select custid from Orders);
-- 대한미디어에서 출판한 도서를 구매한 고객의 이름을 보이시오.
select name from Customer where custid in (
    select custid from Orders where Orders.bookid in(
        select bookid from Book where publisher='대한미디어'
        )
    )
-- 출판사별로 출판사의 평균 도서 가격보다 비싼 도서를 구하시오
select b1.bookname, b1.publisher from Book b1 where b1.price > (
  select avg(b2.price) from Book b2 where b2.publisher=b1.publisher
  );
-- Join으로 바꿔보기
SELECT b1.bookname, b1.publisher
FROM Book b1 JOIN Book b2 ON b1.publisher=b2.publisher
WHERE b1.price >= avg(b2.price)

SELECT name FROM Customer WHERE address LIKE '대한민국%'
UNION
SELECT name FROM Customer WHERE custid IN (
  SELECT custid FROM Orders
  )
```

### 데이터 정의어(DDL)  

테이블 구성, 속성에 관한 제약 정의, 기본키 및 외래키 정의  

CREATE, UPDATE, ALTER  

* CREATE(Primary Key는 여럿이 와도 됨 - 복합키)

CREATE TABLE 테이블이름
( { 속성이름 데이터타입
[NOT NULL | UNIQUE | DEFAULT 기본값 | CHECK 체크조건]
}
[PRIMARY KEY 속성이름(들)]
{[FOREIGN KEY 속성이름 REFERENCES 테이블이름(속성이름)]
[ON DELETE [CASCADE┃SET NULL]
}
)

* VARCHAR(20) - 문자가 들어갈 공간을 20 확보하지만 가변 길이로써 사용 안하는 곳은 제거, 효율적  

* CREATE 문에서 Foriegn Key 지정 시 제약  

  * 외래키 제약조건을 명시할 때는 반드시 참조되는 테이블(부모 릴레이션)이 존재해야 하며(참조 무결성 제약) 참조되는 테이블의 기본키여야 함  

  * ON DELETE 또는 ON UPDATE 옵션은 참조되는 테이블의 튜플이 삭제되거나 수정될 때 취할 수 있는 동작을 지정함  

* ALTER  

  * ALTER 문은 생성된 테이블의 속성과 속성에 관한 제약을 변경하며, 기본키 및 외래키를 변경함  

  * ADD, DROP은 속성을 추가하거나 제거할 때 사용함  

  * MODIFY는 속성의 기본값을 설정하거나 삭제할 때 사용함  

  * ADD <제약이름>, DROP <제약이름>은 제약사항을 추가하거나 삭제할 때 사용함  

  * ADD, DROP, MODIFY  

```sql
drop database if exists mydb;
create database mydb;
use mydb;
drop table if exists NewBook;
create table NewBook(
    bookid INTEGER NOT NULL,
    bookname VARCHAR(20) NOT NULL,
    publisher VARCHAR(20)  NULL,
    price INTEGER NOT NULL,
    PRIMARY KEY(bookid)
);

DROP TABLE IF EXISTS NewCustomer;
CREATE TABLE NewCustomer(
    custid INTEGER PRIMARY KEY,
    name VARCHAR(40) NOT NULL,
    address VARCHAR(40),
    phone VARCHAR(15) DEFAULT '000-0000-0000'
);
-- 다음과 같은 속성을 가진 NewOrders 테이블을 생성하시오.
-- • orderid(주문번호) - INTEGER, 기본키
-- • custid(고객번호) - INTEGER, NOT NULL 제약조건, 외래키(NewCustomer.custid, 연쇄삭제)
-- • bookid(도서번호) - INTEGER, NOT NULL 제약조건
-- • saleprice(판매가격) - INTEGER
-- • orderdate(판매일자) – DATE

DROP TABLE IF EXISTS NewOrders;
CREATE TABLE NewOrders(
  orderid INTEGER,
  custid INTEGER NOT NULL,
  bookid INTEGER NOT NULL,
  saleprice INTEGER,
  orderdate DATE,
  PRIMARY KEY (orderid),
  FOREIGN KEY (custid) REFERENCES NewCustomer(custid) ON DELETE CASCADE
);

ALTER TABLE NewBook ADD isbn VARCHAR(20);
ALTER TABLE NewBook MODIFY isbn INTEGER;
```

### 데이터 조작어(DML)  

* INSERT  
  
  * VALUES 뒤에 SELECT 문 결과도 올 수 있음  

INSERT INTO <테이블> VALUES( , , )  

```sql
INSERT INTO Book(bookid, bookname, publisher, price) VALUES(11, '스포츠 의학', '한솔의학서적', 90000);
INSERT INTO Book(bookid, bookname, publisher) VALUES(12, '스포츠 의학', '한솔의학서적');

INSERT INTO Book(bookid, bookname, publisher, price) SELECT bookid, bookname, publisher, price FROM Imported_Book;
```

* UPDATE  

UPDATE <테이블> SET <바꿀 속성>=<값> WHERE <조건>  

```sql
update Book set publisher=(select publisher from Imported_Book where Imported_Book.bookid=21) where Book.bookid=14;
```

* DELETE

DELETE FROM <테이블> WHERE <조건>

```sql
DELETE FROM Book WHERE bookid between 12 and 14;
```

* TRUNCATE

TRUNCATE table <테이블>

* 'Truncate' 는 레코드 단위가 아닌 테이블을 Drop 한 후 재 생성하는 과정을 거침 (테이블의 전체 내용 제거 시 'delete' 보다 빠름)  

---

## 실습  

```sql
-- distinct : 중복 없이
SELECT DISTINCT publisher, COUNT(publisher)
FROM book
GROUP BY publisher

-- 1차 출판사 오름차순 정렬, 2차 가격 내림차순 정렬, 옵션 주지 않으면 ASC(오름차순)으로 정렬
SELECT *
FROM Book
ORDER BY publisher ASC, price DESC;

use madang;

select distinct publisher, count(publisher) from Book group by publisher;

SELECT * FROM Book WHERE price < 20000;

SELECT * FROM Book WHERE price between 10000 AND 20000;
SELECT * FROM Book WHERE price >= 10000 AND price <= 20000;

SELECT * FROM Book WHERE publisher = '굿스포츠' OR publisher = '대한미디어';
SELECT * FROM Book WHERE publisher IN ('굿스포츠', '대한미디어');

SELECT * FROM Book WHERE bookname LIKE '축구%';
SELECT * FROM Book WHERE bookname LIKE '%구%';

SELECT * FROM Book ORDER BY publisher ASC, price DESC;

select sum(saleprice) as total_sum, avg(saleprice) as average, count(saleprice) as cnt, max(saleprice) as maximum, min(saleprice) as minimum from Orders;

SELECT custid, count(*), sum(saleprice), avg(saleprice) FROM Orders GROUP BY custid;

select custid, count(*) as '수량' from Orders where saleprice >= 8000 group by custid having count(*) >= 2;

select custid, sum(saleprice) as '총구매액' from Orders group by custid having sum(saleprice) >= 30000;

select name, saleprice, sum(saleprice) from Customer, Orders where Customer.custid = Orders.custid order by Customer.custid group by name;

select name, sum(saleprice) from Customer, Orders where Customer.custid = Orders.custid group by Customer.name ASC;
```

## 퀴즈

### 1번  

```sql
show tables;
select * from Book;
select * from Customer;
select * from Orders;

select bookname, price, publisher, name
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name='박지성';

# 박지성의 총 구매액
select sum(price)
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name='박지성';
# 박지성이 구매한 도서의 수
select count(*)
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name='박지성';
# 박지성이 구매한 도서의 출판사 수
select count(publisher)
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name='박지성';
# 박지성이 구매한 도서의 이름, 가격, 정가와 판매가격의 차이
select bookname, price, (price - saleprice) as '정가와의 차이'
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name='박지성';
# 박지성이 구매하지 않은 도서의 이름
select bookname
from Orders as O, Book as B, Customer as C
where O.custid=C.custid and O.bookid=B.bookid and C.name!='박지성';
# 2014년 7월 4일~7월 7일 사이에 주문받은 도서의 주문번호
select orderid
from Orders
where orderdate between '2014-07-04' and '2014-07-07';
# 2014년 7월 4일~7월 7일 사이에 주문받은 도서를 제외한 도서의 주문번호
select distinct bookid
from Orders
where orderdate not in (
    select orderdate
    from Orders
    where orderdate between '2014-07-04' and '2014-07-07'
    );
# 고객의 이름과 고객이 구매한 도서 목록
select C.name, B.bookname
from Orders as O, Book as B, Customer as C
where O.custid = C.custid and O.bookid = B.bookid;
# 도서의 판매액 평균보다 자신의 구매액 평균이 더 높은 고객의 이름
select C.name, avg(O.saleprice)
from Orders as O, Customer as C
where C.custid=O.custid
group by C.name
having avg(O.saleprice) > (
    select avg(saleprice) from Orders
    );
# 박지성이 구매한 도서의 출판사와 같은 출판사에서 도서를 구매한 고객의 이름
# 틀림
# select distinct C.name
# from Orders as O, Book B, Customer as C
# where O.custid=C.custid and C.name!='박지성' and B.publisher in (
#     select B.publisher
#     from Orders as O, Book as B, Customer as C
#     where O.custid=C.custid and C.name='박지성'
#     group by publisher
#     );
# 정답
select name from Customer, Orders, Book
where Customer.custid=Orders.custid and Orders.bookid=Book.bookid and name not like '박지성' and publisher in(
    select publisher from Customer, Orders, Book
    where Customer.custid=Orders.custid and Orders.bookid=Book.bookid and name like '박지성'
    );
# 두 개 이상의 서로 다른 출판사에서 도서를 구매한 고객의 이름
select C.name
from Orders as O, Book as B, Customer as C
where O.custid = C.custid and O.bookid = B.bookid
group by C.name
having count(distinct publisher) >= 2;
# 정답
select name from Customer c1
where 2 <= (
    select count(distinct publisher)
    from Customer,
         Orders,
         Book
    where Customer.custid = Orders.custid
      and Orders.bookid = Book.bookid
      and name like c1.name
);
# 전체 고객의 30% 이상이 구매한 도서
# 틀림
# select count(*), C.name
# from Customer as C, Orders as O, Book as B
# where C.custid=O.custid and B.bookid=O.bookid
# group by C.name
# having count(*) >= (select count(distinct name) * 0.3 from Customer);
# 오류 수정하면 맞을거라
# select bookname from Book B1
# where (
#     (select count(B.bookid) from Book as B, Orders as O where B.bookid=O.bookid)
# >= (select count(distinct name) * 0.3 from Customer)
# );
# 정답
select bookname from Book B1
where (
    (select count(Book.bookid) from Book, Orders where Book.bookid=Orders.bookid and Book.bookid=B1.bookid)
>= 0.3 * (select count(*) from Customer)
);
```

### 2번  

```sql
# 팀장(manager)의 이름을 보이시오.
select name from Employee where position='Manager';
# ‘IT’ 부서에서 일하는 사원의 이름과 주소를 보이시오.
select name, address
from Employee as E, Department as D
where E.deptno=D.deptno and D.deptname='IT' and E.position!='Manager'
# ‘홍길동’ 팀장(manager) 부서에서 일하는 사원의 수를 보이시오.
select count(*) as '홍길동 팀장 밑에서 일하는 사원 수' from Employee where deptno =(
    select deptno from Department where manager='홍길동'
);
# 사원들이 일한 시간 수를 부서별, 사원 이름별 오름차순으로 보이시오.
select hoursworked, deptname, name
from Works as W, Employee as E, Department as D
where W.empno=E.empno and E.deptno=D.deptno
order by deptname, name
# 두 명 이상의 사원이 참여한 프로젝트의 번호, 이름, 사원의 수를 보이시오.
select W.projno, P.projname, count(*) as '사원 수'
from Employee as E, Project as P, Works as W
where W.projno = P.projno and E.empno=W.empno
group by W.projno
having count(*) >= 2;
# 세 명 이상의 사원이 있는 부서의 사원 이름을 보이시오.
select E.name
from Employee as E, Department as D
where E.position!='Manager' and E.deptno = D.deptno and D.deptname = (
    select D.deptname
    from Employee as E, Department as D
    where E.deptno=D.deptno
    group by D.deptname
    having count(*) >= 3
)
```

---

## 이슈  

### 현재 로컬 도커는 encoding 세팅을 못했기때문에 아래 명령어가 필요함  

```sql
ALTER TABLE <테이블이름> CONVERT TO character SET utf8;
```