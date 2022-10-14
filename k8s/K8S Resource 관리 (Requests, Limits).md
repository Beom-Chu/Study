# K8S Resource 관리 (Requests, Limits)

### Resource Request와 Limit이란?

* Resource는 k8s에서 컨테이너에 할당되는 CPU와 Memory 자원량을 의미.

* 단위

  * CPU : Milicore 단위(1000Milicore = 1Core)  
    *  예) 0.1 = 100m = 100 milicore

  * Memory : Mbyte 단위

* Request는 k8s의 Pod **스케쥴링 기준**.
  * 컨테이너 별로 설정이 가능하며 실제 Pod의 Request는 Pod의 모든 컨테이너 Request의 합.
  * Node가 요청되는 Pod Request를 수용 할 자원을 가지고 있으면 Pod를 생성하고, 그렇지 않은 경우 다음 Node가 동일한 작업을 수행한다.
  * 모든 Node가 Pod Request를 수용 할 수 없으면 Pod는 Pending 상태로 남는다.
* Limit은 Pod가 사용하는 **자원을 제한**.
  * Pod가 Limit 이상의 자원을 사용하면 비정상 동작하거나 종료될 수 있다.
  * **Limit은 Request보다 반드시 같거나 커야한다**.

##### resources 사용 예

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: simple-pod
spec:
  containers:
  - name: simple-pod-ctr
    image: busybox
    stdin: true
    tty: true
    resources:
      requests:
        cpu: 0.5 # 0.5 = 500m
        memory: 300Mi
      limits:
        cpu: 1
        memory: 500Mi
```

### Request를 초과 사용하면?

Pod가 Request를 초과해도 Limit을 넘지 않으면 문제 없다.

다만 Node의 자원이 부족할 경우에는 Request를 초과한 Pod는 퇴거(Eviction) 대상이 된다.

K8s는 Node 자원 부족 시(예:Out of memory) 초과한 Pod부터 종료(Terminate)를 수행하는데,

이는 자원이 부족한 Node에서 다른 Node로 재배치를 수행하기 위함이다.

만약 모든 Node가 자원 부족이면 해당 Pod는 Pending 상태로 남게 된다.

### Limit을 초과 사용하면?

Pod가 Limit을 초과하게 되면 overcommitted 상태가 된다.

이 상태에서 CPU는 throttling을 통해 압축 조정된다.

Memory 사용량을 초과하게 되면 Pod가 이상 동작을 보이거나 종료된다.

### Request와 Limit 산정은 어떻게?

정답은 없지만 한 블로거의 말에 따르자면.

Request는 평균 사용량+10% ,

Limit은 피크 사용량 중 최대 사용량을 기준으로 설정 권장

### Request와 Limit 설정을 하지 않으면?

Request와 Limit을 설정하지 않으면 Node의 자원 전체를 사용 할 수 있다는 의미.

운영시 Request량이 설정되어 있지 않으면 Node 자원을 기준으로 정확한 스케줄링을 수행하지 못한다.

즉, Node에 Podr가 초과 배치 될 수 있고, Node 자원 부족으로 퇴거 대상이 많아지는 경우가 발생한다.

<br><br><br><br>

reference : https://itchain.wordpress.com/2018/05/16/kubernetes-resource-request-limit/