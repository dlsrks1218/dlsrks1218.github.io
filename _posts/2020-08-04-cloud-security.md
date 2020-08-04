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
|Deployment|레플리카 셋의 리비전을 관리|
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
## Service Type 변경
kubectl edit service nginx-test # (ClusterIp -> NodePort)
## 확인 (port는 service에서 forwarding 된 port 사용)
http://192.168.56.10:30039/ # (<- port forwarding)
http://192.168.56.11:30039/ # (<- port forwarding)
```  

* 자주 사용하는 명령어  

```shell
# ~에 해당하는 파일(설정)을 적용시키겠다
kubectl apply -f ~
# k8s 클러스터에 참여한 노드 확인
kubectl get nodes
# k8s 클러스터가 기동중인 파드들을 모두 확인
kubectl get pods --all-namespaces
```  

### 이슈  

* 쿠버네티스에서 설치나 서비스 시작은 시간이 좀 걸림  

* 초기화 작업 중 오류 발생시 kubeadm reset하고 다시 수행하기  

## AWS  
