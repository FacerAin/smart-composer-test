# Kubernetes 컨테이너 실행 및 관리 가이드
#Kubernetes #컨테이너 #Deployment #kubectl #YAML

**Summary**: 이 문서는 Kubernetes에서 Nginx 컨테이너를 실행하고 관리하는 방법을 설명합니다. CLI와 YAML 파일을 사용하여 Deployment를 생성하고, Kubernetes의 주요 컴포넌트와 명령어를 소개합니다.

## 컨테이너 실행 요청하기

### CLI로 실행하기
- `kubectl create deploy webserver --image=nginx`: 이 명령어는 Nginx 컨테이너를 실행하는 Deployment를 생성합니다.

### YAML 파일로 실행하기
1. **YAML 파일 생성**:
   - `kubectl create deploy webserver --image=nginx --dry-run=client -o yaml > nginx.yaml`
   - `--dry-run=client`: 실제 리소스를 생성하지 않고 YAML 파일만 생성합니다.
   - `-o yaml`: YAML 형식으로 출력합니다.
   - `nginx.yaml`: 결과를 이 파일에 저장합니다.

2. **YAML 파일 내용 확인**:
   ```yaml
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
   ```
   - `replicas`: 파드의 개수
   - `containers`: Nginx 이미지를 사용

3. **YAML 파일 적용하기**:
   - `kubectl apply -f nginx.yaml`: 작성한 YAML 파일을 사용해 Deployment를 생성합니다.

## Kubernetes 배포 및 실행 프로세스

1. `kubectl` 명령어는 Control Plane으로 전달됩니다.
2. Control Plane의 REST API Server가 명령어를 처리합니다.
3. Scheduler가 노드 상태를 평가해 파드 생성 위치를 결정합니다.
4. Kubelet이 Docker Daemon에게 요청해 컨테이너를 생성합니다.
5. 생성된 컨테이너는 POD 단위로 관리됩니다.

## Kubernetes 컴포넌트

### 마스터 컴포넌트 (Control Plane)
- **etcd**: Key-Value 저장소로, 다양한 상태 정보를 저장합니다.
- **kube-apiserver**: K8s API 요청을 받고 유효성을 검사합니다.
- **kube-scheduler**: 파드를 실행할 노드를 선택합니다.
- **kube-controller-manager**: 파드를 관찰하며 개수를 보장합니다.

### 워커 노드
- **kubelet**: 모든 노드에서 실행되는 K8s 에이전트로, 데몬 형태로 동작합니다.
- **kube-proxy**: 네트워크 동작을 관리하고 iptables 규칙을 구성합니다.
- **컨테이너 런타임**: Docker, Containerd, Runc 등을 지원합니다.

### 애드온
- **네트워크 애드온**: CNI - Weave, Calico, Flannel
- **DNS 애드온**: CoreDNS
- **대시보드 애드온**: Kubernetes 웹 UI 대시보드
- **컨테이너 자원 모니터링**: cAdvisor
- **클러스터 로깅**: ELK, EFK, DataDog

## 주요 kubectl 명령어들

- `apply`: 원하는 상태를 적용합니다.
- `get`: 리소스 목록을 보여줍니다.
- `describe`: 리소스의 상태를 자세히 보여줍니다.
- `delete`: 리소스를 제거합니다.
- `logs`: 컨테이너의 로그를 봅니다.
- `exec`: 컨테이너에 명령어를 전달합니다.
- `config`: kubectl 설정을 관리합니다.
- `create`: 새로운 리소스를 생성합니다.
- `scale`: 리소스의 개수를 조정합니다.
- `edit`: 리소스를 편집합니다.
- `patch`: 리소스에 변경 사항을 적용합니다.
- `rollout`: 배포와 관련된 작업을 수행합니다.
- `apply --prune`: 적용 후 존재하지 않는 리소스를 삭제합니다.
- `top`: 클러스터의 리소스 사용량을 보여줍니다.
- `version`: kubectl 및 클러스터 버전을 확인합니다.

## Key Points
- Kubernetes에서 Nginx 컨테이너를 실행하는 방법
- CLI와 YAML 파일을 사용한 Deployment 생성
- Kubernetes의 주요 컴포넌트와 역할
- kubectl 명령어의 다양한 기능

## Follow-up Questions / Discussion Points
1. YAML 파일을 사용하여 Deployment를 생성하는 장점은 무엇인가요?
2. Kubernetes의 Control Plane과 워커 노드의 역할 차이는 무엇인가요?
3. `kubectl` 명령어 중 가장 자주 사용하는 것은 무엇이며, 그 이유는 무엇인가요?
4. Kubernetes에서 애드온의 역할은 무엇인가요?
5. Kubernetes에서 파드의 수를 조정하는 방법은 무엇인가요?