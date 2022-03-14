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
  schedule: "*/5 * * * *"
  concurrencyPolicy: Forbid	# 동시성 정책
  startingDeadlineSeconds : 200 # 시작 기한
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
# │ │ │ │ │                           특정 시스템에서는 7도 일요일)
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



#### concurrencyPolicy 옵션

* Allow : 중복실행 가능
* Forbid : 중복실행 불가 (실행 상태인 잡이 있을 경우 다음 잡 실행 시간이 되도 잡을 실행하지 않음)
* Replace : 새로운 잡을 실행할 시간이고 이전 잡 실행이 아직 완료되지 않은 경우, 크론 잡은 현재 실행 중인 잡 실행을 새로운 잡 실행으로 대체



#### startingDeadlineSeconds 옵션

어떤 이유로 스케줄된 시간을 놓친 경우 잡의 시작 기한을 초 단위로 설정.

기한이 지나면 크론 잡이 잡을 실행하지 않음.

이러한 방식으로 기한을 맞추지 못한 잡은 실패한 잡으로 간주.

이 필드를 지정하지 않으면 잡에 기한 없음.

