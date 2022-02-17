# CronJob

CronJob은 반복 일정에 따라 Job을 생성.

CronJob은 백업, 리포트 생성 등의 정기적 작업을 수행하기 위해 사용.

각 Job은 무기한 반복되도록 구성해야함.

*모든 크론잡 일정: 시간은 kube-controller-manager의 시간대가 기준.*



```yaml
# CronJob yaml 파일 샘플
apiVersion: batch/v1
kind: CronJob
metadata:
  name: hello
spec:
  schedule: "*/1 * * * *"
  jobTemplate:
    spec:
      template:
        spec:
          containers:
          - name: hello
            image: busybox
            imagePullPolicy: IfNotPresent
            command:
            - /bin/sh
            - -c
            - date; echo Hello from the Kubernetes cluster
          restartPolicy: OnFailure
```



#### 크론 스케줄 문법

```yaml
# ┌───────────── 분 (0 - 59)
# │ ┌───────────── 시 (0 - 23)
# │ │ ┌───────────── 일 (1 - 31)
# │ │ │ ┌───────────── 월 (1 - 12)
# │ │ │ │ ┌───────────── 요일 (0 - 6) (일요일부터 토요일까지;
# │ │ │ │ │                                   특정 시스템에서는 7도 일요일)
# │ │ │ │ │
# │ │ │ │ │
# * * * * *
```

| 설명                     | 상응표현    |
| :----------------------- | ----------- |
| 매년 1월 1일 자정에 실행 | 0 0 1 1 *   |
| 매월 1일 자정에 실행     | 0 0 1 * *   |
| 매주 일요일 자정에 실행  | 0 0 * * 0   |
| 매일 자정에 실행         | 0 0 * * *   |
| 매시 0분에 실행          | 0 * * * *   |
| 매시 5분 단위로 실행     | */5 * * * * |

**CronJob을 적용하면 Scheduling 된 주기마다 한번씩 Pod를 생성하고 Job을 실행하고 Complete하는 과정을 반복**
