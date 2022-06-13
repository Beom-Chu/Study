# Secret

* `ConfigMap`이 일반적인 환경 설정 정보나 CONFIG 정보를 저장하도록 디자인 되었다면, 보안이 중요한 패스워드나, API키, 인증서 파일들은 `Secret` 에 저장 가능.

* Secret은 항상 메모리에 저장 
  * 하나의 Secret Object는 최대 1M까지 지원 가능한데 메모리에 올라가는 특성으로 인해 여러개를 저장하게 되면 Out of Memory 이슈가 발생할 수 있어 주위 필요.

* Secret의 Value는 항상 Base64 포맷
  * secret에 저장되는 내용은 패스워드 같은 단순 문자열의 경우에는 바로 저장이 가능하지만, SSL 인증서와 같은 바이너리 파일의 경우에는 문자열로 저장이 불가능 -> 이러한 바이너라 파일 저장을 지원하기 위해서.

#### Secret.yml 샘플

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: iksoon-secret-test
data:
# data 하위에 전달할 Key-Value를 명시
  K8S_VERSION_TEST: "a3ViZXJuZXRlc192ZXJzaW9uX3Rlc3Qx"  
# Key : K8S_CONFIGMAP_TEST, Value : Kubernetes_configMap_1 을 base64 encofing
  K8S_CONFIGMAP_TEST: "S3ViZXJuZXRlc19jb25maWdNYXBfMQ==" 
# Key :K8S_TEST, Value :iksoon_TEST_1 을 base64 encofing
  K8S_TEST: "aWtzb29uX1RFU1RfMQ=="
```

[Base64 인코딩 참고]: ../etc/Base64%20인코딩%2C디코딩.md



#### Secret 설정 중 일부 사용  deployment.yml 샘플

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iksoon-deployment
  labels:
    app: iksoon-tomcat
spec:
  replicas: 1
  selector:
    matchLabels:
      app: iksoon-pod
  template:
    metadata:
      labels:
        app: iksoon-pod
    spec:
      containers:
      - name: iksoon-tomcat
        image: peksoon/iksoon_tomcat:1.0.4
        ports:
        - containerPort: 8080
        env:                    # env : 항목을 추가해서 secret 설정을 적용
        - name: VERSION_TEST    # - name : 해당 Pod에 적용될 환경변수 이름 
          valueFrom:            # valueFrom : name에서 명시한 환경변수에 올 값을 설정
            secretKeyRef:       # secretKeyRef : 불러올 Secret과 key를 명시
              name: iksoon-secret-test # name: 항목에 secret 파일의 metadata:  name
              key: K8S_VERSION_TEST  # key : 항목에 secret에 설정되어있는 data의 key
        - name: K8S_CONFIGMAP       # - name을 추가해서 다수의 환경변수를 설정할 수 있음
          valueFrom:
            secretKeyRef:
              name: iksoon-secret-test
              key: K8S_CONFIGMAP_TEST
        - name: TEST_IKSOON
          valueFrom:
            secretKeyRef:
              name: iksoon-secret-test
              key: K8S_TEST
```

#### Secret 설정 모두 사용  deployment.yml 샘플

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: iksoon-deployment
  labels:
    app: iksoon-tomcat
spec:
  replicas: 1
  selector:
    matchLabels:
      app: iksoon-pod
  template:
    metadata:
      labels:
        app: iksoon-pod
    spec:
      containers:
      - name: iksoon-tomcat
        image: peksoon/iksoon_tomcat:1.0.4
        ports:
        - containerPort: 8080
        envFrom:                     # envFrom : 항목을 추가해서 secret 설정을 적용
        - secretRef:                 # secret : 불러올 secret을 명시
            name: iksoon-secret-test # name: 항목에 secret 파일의 metadata:  name
```



<br><br>

reference : https://ikcoo.tistory.com/21