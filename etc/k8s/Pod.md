# Pod

쿠버네티스에서 관리하는 가장 작은 배포 단위.

도커는 컨테이너를 만들지만 쿠버네티스는 Pod를 생성.

Pod는 한개 또는 여러 개의 컨테이너를 포함.



* Pod 생성

```bash
# echo 라는 이름의 pod 생성
kubectl run echo --image ghcr.io/subicur/echo:v1
```

* Pod 목록 조회

```bash
kubectl get pod
```

```bash
NAME   READY   STATUS              RESTARTS   AGE
echo   0/1     ContainerCreating   0          10s
```

`STATUS` 는 정상적으로 생성되면 `Running`상태로 바뀜.

* 단일 Pod 상세 조회

```bash
kubectl describe pod/echo
```

```bash
Name:         echo
Namespace:    default
Priority:     0
Node:         minikube/192.168.64.5

...(생략)...

Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  68s   default-scheduler  Successfully assigned default/echo to minikube
  Normal  Pulling    68s   kubelet            Pulling image "ghcr.io/subicura/echo:v1"
  Normal  Pulled     35s   kubelet            Successfully pulled image "ghcr.io/subicura/echo:v1" in 33.176019499s
  Normal  Created    35s   kubelet            Created container echo
  Normal  Started    35s   kubelet            Started container echo
```

쿠버네티스를 운영하면서 가장 많이 확인하는 부분은 `Events`

* Pod 제거

```bash
kubectl delete pod/echo
```

