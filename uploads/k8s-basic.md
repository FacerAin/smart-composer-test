생성한 컨테이너가 실행되도록 요청하기


1. 생성한 컨테이너가 실행되도록 요청하기
CLI로 실행하기
kubectl create deploy webserver --image=nginx
kubectl 명령어로 Nginx 컨테이너를 실행하는 Deployment 생성
YAML 파일로 실행하기
1. YAML 파일 얻는 방법
kubectl create deploy webserver --image=nginx --dry-run=client -o yaml > nginx.yaml
--dry-run=client: 리소스를 실제로 생성하지 않고, 클라이언트에서 YAML 파일만 생성함.
-o yaml: YAML 형식으로 출력함.
nginx.yaml: 결과를 nginx.yaml 파일로 저장함.

2. Yaml 파일 확인
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webserver
  labels:
    app: webserver
spec:
  replicas: 2
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      labels:
        app: webserver
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
replicas: 파드의 개수
containers 섹션에서 Nginx 이미지를 사용
3. YAML 파일 적용하기
kubectl apply -f nginx.yaml
작성한 YAML 파일을 사용해 Deployment를 생성함.
Kubectl 명령 전달 과정


Kubectl 명령어를 사용하면 Control Plane으로 전달됨.
Control Plane의 REST API Server가 명령어를 받음.
Pod 생성 및 스케줄링


Scheduler: 노드 자원 상태를 확인하고 가장 적합한 노드를 선택해 파드 생성 요청을 전달
Kubelet 역할


Scheduler가 적합한 노드를 선택하면, 해당 노드의 Kubelet에 파드 생성 요청을 보냄.
Kubelet이 Docker Daemon에게 컨테이너 생성 요청을 전달함.
Docker Daemon이 Docker Hub에서 이미지를 다운로드하고 컨테이너를 생성함.
생성된 컨테이너는 POD 단위로 관리됨.
Kubernetes 배포 및 실행 프로세스 요약
kubectl 명령으로 Control Plane에 요청 보냄.
Control Plane의 REST API Server가 명령을 처리함.
Scheduler가 노드 상태를 평가해 파드 생성 위치를 결정함.
Kubelet이 Docker Daemon에게 요청해 컨테이너를 생성함.
생성된 컨테이너는 POD 단위로 관리됨.
쿠버네티스 컴포넌트


마스터 컴포넌트 (Control Plane)
컴포넌트	설명
etcd	- Key-Value 타입의 저장소
- 쿠버네티스 마스터 및 노드들의 다양한 상태 정보 저장
kube-apiserver	K8s API를 사용하도록 요청을 받고 요청이 유효한지 검사
kube-scheduler	파드를 실행할 노드 선택
kube-controller-manager	파드를 관찰하며 개수를 보장
워커 노드
컴포넌트	설명
kubelet	- 모든 노드에서 실행되는 K8s 에이전트
- 데몬 형태로 동작
kube-proxy	- K8s의 네트워크 동작을 관리
- iptables 규칙을 구성
컨테이너 런타임	- 컨테이너를 실행하는 엔진
- Docker, Containerd, Runc 등 지원
애드온
분류	종류
네트워크 애드온	CNI - Weave, Calico, Flannel
DNS 애드온	CoreDNS
대시보드 애드온	Kubernetes 웹 UI 대시보드
컨테이너 자원 모니터링	cAdvisor
클러스터 로깅	- 컨테이너 로그 및 K8s 운영 로그를 중앙화
- ELK (ElasticSearch, Logstash, Kibana)
- EFK (ElasticSearch, Fluentd, Kibana)
- DataDog
주요 kubectl 명령어들
명령어	설명
apply	원하는 상태를 적용합니다. 보통 -f 옵션으로 파일과 함께 사용합니다.
get	리소스 목록을 보여줍니다.
describe	리소스의 상태를 자세하게 보여줍니다.
delete	리소스를 제거합니다.
logs	컨테이너의 로그를 봅니다.
exec	컨테이너에 명령어를 전달합니다. 컨테이너에 접근할 때 주로 사용합니다.
config	kubectl 설정을 관리합니다.
create	새로운 리소스를 생성합니다.
scale	리소스의 개수를 조정합니다. 주로 replica 설정을 변경할 때 사용합니다.
edit	리소스를 편집합니다.
patch	리소스에 변경 사항을 적용합니다.
rollout	배포와 관련된 작업을 수행합니다. (예: rollout status, rollout undo)
apply --prune	적용 후 존재하지 않는 리소스를 삭제합니다.
top	클러스터의 리소스 사용량을 보여줍니다. (CPU, 메모리 등)
version	kubectl 및 클러스터 버전을 확인합니다.

