---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "Linux & Docker 스터디 1일차"
date: 2020-07-14 20:10:11 -0400
background: '/img/posts/04.jpg'
---

## 리눅스  

리눅스 토발즈에 의해 개발된 OS

### GNU  

유닉스와 호환되는 소프트웨어를 개발하는 프로젝트  

* **GNU 선언문**

### 라이센스  

* GPL(**추후 조사할 것**)  

* **민감한 부분이므로 소프트웨어 출시 전 꼭 고려를 해야 함**  

### 배포판  

* 레드햇 계열  

* 데비안 계열  

### 특징과 구조  

* 추후 조사  

### 커널  

* 하드웨어 제어에 관한 OS의 핵심이 되는 기능(응용프로그램, 메모리, 파일시스템, 장치 관리)을 담당  

* 컴퓨터의 모든 자원 초기화 및 제어  

* 디바이스 관리 : 하드웨어(CPU/메모리/디스크/입출력)를 디바이스 드라이버라는 소프트웨어를 이용해 제어  

* 프로세스 관리 : 리눅스는 명령을 실행할 때 해당 프로그램 파일에 쓰여 있는 내용을 읽어 들여 메모리상에 전개한 후 메모리 상의 프로그램(프로세스)을 실행  

  * 이러한 프로세에 PID라는 식별자를 붙여 관리함  

  프로세스의 실행을 위해 필요한 CPU를 효율적으로 할당하는 역할을 하고 있음  

* 메모리 관리 : ㅍ로세스 실행 시 메모리상에 프로그램이 전개됨과 동시에 프로그램 안에서 이용하는 데이터도 메모리 상에 전개됨  

* 조작은 Shell로 함

---

## 리눅스 명령어  

### 파일과 디렉토리  

* 파일  

* 디렉토리  

* 심볼릭 링크  

### 디렉토리의 계층 구조  

* 트리 구조이며 루트 디렉토리(/) 한 개 휘하의 서브 디렉토리 여러 개  

* 중요 루트 디렉토리  

  1. /dev - 장치 파일이 담긴 디렉토리  

  2. /home - 사용자 홈 디렉토리가 생성되는 디렉토리  

  3. /opt - 추가 패키지가 설치되는 디렉토리  

  4. /root - 루트 계정의 홈 디렉토리  

  5. /usr(unix system resource) -> Windows의 Program Files와 같은 역할  

  6. /etc - 각종 설정 파일  

* 현재 디렉토리 : (.)  

* 작업 디렉토리 : (pwd - present working directory)  

* 홈 디렉토리로 이동 : (~)  

* 절대 경로는 /로 표시, 상대 경로는 /로 표시 안함 

### 파일, 디렉토리 관련 명령어  

* history로 나온 명령어의 번호를 기억해 **!번호**하면 해당 명령어 다시 수행해줌  

```shell
# one디렉토리안에 tmp디렉토리가 없어도 -p 입력시 생성한 후 그 안에 test 생성
mkdir -p /one/tmp/test
# test가 디렉토리이므로 안의 내용까지 복사하기 위해 -r 명령어 추가
cp -r ./test ./new_test
# a를 b에 옮긴다, b가 없으면 a를 복사해서 만듦
mv a b
# 파일 생성  
vi 파일명, touch 파일명, cp 원본 사본  
# 파일 복사  
cp
# 파일 이동, 파일명 변경  
cp, mv  
# 파일, 디렉토리 삭제  
rm -rf(recursive + force)  
# 디렉토리
mkdir, cd, rmdir(rm -rf)
# 현재 디렉토리 확인  
pwd  
# 사용자 전환  
su -
# 파일 목록 상세 확인
ls -l, ls -al  
```  

### 하드 링크, 심볼릭 링크  

```shell
# 하드 링크 생성, 원본 삭제 되어도 링크 파일 사용 가능
ln hosts hosts.ln

# 심볼릭 링크, 원본 삭제 되면 링크 파일 사용 불가
ln -s hosts s_hosts.ln
ln -s /원본/경로/abc.sh ./abc  
```

### 검색  

* grep  

  ```shell
  # /usr/bin 디렉토리에서 cat라는 문자열이 포함된 디렉토리 혹은 파일을 출력
  cat yum | grep import
  sudo yum install net-tools
  netstat -tnlp | grep tcp
  ```

* find  

  ```shell
  find ./ -name hosts | xargs grep "localhost"
  
  # localhost is used to configure the loopback interface
  127.0.0.1       localhost
  ::1             localhost
  ```  

## 파일의 속성  

* -rw-r--r--. 1 root root 240 Jul 14 05:04 /etc/hosts  

* 권한 / 하드링크 갯수 / 소유자의 로그인 id / 파일이 속한 그룹 / 파일 사이즈 / 생성 시간 / 파일명  

* 권한은 **소유자, 그룹, 기타 사용자** 별로 3자리씩 read(r), write(w), execute(x) 권한을 나타냄  

```shell
$ ls -l /etc/hosts
-rw-r--r--. 1 root root 240 Jul 14 05:04 /etc/hosts
권한 / 하드링크 갯수 / 소유자의 로그인 id / 파일이 속한 그룹 / 파일 사이즈 / 생성 시간 / 파일명
# -rw-r--r--에서 첫 -는 파일의 종류(-:일반파일, d:디렉토리)
# 첫-를 제외하고 9자리는 각 3 그룹별(소유자, 그룹, 기타 사용자)로 rwx rwx rwx를 나타냄
# 각 3자리를 2진수로 나타내서 권한 부여 가능
# r : read
# w : write
# x : excute

$ file /etc/hosts
>> /etc/hosts: ASCII text

```  

### 파일 권한 변경  

* 기호나 숫자로 변경 가능  

* 기호 r, w, x  

* **숫자로 변경 시 3그룹 각자 3자리의 2진수 표현으로 각각 권한 부여 가능**  

```shell
# other 사용자에게 실행 권한을 주겠다
chmod o+x
# 모든 사용자에게 읽기쓰기실행 권한을 모두 주겠다
chmod a+rwx
# 소유자(7), 그룹(5), 기타 사용자(5)
# chmod rwxr-xr-x
chmod 755
```

---

## 쉘  

### 쉘의 기능과 종류  

* 명령어 해석기 기능  

* 프로그래밍 기능  

* 사용자 환경 설정 기능  

```shell
# vi에서 파일 최하단 갈때 
shift + g
# vi 에서 라인넘버 표현
:set nu
# /usr/bin에서 yum이 들어가는 모든 것을 >(리다이렉트) yum_list.txt에 저장한다
ls -l /usr/bin | grep yum > yum_list.txt
```

### > 리다이렉트  

기존 파일 내용 삭제  

### >> 리다이렉트  

덮어쓰기  

### <

## 환경 변수 설정  

* set  

```shell
$ echo $SOME
$ set | grep SOME
SOME=test
```

* export : 환경변수 설정시 사용하는 명령어  

```shell
$ export SOME=test
$ echo $SOME
# 결과는 다른 shell에서도 사용 가능
```

* alias : 긴 문장을 짧게 표현해주기 위한 명령어  

```shell  
alias li='ls -l --color=auto'
li
```

### ~/.bash_profile  

* 시스템에 로그인 할 때마다 실행됨  

* ~/.bashrc가 있다면 그것을 실행함  

### ~/.bashrc  

* 이미 로그인한 상태에서 새 터미널 창을 열때마다 실행  

* alias 같은 것 넣는다

## 실습  

### vagrant(프로비저닝 도구)를 통해 virtualbox에 리눅스 - centos7 설치  

* 계정(vagrant), 비밀번호(vagrant)  

* vagrant로 설치한 vm 접근은 password가 아닌 public key를 통해서 접근 가능  

* node1(2vcpus, 2048Mb)  

  * 포트포워딩  

    * guest: 8080, host: 18080  

    * guest: 80, host: 18000  

    * guest: 3306, host: 13306  

* node2(2vcpus, 2048Mb)  

  * 포트포워딩  

    * guest: 8080, host: 28080  

    * guest: 80, host: 28000  

    * guest: 3306, host: 23306  

* node3(2vcpus, 2048Mb)  

  * 포트포워딩  

    * guest: 8080, host: 38080  

    * guest: 80, host: 38000  

    * guest: 3306, host: 33306  

```shell
# 프로비저닝 시작 -> vagrant 클라우드에서 centos7 box를 다운받아 virtualbox에 설치
vagrant up
# 프로비저닝 결과 확인
vagrant status
# ssh를 통해 vm 접속
vagrant ssh node1
vagrant ssh node2
vagrant ssh node3
# node끼리 접속하기 위해 호스트ip와 호스트 이름 추가
su vi /etc/hosts
192.168.56.11 node1
192.168.56.12 node2
192.168.56.13 node3
#Xshell에서 공개키 기반 접속하면서 각 프라이빗 키의 이름을 구분하기 위해 키 이름 변경
mv k8s/.vagrant/machines/node1/virtualbox/private_key -> k8s/.vagrant/machines/node1/virtualbox/node1_private_key
mv k8s/.vagrant/machines/node2/virtualbox/private_key -> k8s/.vagrant/machines/node2/virtualbox/node2_private_key
mv k8s/.vagrant/machines/node3/virtualbox/private_key -> k8s/.vagrant/machines/node3/virtualbox/node3_private_key
```  
