# HostAliases로 파드의 /etc/hosts 항목 추가

* 파드의 /etc/hosts 파일에 항목을 추가하면 파드 수준의 호스트 네임 해석을 제공.

* Pod Spec의 `hostAliases` 항목 사용으로 사용자 정의 항목 추가 가능.
* `hostAliases`를 사용하지 않고 직접 호스트 파일을 수정하는 경우 pod 재시작하면 사라질 수 있음.

### 파드 호스트 파일 확인

##### 파드 목록 조회

```sh
kubectl get pods --output=wide
```

```sh
NAME     READY     STATUS    RESTARTS   AGE    IP           NODE
nginx    1/1       Running   0          13s    10.200.0.4   worker0
```

##### 파드 컨테이너 접속 및 호스트 파일 보기

```sh
kubectl exec nginx -- cat /etc/hosts
```

```sh
# Kubernetes-managed hosts file.
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
fe00::0	ip6-mcastprefix
fe00::1	ip6-allnodes
fe00::2	ip6-allrouters
10.200.0.4	nginx
```

*위와 같이 기본 정보만 존재*

### hostAliases 사용

##### pod yaml [hostAliases-pod.yml]

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: hostaliases-pod
spec:
  restartPolicy: Never
  hostAliases:
  - ip: "127.0.0.1"
    hostnames:
    - "foo.local"
    - "bar.local"
  - ip: "10.1.2.3"
    hostnames:
    - "foo.remote"
    - "bar.remote"
  containers:
  - name: cat-hosts
    image: busybox
    command:
    - cat
    args:
    - "/etc/hosts"
```

##### ymal로 pod 실행

```sh
kubectl apply -f hostAliases-pod.yml
```

##### 내용이 추가된 호스트 파일

```sh
# Kubernetes-managed hosts file.
127.0.0.1	localhost
::1	localhost ip6-localhost ip6-loopback
fe00::0	ip6-localnet
fe00::0	ip6-mcastprefix
fe00::1	ip6-allnodes
fe00::2	ip6-allrouters
10.200.0.5	hostaliases-pod

# Entries added by HostAliases.
127.0.0.1	foo.local	bar.local
10.1.2.3	foo.remote	bar.remote
```

<br>

> 컨테이너 내부의 호스트 파일을 수동으로 변경 금지.
>
> 호스트 파일을 수동으로 변경하면, 컨테이너가 종료되면 변경 사항이 손실.
