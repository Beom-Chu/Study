# Kubectl_기본명령어

| 명령어   | 설명                                                     |
| -------- | -------------------------------------------------------- |
| apply    | 원하는 상태를 적용. 보통 `-f` 옵션으로 파일과 함께 사용  |
| get      | 리소스 목록 보기                                         |
| describe | 리소스 상태 상세 보기                                    |
| delete   | 리소스 제거                                              |
| logs     | 컨테이너의 로그 보기                                     |
| exec     | 컨테이너에 명령어를 전달. 컨테이너에 접근 할때 주로 사용 |
| config   | kubectl 설정 관리                                        |



### 상태 설정하기

> kubectl apply -f [파일명 또는 URL]

```bash
# 다시 한번 워드프레스 배포하기 (URL로!)
kubectl apply -f https://subicura.com/k8s/code/guide/index/wordpress-k8s.yml
```



### 리소스 보기

> kubectl get [TYPE]

```bash
# Pod 조회
kubectl get pod

# 줄임말(Shortname)과 복수형 사용가능
kubectl get pods
kubectl get po

# 여러 TYPE 입력
kubectl get pod,service
#
kubectl get po,svc

# Pod, ReplicaSet, Deployment, Service, Job 조회 => all
kubectl get all

# 결과 포멧 변경
kubectl get pod -o wide
kubectl get pod -o yaml
kubectl get pod -o json

# Label 조회
kubectl get pod --show-labels
```



### 리소스 상세 상태보기

> kubectl describe [TYPE]/[NAME] 또는 [TYPE] [NAME]

```bash
# Pod 조회로 이름 검색
kubectl get pod

# 조회한 이름으로 상세 확인
kubectl describe pod/wordpress-5f59577d4d-8t2dg # 환경마다 이름이 다릅니다
```



### 리소스 제거

> kubectl delete [TYPE]/[NAME] 또는 [TYPE] [NAME]

```bash
# Pod 조회로 이름 검색
kubectl get pod

# 조회한 Pod 제거
kubectl delete pod/wordpress-5f59577d4d-8t2dg
```



### 컨테이너 로그 조회

> kubectl logs [POD_NAME]

```bash
# Pod 조회로 이름 검색
kubectl get pod

# 조회한 Pod 로그조회
kubectl logs wordpress-5f59577d4d-8t2dg

# 실시간 로그 보기
kubectl logs -f wordpress-5f59577d4d-8t2dg
```



### 컨테이너 명령어 전달

> kubectl exec [-it] [POD_NAME] -- [COMMAND]

쉘로 접속하여 컨테이너 상태를 확인하는 경우 `-it` 옵션 사용.

여러개의 컨테이너가 있는 경우 `-c` 옵션으로 컨테이너 지정.

```bash
# Pod 조회로 이름 검색
kubectl get pod

# 조회한 Pod의 컨테이너에 접속
kubectl exec -it wordpress-5f59577d4d-8t2dg -- bash
```



### 설정 관리

```bash
# 현재 컨텍스트 확인
kubectl config current-context

# 컨텍스트 설정
kubectl config use-context minikube
```