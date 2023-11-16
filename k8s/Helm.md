# Helm

helm이란 쿠버네티스 패키지 매니저입니다. 
centOS에서 yum 이나 python에서는 pip 툴 MAC OS에서는 brew와 같이 패키지를 관리 주는 툴입니다. 
helm을 이용하면 원하는 패키지들을 쿠버네티스에 쉽게 설치할 수 있습니다.

helm은 docker hub와 비슷하게 helm 패키지들을 저장하고 있는 저장소(repository)가 있습니다. 
사용자는 저장소를 추가하고 해당 저장소의 패키지를 install하기만 하면 됩니다. 
helm 차트로 원하는 패키지를 install할때 `values.yaml` 을 이용하여 사용자의 환경에 따라 커스텀하여 사용할 수 있습니다.

### Helm의 기본 구조

helm의 기본구조는 아래와 같이 helm 명령어로 생성할 수 있습니다.
```sh
helm create hello_chart
```
생성한 helm chart 구조는 다음과 같습니다.
* Chart.yaml : 차트의 이름, 버전등 기본적인 메타 정보를 정의 합니다.
* templates : kubernets에 배포될 리소스들의 manifest 탬플릿들을 포함 합니다.
* charts : dependency가 있는 chart 파일들이 해당 디렉토리에 생기게 됩니다.
* values.yaml : 템플릿에 사용될 변수(value) 값들을 정의합니다.
```
.
├── Chart.yaml
├── charts
├── templates
│   ├── NOTES.txt
│   ├── _helpers.tpl
│   ├── deployment.yaml
│   ├── hpa.yaml
│   ├── ingress.yaml
│   ├── service.yaml
│   ├── serviceaccount.yaml
│   └── tests
│       └── test-connection.yaml
└── values.yaml
```

생성된 `deployment.yaml` 파일을 보면 `{{ . }}` 와 같은 문법을 볼 수 있는데 이는 helm chart가 go template을 따라 만들었기 때문에 template변수들도 go lang의 자료형을 따라가고 있습니다.

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "hello_chart.fullname" . }}
  labels:
    {{- include "hello_chart.labels" . | nindent 4 }}
spec:
  {{- if not .Values.autoscaling.enabled }}
  replicas: {{ .Values.replicaCount }}
  {{- end }}
  selector:
    matchLabels:
      {{- include "hello_chart.selectorLabels" . | nindent 6 }}
  template:
    metadata:
      {{- with .Values.podAnnotations }}
      annotations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      labels:
        {{- include "hello_chart.selectorLabels" . | nindent 8 }}
    spec:
      {{- with .Values.imagePullSecrets }}
      imagePullSecrets:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      serviceAccountName: {{ include "hello_chart.serviceAccountName" . }}
      securityContext:
        {{- toYaml .Values.podSecurityContext | nindent 8 }}
      containers:
        - name: {{ .Chart.Name }}
          securityContext:
            {{- toYaml .Values.securityContext | nindent 12 }}
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - name: http
              containerPort: 80
              protocol: TCP
          livenessProbe:
            httpGet:
              path: /
              port: http
          readinessProbe:
            httpGet:
              path: /
              port: http
          resources:
            {{- toYaml .Values.resources | nindent 12 }}
      {{- with .Values.nodeSelector }}
      nodeSelector:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.affinity }}
      affinity:
        {{- toYaml . | nindent 8 }}
      {{- end }}
      {{- with .Values.tolerations }}
      tolerations:
        {{- toYaml . | nindent 8 }}
      {{- end }}
```


### Helm 명령어


##### helm search

```sh
# 내 로컬에 저장되어있는 helm repsitory 검색
helm search repo  

# wordpress가 있는 hub 검색
helm search hub wordpress

# helm search는 퍼지 문자열 매칭 알고리즘을 사용하므로, 단어 또는 문구의 일부분만 입력해도 된다.
helm search repo kash
```

##### helm install
```sh
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install my-release bitnami/nginx
```

##### helm ls
```sh
helm ls
NAME          NAMESPACE    REVISION    UPDATED                                 STATUS      CHART           APP VERSION
my-release    default      1           2022-07-02 01:18:03.455447 +0900 KST    deployed    nginx-13.0.0    1.23.0
```

##### helm uninstall
```sh
helm delete my-release
```

##### helm template
```sh
helm template bitnami/nginx
```

##### helm pull
```sh
helm pull bitnami/nginx

### pull 받고 template 랜더링
helm template . -f values.yaml
```

##### helm upgrade
```sh
### helm upgrade [RELEASE] [CHART] [flags]
helm upgrade -f myvalues.yaml redis ./redis
```

</br>
Helm명령어 : https://helm.sh/ko/docs/helm/helm/