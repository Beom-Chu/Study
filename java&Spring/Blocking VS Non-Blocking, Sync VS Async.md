# Blocking VS Non-Blocking, Sync VS Async

### Blocking/Non-Blocking

* **Blocking** : 직접 제어할 수 없는 대상의 작업이 끝날 때까지 기다려야 하는 경우
* **Non-Blocking** : 직접 제어할 수 없는 대상의 작업이 완료되기 전에 제어권을 넘겨주는 경우

---

### Synchronous/Asynchronous

* **Synchronous** : 호출된 함수의 리턴하는 시간과 결과를 반환하는 시간이 일치하는 경우

* **Asynchronous** : 호출된 함수의 리턴하는 시간과 결과를 반환하는 시간이 일치하지 않는 경우

---

|                  | **Blocking**          | **Non-Blocking**          |
| ---------------- | --------------------- | ------------------------- |
| **Synchronous**  | Synchronous Blocking  | Synchronous Non-Blocking  |
| **Asynchronous** | Asynchronous Blocking | Asynchronous Non-Blocking |



### Synchronous Blocking  

![img](https://media.vlpt.us/post-images/codemcd/8c51a790-4a1d-11ea-a77e-bff20a601eb8/synchronous-blocking-IO.png)

- Synchronous :  메서드(애플리케이션)가 리턴하는 시간과 커널에서 결과를 가져오는 시간이 일치한다.
- Blocking : 커널의 작업이 완료될 때까지 대기한다.



### Synchronous Non-Blocking

![img](https://media.vlpt.us/post-images/codemcd/982dd930-4a1d-11ea-a77e-bff20a601eb8/Synchronous-non-blocking-IO.png)

- Synchronous : 메서드(애플리케이션)가 리턴하는 시간과 커널에서 결과를 가져오는 시간이 일치한다.
- Non-Blocking : 애플리케이션으로부터 요청을 받은 커널은 작업 완료 여부와 상관없이 바로 반환하여 제어권을 애플리케이션에게 넘겨준다. 커널의 작업이 완료되면 작업 결과를 애플리케이션에게 반환한다.



### Asynchronous Non-Blocking

![img](https://media.vlpt.us/post-images/codemcd/9dd91ca0-4a1d-11ea-a77e-bff20a601eb8/Asynchronous-non-blocking-IO.png)

- Asynchronous:  메서드(애플리케이션)가 리턴하는 시간과 커널에서 결과를 가져오는 시간이 일치하지 않는다.
- Non-Blocking: 애플리케이션으로부터 요청을 받은 커널은 작업 완료 여부와 상관없이 바로 반환하여 제어권을 애플리케이션에게 넘겨준다. 작업이 끝나면 애플리케이션에게 시그널 또는 콜백을 보낸다.



### Asynchronous Blocking

비효율적인 조합이라 직접적으로 사용하는 경우는 없다. 하지만 Asynchronous Non-Blocking 모델 중에서 Blocking 으로 동작하는 작업이 있는 경우 의도와 다르게 Asynchronous Blocking으로 동작할 때가 있다고 한다.


<br>
<br>
<br>
<br>

* 참고

https://velog.io/@codemcd/Sync-VS-Async-Blocking-VS-Non-Blocking-sak6d01fhx

http://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
