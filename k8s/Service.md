# Service

Pod는 자체 IP를 가지고 다른 Pod와 통신 할 수 있지만 쉽게 사라지고 생성되는 특징 때문에 직접 통신 방법은 권장하지 않음.

쿠버네티스는 Pod와 직접 통신하는 방법 대신, 별도의 고정된 IP를 가진 Service를 만들고 그를 통해 Pod에 접근.

![](./images/Service구조.png)

### Service(ClusterIP) 생성

ClusterIP는 클러스터 내부에 새로운 IP를 할당하고 여러 개의 Pod를 바라보는 로드밸런서 기능 제공.

그리고 서비스 이름을 내부 도메인 서버에 등록해 Pod간에 서비스 이름으로 통신 가능.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: counter
      tier: db
  template:
    metadata:
      labels:
        app: counter
        tier: db
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
              protocol: TCP

---
apiVersion: v1
kind: Service
metadata:
  name: redis
spec:
  ports:
    - port: 6379
      protocol: TCP
  selector:
    app: counter
    tier: db
```

> 구분자
>
> 하나의 YAML파일에 여러 개의 리소스를 정의할 땐 "---"를 구분자로 사용

```sh
# 생성
kubectl apply -f counter-redis-svc.yml

# Pod, ReplicaSet, Deployment, Service 상태 확인
kubectl get all
```

```sh
NAME                         READY   STATUS    RESTARTS   AGE
pod/redis-57d787df44-mf5w5   1/1     Running   0          10s

NAME                 TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)    AGE
service/kubernetes   ClusterIP   10.96.0.1       <none>        443/TCP    3d19h
service/redis        ClusterIP   10.103.50.102   <none>        6379/TCP   10s

NAME                    READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/redis   1/1     1            1           10s

NAME                               DESIRED   CURRENT   READY   AGE
replicaset.apps/redis-57d787df44   1         1         1       10s
```

