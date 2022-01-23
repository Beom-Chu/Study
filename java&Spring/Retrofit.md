# Retrofit

Retrofit은 HTTP API를 자바 인터페이스 형태로 사용할 수 있습니다.

```java
public interface GitHubService {
  @GET("/users/{user}/repos")
  Call<List<Repo>> listRepos(@Path("user") String user);
}
```

`Retrofit` 클래스로 `GitHubService` 인터페이스를 구현하여 생성 합니다.

```java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com")
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```

각각의 `Call`은 `GitHubService`를 통해 동기 또는 비동기 HTTP 요청을 원격 웹서버로 보낼 수 있습니다.

```java
Call<List<Repo>> repos = service.listRepos("octocat");
```

HTTP 요청은 어노테이션을 사용하여 명시합니다.

- URL 파라미터 치환과 쿼리 파라미터가 지원됩니다.
- 객체를 요청 body로 변환합니다. (예: JSON, protocol buffers)
- Multipart 요청 body와 파일 업로드가 가능합니다.

<br>

## API 정의

#### 요청 메소드

모든 메소드들은 반드시 상대 URL과 요청 메소드를 명시하는 어노테이션을 가지고 있어야 합니다.

기본 제공 요청 메소드 어노테이션 : `HTTP`, `GET`, `POST`, `PUT`, `PATCH`, `DELETE`, `OPTIONS` ,`HEAD`

```java
@GET("/users/list")
```

URL에 쿼리 매개변소를 지정 할 수도 있습니다.

```java
@GET("users/list?sort=desc")
```

#### URL 다루기

요청 URL은 동적으로 부분 치환 가능하며, 이는 메소드 매개변수로 변경이 가능합니다. 

부분 치환은 영문/숫자로 이루어진 문자열을 `{`와`}`로 감싸 정의 해 줍니다.

반드시 이에 대응하는 `@Path`를 메소드 매개변수에 명시 해 줘야합니다.

```java
@GET("/group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId);
```

쿼리 매개변수도 명시 가능합니다.

```java
@GET("/group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @Query("sort") String sort);
```

복잡한 쿼리 매개변수 조합의 경우 `Map`을 사용할 수 있습니다.

```java
@GET("group/{id}/users")
Call<List<User>> groupList(@Path("id") int groupId, @QueryMap Map<String, String> options);
```

#### REQUEST BODY

HTTP Request Body에 객체를 `@Body` 어노테이션을 통해 명시가 가능합니다.

```java
@POST("/users/new")
Call<User> createUser(@Body User user);
```

이러한 객체들은 `Retrofit` 인스턴스에 추가된 컨버터에 따라 변환됩니다. 

만약 해당 타입에 맞는 컨버터가 추가 되어있지 않다면, `RequestBody`만 사용 가능 합니다.

#### FORM-ENCODED와 MULTIPART

`@FormUrlEncoded` 어노테이션을 메소드에 명시하면 form-encoded 데이터로 전송 됩니다. 

각 key-value pair의 key는 어노테이션 값에, value는 객체를 지시하는 `@Field` 어노테이션으로 매개변수에 명시하시면 됩니다.

```java
@FormUrlEncoded
@POST("/user/edit")
Call<User> updateUser(@Field("first_name") String first, @Field("last_name") String last);
```

Multipart 요청은 `@Multipart` 어노테이션을 메소드에 명시하면 됩니다.

각 파트들은 `@Part` 어노테이션으로 명시합니다.

```java
@Multipart
@PUT("/user/photo")
Call<User> updateUser(@Part("photo") RequestBody photo, @Part("description") RequestBody description);
```

Multipart의 part는 `Retrofit` 의 컨버터나, `RequestBody` 를 통하여 직렬화(serialization) 가능한 객체를 사용하실 수 있습니다.

#### HEADER 다루기

정적 헤더들은 `@Headers` 어노테이션을 통해 명시할 수 있습니다.

```java
@Headers("Cache-Control: max-age=640000")
@GET("/widget/list")
Call<List<Widget>> widgetList();
```

```java
@Headers({
    "Accept: application/vnd.github.v3.full+json",
    "User-Agent: Retrofit-Sample-App"
})
@GET("/users/{username}")
Call<User> getUser(@Path("username") String username);
```

헤더는 서로 덮어쓰지 않습니다. 동일한 이름을 가진 모든 헤더가 요청에 포함됩니다.

동적인 헤더는 `@Header` 어노테이션을 통해 명시 가능합니다. 반드시 이에 대응하는 `@Header` 어노테이션을 매개변수에 명시해야 합니다. 만약 값이 `null`이라면 헤더가 생략됩니다. 아니면, 매개변수 객체의 `toString` 메소드를 호출하여 반환된 값을 헤더의 값으로 추가합니다.

```java
@GET("/user")
Call<User> getUser(@Header("Authorization") String authorization)
```

헤더를 모든 요청마다 추가하셔야 한다면 [OkHttp interceptor](https://github.com/square/okhttp/wiki/Interceptors) 를 사용

#### 동기 VS. 비동기

`Call` 인스턴스는 동기 혹은 비동기로 요청 실행이 가능합니다. 각 인스턴스들은 동기 혹은 비동기중 한가지 방식만 사용가능합니다, 하지만 `clone()` 메소드를 통해 새 인스턴스를 생성하시면 이전과 다른 방식을 사용 가능합니다.

안드로이드에서의 콜백들은 메인 스레드에서 실행됩니다. JVM에서는, HTTP 요청을 호출한 스레드와 동일한 스레드에서 콜백들이 실행됩니다.

```java
//동기 실행 sample
Call<List<Repo>> repos = service.listRepos("octocat");
Response<List<Repo>> response = repos.execute();

System.out.println("response.isSuccessful() = " + response.isSuccessful());
for (Repo repo : response.body()) {
    System.out.println("repo = " + repo);
}
```

```java
//비동기 실행 sample
Call<List<Repo>> repos = service.listRepos("octocat");
repos.enqueue(new Callback<List<Repo>>() {
            @Override
            public void onResponse(Call<List<Repo>> call, Response<List<Repo>> response) {
                //수신된 HTTP 응답에 대해 호출
                System.out.println("response.isSuccessful() = " + response.isSuccessful());
                
                for (Repo repo : response.body()) {
                    System.out.println("repo = " + repo);
                }
            }

            @Override
            public void onFailure(Call<List<Repo>> call, Throwable t) {
                //서버와 통신하는 동안 네트워크 예외가 발생하는 경우
                System.out.println("t.getMessage() = " + t.getMessage());
            }
        });
```



<br>

## Retrofit 설정

`Retrofit` 은 API 인터페이스를 호출 가능한 객체로 바꿔줍니다. 기본적으로 Retrofit은 실행중인 플랫폼에 최적화된 설정을 제공하지만, 사용자정의도 가능합니다.

#### CONVERTERS

기본적으로, Retrofit은 HTTP 요청 본문을 OkHttp의 `ResponseBody` 형식과 `@Body` 에 이용하는 `RequestBody` 타입만 역직렬화(deserialization) 할 수 있습니다.

컨버터들은 이외의 형식들을 변환해주는 역할을 합니다. 아래에 많이 사용하고 편리한 자사의 컨버터 6개의 라이브러리들을 사용하실 수 있습니다.

- [Gson](https://github.com/google/gson): `com.squareup.retrofit:converter-gson`
- [Jackson](http://wiki.fasterxml.com/JacksonHome): `com.squareup.retrofit:converter-jackson`
- [Moshi](https://github.com/square/moshi/): `com.squareup.retrofit:converter-moshi`
- [Protobuf](https://developers.google.com/protocol-buffers/): `com.squareup.retrofit:converter-protobuf`
- [Wire](https://github.com/square/wire): `com.squareup.retrofit:converter-wire`
- [Simple XML](http://simple.sourceforge.net/): `com.squareup.retrofit:converter-simplexml`

Gson을 사용하는 `GitHubService` 인터페이스가 Gson을 역직렬화가 가능하게 `GsonConverterFactory` 클래스를 통해 컨버터를 추가하는 예제

```java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl("https://api.github.com")
    .addConverterFactory(GsonConverterFactory.create())
    .build();

GitHubService service = retrofit.create(GitHubService.class);
```



<br><br><br><br>

출처 : https://square.github.io/retrofit/

http://devflow.github.io/retrofit-kr/