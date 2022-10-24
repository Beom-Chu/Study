# 네트워크 TEST Pod(netshoot)



traceroute, tcpdump, ping, curl, ip 등 사용 가능

### POD 생성

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: network-test-pod
spec:
  containers:
  - name: network-test-pod
    image: nicolaka/netshoot
    command: ["tail"]
    args: ["-f", "/dev/null"]
```

### 접속

`kubectl exec -it network-test-pod -- zsh`



