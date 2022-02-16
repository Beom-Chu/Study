# ResourceQuota

네임스페이스 당 리소스 총 소비량을 제한.

프로젝트 내에서 리소스들이 소비할 수 있는 컴퓨팅 리소스 총량 뿐만 아니라 유형에 따라 하나의 네임스페이스 내에 생성할 수 있는 개체의 수량을 제한.



### ResourceQuota 생성

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: mem-cpu-demo
spec:
  hard:
    requests:
    	cpu: "1"
    	memory: 1Gi
    limits:
    	cpu: "2"
    	memory: 2Gi
```

```sh
# 리소스쿼터 생성
kubectl apply -f quota-mem-cpu.yaml --namespace=quota-mem-cpu-example

# 리소스쿼터 확인
kubectl get resourcequota
```

| 정의            | 설명                             |
| --------------- | -------------------------------- |
| requests.cpu    | 모든 컨테이너의 총 cpu 요청량    |
| requests.memory | 모든 컨테이너의 총 메모리 요청량 |
| limits.cpu      | 모든 컨테이너의 총 cpu 상한      |
| limits.memory   | 모든 컨테이너의 총 메모리  상한  |



##### 메모리 요청량의 총 합계가 메모리 요청량 쿼터를 초과할 경우

```sh
Error from server (Forbidden): error when creating "examples/admin/resource/quota-mem-cpu-pod-2.yaml":
pods "quota-mem-cpu-demo-2" is forbidden: exceeded quota: mem-cpu-demo,
requested: requests.memory=700Mi,used: requests.memory=600Mi, limited: requests.memory=1Gi
```

