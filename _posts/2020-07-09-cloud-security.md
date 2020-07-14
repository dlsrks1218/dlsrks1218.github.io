---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "python 스터디"
date: 2020-07-09 20:10:11 -0400
background: '/img/posts/03.jpg'
---

## 자료구조  

* 스택 : LIFO  

* 큐 : FIFO  

* 리스트 : [] 순서가 존재  

* 튜플 : () 변경 허용 X  

* 셋 : {} 중복 허용 X  

* 딕셔너리 : 순서가 없음, Key, Value 한 쌍으로 저장됨, Key는 중복 X  

* 콜렉션 모듈 : 위 자료구조를 효율적으로 사용할 수 있도록 지원하는 파이썬 내장(Built-in) 모듈  

## 스택(Stack)  

### 스택  

* 자료구조의 핵심 개념 중 하나로 **Last In First Out**으로 정의  

### Push()  

* 스택에 데이터 저장

### Pop()  

* 데이터를 추출  

### 구현  

* python에서는 리스트를 사용해 구현  

* append() 함수로 데이터를 저장(push)  

* 추출(pop)  

### 큐(Queue)  

* 스택과 다르게 먼저 들어간 데이터가 먼저 나오는 **First In First Out** 메모리 구조  

### 튜플(Tuple)  

* 리스트와 같은 개념이지만, 데이터를 **변경할 수 없는** 자료구조  

* 학번, 이름, 주민등록번호 등 변경할 수 없는 데이터를 위해 사용  

* ()로 표현  

### 셋(Set)  

* 값을 순서 없이 저장하며 중복을 허용하지 않는 자료형  

* 튜플과 다르게 삭제나 변경이 가능  

* 다양한 집합 연산 제공  

```python
# 값 추가
add()
# 값 제거, 지우려는 값이 없으면 KeyError 발생
remove()
# 값(리스트) 추가
update()
# 값 제거, 지우려는 값이 없어도 에러 발생 X
discard()
# 셋 비우기
clear()
```

### 딕셔너리(Dictionary)  

* 전화번호부와 같이 Key와 Value 형태로 데이터를 저장하는 자료구조  

* Key는 유일해야 함  

* 딕셔너리는 순서가 없음 

```python
keys()
values()
# tuple 형태의 아이템들의 리스트 출력
items()

```

## 반복문 Loop  

### while  

* 조건  

### for  

* 횟수  

## 람다 함수(Lambda Function)  

### 람다 함수 -> 함수형 프로그래밍  

* 함수의 이름 없이, 함수처럼 사용할 수 있는 익명의 함수  

* 이름 지정 없이 사용 가능  

* lambda (Parameter): (수행할 동작)  

```python
f = lambda x, y: x + y
print(f(1, 4))

# dictionary.items()[1]를 dictionary의 value로 정렬
sorted(dictionary.items(), key = lambda x: x[1], reverse=True)
```
