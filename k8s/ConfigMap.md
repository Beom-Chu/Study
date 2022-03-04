# ConfigMap

환경 변수나 설정값 들을 변수로 관리하는 기능

### ConfigMap 생성

* 명령어 사용

```sh
kubectl create configmap hello-cm --from-literal=language=java
```

* yaml파일 사용

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true    
```

```sh
kubectl apply -f game-demo.yml
```

<br>

### ConfigMap을 활용한 파드 구성

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo-pod
spec:
  containers:
    - name: demo
      image: alpine
      command: ["sleep", "3600"]
      env:
        # 환경 변수 정의
        - name: PLAYER_INITIAL_LIVES # 환경변수명
          valueFrom:
            configMapKeyRef:
              name: game-demo           # ConfigMap의 name
              key: player_initial_lives # 가져올 키.
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
    # 파드 레벨에서 볼륨을 설정한 다음, 해당 파드 내의 컨테이너에 마운트한다.
    - name: config
      configMap:
        name: game-demo  # 마운트하려는 컨피그맵의 이름
        items:  # 컨피그맵에서 파일로 생성할 키 배열
        - key: "game.properties"
          path: "game.properties"
        - key: "user-interface.properties"
          path: "user-interface.properties"
```

