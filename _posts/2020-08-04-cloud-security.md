---
layout: post
title: "클라우드 보안 융합 전문가 과정"
subtitle: "클라우드 AWS 1일차"
date: 2020-08-04 20:10:11 -0400
background: '/img/posts/03.jpg'
---

## 시험 리뷰  

* 시행 일자 : 2020년 08월 03일

* 시험 범위 : 클라우드(2), 파이썬(8), 리눅스 및 도커(8), 데이터베이스(8)  

* 클라우드 제공자이자 소비자 -> 프라이빗 클라우드(하이브리드 클라우드가 아님)  

* 딕셔너리는 정렬 기능이 없다  

* ZeroDivisionError는 0으로 나눴을 때 발생  

* 도커파일 RUN은 도커파일 안의 환경변수를 설정하는 것임  

* 데베에서 **실수가 많다 -> 꼼꼼히 읽고 문제를 풀자**  

* WHERE NOT DCODE <=> NULL 이라는 문법이 존재한다  

## 쿠버네티스  

### 정의  

도커 컨테이너 운영을 자동화하기 위한 오케스트레이션 툴, 도커만 관리하는 것은 아님(다른 컨테이너도 관리)  

### 특징  

* 컨테이너 배포 및 배치 전략  

* Scale In / Out  

* Service Discovery  

* 기타 운용  

* 도커 스웜 클러스터보다 충실한 기능을 갖춘 컨테이너 오케스트레이션 시스템  

* 멀티 호스트 갯수가 많지 않으면 쿠버네티스는 오버스펙이므로 스웜 사용이 나은 상황도 존재  

* 싱글 호스트에서 사용(MacOS, Windows)에서 사용하려면 Docker for Desktop에서 사용 가능  

### 새로운 용어

|Resource of Object|용도|
|---|---|
|Node|컨테이너가 배치되는 서버|
|Namespace|쿠버네티스 클러스터 안의 가상 클러스터|
|Pod|쿠버네티스에서 관리되는 최소 단위, 컨테이너의 실행 방법 정의(파드는 컨테이너 용어를 포함한다)|
|Replica Set|같은 스펙을 갖는 파드를 여러 개 생성하고 관리하는 역할|
|Deployment|레플리카 셋의 리비전(수정 및 변경사항)을 관리|
|Service|파드의 집합에 접근하기 위한 경로를 정의|
|Ingress|파드에 대한 포트포워딩을 마친 뒤 외부에서 컨테이너 안으로 들어올 수 있게 쿠버네티스 클러스터를 외부로 노출하는 것|
|ConfigMap|설정 정보를 정의하고 파드에 전달|
|Persistent Volume|파드가 사용할 스토리지 크기 및 종류를 정의|
|Persistent Volume Claim|퍼시스턴트 볼륨을 동적으로 확보|
|Dashboard|쿠버네티스에 배포된 컨테이너 등에 대한 정보를 보여주는 관리 도구|  

### k8s 데모  

* 리눅스 상에 도커 +  k8s 설치 필요  

* 컨테이너를 Pod로 감싸기  

* apiVersion : pod 버전  

* metadata : 파드 이름, 라벨 이름(태그 정보, name:value, 자유롭게 지정 가능)  

* spec : 파드 구성 정보  

### k8s 설치  

* manager(mysql:5.7 이미지, node:slim 이미지 설치된 상태), 나머지 worker들은 깨끗한 상태  

* kubeadm, kubectl, kubelet(엔진)  

* CentOS에 쿠버네티스 설치(도커가 이미 설치 되어있어야 함)  

```shell
# 모든 시스템 디렉토리의 값 읽기
sysctl --system
# 쿠버네티스 리포지토리 설정
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
# 쿠버네티스 설치
yum install -y --disableexcludes=kubernetes kubeadm-1.15.5-0.x86_64 kubectl-1.15.5-0.x86_64 kubelet-1.15.5-0.x86_64
# kubectl 버전 확인
kubectl version #Minor:15는 옛 버전이며, 대시보드가 지원안하는 최신버전이 있어 15버전 사용
Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.5", GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", GitTreeState:"clean", BuildDate:"2019-10-15T19:16:51Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
# 1. 쿠버네티스 설정 - Master
## 실행 kubelet(엔진) 실행, 네트워크 등록(도커 스웜에서 swarm init과 같은 과정)
systemctl enable --now kubelet
## 초기화
kubeadm init --pod-network-cidr=10.96.0.0/16 --apiserver-advertise-address=192.168.56.14 ###<- 설치 성공 후 아래 커맨드 부분을 복사 (생성되는 값은 본인의 환경에 따라 다름)>
kubeadm join 192.168.56.14:6443 --token 1xzibw.dlrft2gc7n8lshm8 \
    --discovery-token-ca-cert-hash sha256:bbbcc3fdaf9f9bb681ef89d96e2c30e266e4060009a841de748fba498a88a057
## 환경변수 설정
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
kubectl get pods --all-namespaces # 모든 pods가 Running 상태인지 확인 
## Calico 기본 설치(Kubernetes Cluster Networking plugin)
kubectl apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml
kubectl get pods --all-namespaces
# 위 칼리코 기본 설치가 안될때 수행하는 명령어임!!
## Calico는 기본적으로 192.68.0.0/16 대역 사용하기 때문에, IP가 중복 될 경우에는 위의 방법 말고(kubectl apply) calico.yaml 파일을 다운로드 후 코드 수정, Calico 설치
curl -O https://docs.projectcalico.org/v3.8/manifests/calico.yaml  
sed s/192.168.0.0\\/16/10.96.0.0\\/12/g -i calico.yaml
kubectl apply -f calico.yaml

# 2. 쿠버네티스 노드 연결 - Node
## 연결(Master의 init 작업에서 복사 한 커맨드를 사용)
kubeadm join 192.168.56.14:6443 --token 1xzibw.dlrft2gc7n8lshm8 \
    --discovery-token-ca-cert-hash sha256:bbbcc3fdaf9f9bb681ef89d96e2c30e266e4060009a841de748fba498a88a057
## 연결 시 오류 발생하면 kubeadm reset 명령어로 초기화 후 다시 실행 (Node 모두 초기화)
kubeadm reset

# 3. 마스터에서 확인 및 대시보드 설치 - Master
## 확인
kubectl get nodes
## 대시보드 설치
kubectl apply -f https://raw.githubusercontent.com/kubetm/kubetm.github.io/master/sample/practice/appendix/gcp-kubernetes-dashboard.yaml
## Proxy 설정(vagrant로 프로비저닝할 때 8000을 포트포워딩해서 사용할 뿐임 다른 포트해도 됨)
nohup kubectl proxy --port=8000 --address=192.168.56.14 --accept-hosts='^*$' >/dev/null 2>&1 &
## 접속
http://192.168.56.14:8000/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/
## 인증 -> skip

# 4. 테스트
## Pod 실행
kubectl run nginx-test --image=nginx --port 80 --generator=run-pod/v1
## Service 실행
kubectl expose pod nginx-test
kubectl get services
## Pod 상세정보 확인
kubectl describe pod nginx-test
## Service Type 변경
kubectl edit service nginx-test # (ClusterIp -> NodePort)
Spec의 type: NodePort
kubectl edit service nginx-test # 다시 확인해 포워딩된 포트 사용
## 확인 (port는 service에서 forwarding 된 port 사용)
http://192.168.56.10:30039/ # (<- port forwarding)
http://192.168.56.11:30039/ # (<- port forwarding)
```  

* 대시보드에서 파드 > 편집 눌러보면 파드를 정의해놓은 yaml 파일을 확인할 수 있음  

* 자주 사용하는 명령어  

```shell
# ~에 해당하는 파일(설정)을 적용시키겠다
kubectl apply -f ~
# k8s 클러스터에 참여한 노드 확인
kubectl get nodes
# k8s 클러스터가 기동중인 파드들을 모두 확인
kubectl get pods --all-namespaces
# kube-system이라는 namespace안의 pod에 대해 조사 수행(crash와 같은 오류의 이유 찾기)
kubectl describe pod <pod 이름> --namespace=kube-system
```  

### 이슈  

* 쿠버네티스에서 설치나 서비스 시작은 시간이 좀 걸림  

* 초기화 작업 중 오류 발생시 kubeadm reset하고 다시 수행하기  

* 시스템이 강제 종료되어 다시 쿠버네티스 클러스터 접속 시 모든 노드가 not ready 상태가 되므로 **systemctl restart kubelet**로 해결  

* 시스템이 강제 종료되어 k8s 대시보드도 문제 발생(외부 접속 안됨)  

  ```shell
    # 확인
    kubectl get svc kubernetes-dashboard -n kube-system  
    # 워커 노드들에 모두 k8s 엔진 재시작
    systemctl restart kubelet
    # 아래 코드 다시 수행  
    nohup kubectl proxy --port=8000 --address=192.168.56.14 --accept-hosts='^*$' >/dev/null 2>&1 &
    kubectl get svc kubernetes-dashboard -n kube-system  
  ```

### 실습 - 쿠버네티스에서 nodejs 배포 20200805  

```shell
# manager
$ mkdir work && cd work
$ vi hello.js
var http = require('http');
var content = function(req, resp) {
  resp.end("Hello, k8s!" + "\n");
  resp.writeHead(200);
}
var w = http.createServer(content);
w.listen(8000);

$ vi Dockerfile
FROM node:slim
EXPOSE 8000
COPY hello.js .
CMD node hello.js

# test - docker run -d -p 9000:8000 dlsrks1218/hello
docker build -t dlsrks1218/hello .
docker push dlsrks1218/hello

# Dashboard로 이동해서 파드 생성하거나 터미널로 yml 파일을 통해 파드 생성
<+ 생성> 클릭 -> yml 파일 생성
$ vi hello-pod.yml
apiVersion: v1
kind: Pod
metadata:
  name: hello-pod
  labels:
    app: hello
spec:
  containers:
  - name: hello-container
    image: dlsrks1218/hello
    ports:
    - containerPort: 8000

kubectl apply -f hello-pod.yml
kubectl get pods

$ vi hello-service.yml
apiVersion: v1
kind: Service
metadata:
  name: hello-svc
  labels:
    app: hello
spec:
  selector:
    app: hello
  ports:
    - port: 9000
    - targetPort: 8000
  type: NodePort

$ v1 pod-1.yml
apiVersion: v1
kind: Pod
metadata:
  name: pod-1
spec:
  containers:
  - name: container1
    image: kubetm/p8000
    ports:
    - containerPort: 8000
  - name: container2
    image: kubetm/p8080
    ports:
    - containerPort: 8080

$ kubectl apply -f xxx.yml
$ kubectl get pods

curl -X GET http://localhost:8000
curl -X GET http://localhost:8080
```

* 대시보드에서 pod 1~6 생성, service 1~2 생성  

```shell
$ vi pod-1.yml # pod-1~6까지 생성

apiVersion: v1
kind: Pod
metadata:
  name: pod-1
  labels:
    type: web
    lo: dev
spec:
  containers:
  - name: container
    image: kubetm/init

apiVersion: v1
kind: Pod
metadata:
  name: pod-2
  labels:
    type: db
    lo: dev
spec:
  containers:
  - name: container
    image: kubetm/init

apiVersion: v1
kind: Pod
metadata:
  name: pod-3
  labels:
    type: server
    lo: dev
spec:
  containers:
  - name: container
    image: kubetm/init
  
$ vi svc-1.yml # svc-1~2까지 생성

apiVersion: v1
kind: Service
metadata:
  name: svc-1
spec:
  selector:
    type: web
  ports:
    - port: 80

$ vi svc-2.yml
apiVersion: v1
kind: Service
metadata:
  name: svc-2
spec:
  selector:
    type: db
  ports:
    - port: 3306
```

### 이슈 20200805  

* 파드 생성 먼저하고 서비스를 생성하여 포트 포워딩 해주어야 서비스에 정의된 포트포워딩이나 여타의 설정들이 적용이 됨  

* **pod 생성하고 서비스를 생성하면 ClusterIP가 자동으로 부어되고 NodePort를 지정해주지 않으면 임의의 노드포트로 포트포워딩이 된다. 그 결과 클라이언트가 특정 노드의 파드에 위에서 지정된 노드포트 번호를 통해 파드 안의 컨테이너로 보내준다. 혹은 별도로 포트포워딩 해준 <포트>가 있다면 클러스터ip:<포트>로 접근가능하다.**  

  ```shell
  kubectl get services hello-svc
  NAME        TYPE       CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
  hello-svc   NodePort   10.96.54.119   <none>        9000:30165/TCP   7m36s

  $ curl -X GET http://10.96.54.119:30165
  Hello, k8s!
  $ curl -X GET http://10.96.54.119:9000
  Hello, k8s!
  ```

  * 위에 보이는 PORT(S)에서 9000은 Service가 생성되면서 자동으로 할당된 클러스터 ip의 9000번을 호출하면 hello-svc가 적용되는 Pod의 8000번 컨테이너로 보내줌  

  * 30165는 hello-svc가 자동으로 생성한 NodePort 번호로 해당 노드의 NodePort번호(30165)로 접근하면 Pod내 컨테이너의 8000번 포트로 보내줌  

  * NodeIP:<NodePort에 의해 생성된 임의의 포트>  

  * Service의 ClusterIP:<직접 포워딩해준 포트>  

    * kubectl get services hello-svc -> Cluster IP를 알아내기  

  * 이 두가지의 방법으로 접근 가능하다  

  * 출처 : <https://bcho.tistory.com/1262>  

* **위 같은 상황에서 외부로 노출하기 위해서는 LoadBalancer를 활용하거나 ExternalEndpoint를 지정해주어야 함**  

## AWS  

### 01 운영 서버와 AWS 소개  

* 운영 서버 : 개발이나 테스트 목적이 아닌 실제 사용자들을 대상으로 서비스하는 서버  

  * 트래픽 대응도 해야하고 빠른 응답 속도와 높은 가용성을 보장해야 함  

* 운영 서버 관리는 환경 구성, 코드 배포, 모니터링 세 단계로 나뉨  

  * 환경 구성 : 서비스할 코드를 구동시킬 수 있는 서버 인프라를 구축하는 것  

  * 코드 배포 : 구성한 환경에 최신 버전의 코드를 빠르고 안전하게 배포하는 것  

  * 안정적인 서비스 운영을 위해 서버와 코드에 어떤 이상이 없는지 바로 파악하고 대응할 수 있게 도와주는 것  

* 운영 서버 아키텍처  

  * 단일 서버  

  * 앱 / DB 서버 분리(3-tier 개발)  

  * 서버 단위의 로드 밸런서  

  * 서버 내 앱 단위의 로드 밸런서  

* EC2(Elastic Compute Cloud) : 원하는 만큼 사용하고 쓴만큼 비용을 지불하는 온디맨드 형식의 가상 서버  

  * AMI(Amazon Machine Image) : EC2 인스턴스의 기반이 되는 이미지  

  * 보안 그룹(Security Group) : 보안을 위해 IP와 포트 번호를 이용해 정의해두는 서버 접속 규칙  

  * 키 페어(Key Pair) : 서버에 접속하기 위한 열쇠, 공개 키 암호화 기법으로 서버에는 공개키(Public Key)를 두고 사용자는 개인키(Private Key)를 들고 접속  

### 06 배포 자동화  

* AWS IAM(Identity and Access Management) : 사용자 권한 제어 서비스  

* 용어 : 권한, 정책, 사용자, 그룹, 역할  

### 실습  

* 루트 계정으로 접속  

* IAM / 액세스 관리 / 그룹 생성  

* 정책 연결(AmazonEC2FullAccess)  

* 액세스 관리 / 사용자 생성 -> 액세스 유형 모두 체크, 콘솔 비밀번호는 지정 방식이며 고정  

* 태그 : 키 = class, 값 = security  

* ARN(리소스 아이디 값) 모든 리소스는 아이디 값을 부여받음  

* IAM 사용자는 루트 계정 아래에 존재하며 루트 계정의 이메일만 존재, 별도의 로그인 주소를 부여받아 링크를 통해 접속하게 되어있음  

* <https://github.com/joneconsulting/cloudcomputing/blob/master/aws/aws_account.md>에서 본인 IAM 계정과 링크 확인, (edu17_us_east_1/skinfosec1!)  

* 인스턴스 생성 규칙 : 같은 지역 사용자들끼리는 인스턴스 만들면 함께 보이기 때문에 instance 이름 지을 때 본인을 구분할 수 있게 자신의 아이디를 앞에 붙일것(prefix)  

* 인스턴스 / 인스턴스 시작

  * 단계 1 : AMI 선택 -> Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type 선택  

  * 단계 2 : 인스턴스 유형 선택 -> t2.micro(프리티어 사용 가능) 기본 값 선택 -> 다음 : 인스턴스 세부 정보 구성  

  * 단계 3 : 인스턴스 세부 정보 구성 -> 변경사항 없음 -> 다음 : 스토리지 추가  

  * 단계 4: 스토리지 추가 -> 변경사항 없음 -> 다음 : 태그 추가  

  * 단계 5: 태그 추가 -> 변경사항 없음 -> 다음 : 보안 그룹 구성  

  * 단계 6: 보안 그룹 구성 -> 소스 / 내 IP 선택 -> 규칙 추가(유형 선택 가능) -> 이름에 launch-wizard-1을 launch-wizard-이름으로 바꾸기(중복되면 안됨) -> Name을 sg-자기이름 으로 지정하기 -> 검토 및 시작  

    * 인바운드 규칙 : 외부에서 인스턴스로 접근할 포트 열어주기  

  * 단계 7: 인스턴스 시작 검토 -> 시작하기 -> 새 키페어 생성 -> 이름 지정하기 -> 키 페어 다운로드  

    * Linux, MacOS는 키 페어 경로로 가서 chmod 400 키페어 해줘야함, 윈도우는 제외  

* 인스턴스 / 본인 인스턴스 선택 후 작업 / 인스턴스 중지  

* 윈도우에서 연결 -> XShell 사용 -> 새 세션 등록 정보 이름(ec2-amazon linux), 호스트(자신의 퍼블릭 ip) -> 연결 / 사용자 키 -> 자신의 pem 파일 선택 후 열기 -> 연결  

* private IP : 프라이빗 네트워크 내에서 서로 통신하기 위해 필요한 IP 주소  

* 서비스 테스트  

```shell
# nodejs 서버 띄우기
$ vi hello.js
var http = require('http');
var content = function(req, res) {
    res.end("Hello Kubernetes!" + "\n");
    res.writeHead(200);
}
var w = http.createServer(content);
w.listen(8000);

# amazon linux에 nodejs 설치
$ curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
$ . ~/.nvm/nvm.sh
$ nvm install node
# hello.js 실행
node hello.js
# 웹에서 본인 ip:8000로 확인해보기
```

### MSA와 서버리스  

* MSA :  

* 서버리스 컴퓨팅 : IT 인프라를 데이터 센터 혹은 클라우드에 별도의 준비 없이, 필요한 기능을 함수 형태로 구현하고, 오토 스케일링 방식으로 시시각각 변하는 자원 수요를 지원하며 전통적인 백엔드를 사용(FaaS, Function as a Service or BaaS, Backend as a Service)  
