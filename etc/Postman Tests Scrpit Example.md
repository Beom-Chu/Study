# Postman Tests Scrpit Example

### 변수

#### 환경변수 설정

```javascript
pm.environment.set("key","value") 
```

#### 중첩된 객체를 환경 변수로 설정하기

```javascript
var array = [1, 2, 3, 4];
pm.environment.set("array", JSON.stringify(array, null, 2));
  
var obj = {a: [1, 2, 3, 4], b: { c: val'} };
pm.environment.set("obj", JSON.stringify(obj)); 
```

#### 환경변수 얻기

```javascript
pm.environment.get("key"); 
```

#### 값이 문자열화된 객체인 경우 환경 변수 얻기

```javascript
var array = JSON.parse(pm.environment.get("array"));
var obj = JSON.parse(pm.environment.get("obj")); 
```

#### 환경 변수 삭제

```javascript
pm.environment.unset("key"); 
```

#### 전역 변수 설정

```javascript
pm.globals.set("key", "value");
```

#### 전역 변수 얻기

```javascript
pm.globals.get("key"); 
```

#### 전역 변수 삭제

```javascript
pm.globals.unset("key"); 
```

#### 변수 가져오기

```javascript
pm.variables.get("key");
```

<br>

## 검증

#### response body에 문자열 포함 여부

```javascript
pm.test("Body matches string", function(){
    pm.expect(pm.response.text()).to.include("string you want to search");
}); 
```

*  test의 첫 번째 매개변수로 들어가는 문자열은, 어떤 것을 테스트하기 위함인지 명시하기 위한 것임으로 간결하고 명확하게 쓰는 것을 권장

* 테스트의 결과 시트에서 이 문자열과 pass, fail 여부만 출력되기 때문에 어떤 것에 대한 테스트가 성공했는지 실패했는지 알 수 있도록 기재하는 것이 중요


#### response body가 문자열과 같은가

```javascript
pm.test("Body is correct", function(){
    pm.response.to.have.body("response body");
});
```

#### JSON 값 확인

```javascript
pm.test("json value test", function(){
    var jsonData = pm.response.json();
    pm.expect(jsonData.value).to.eql(100);
}); 
```

#### content-type이 헤더에 있는가

```javascript
pm.test("Content-Type is present", function(){
    pm.response.to.have.header("Content-Type");
});
```

#### 응답 시간이 200ms 미만인가

```javascript
pm.test("Response time is less than 200ms",function(){
    pm.expect(pm.response.responseTime).to.be.below(200);
}); 
```

#### status code는 200(ok)

```javascript
pm.test("Status code is 200 - OK",function(){
    pm.response.to.have.status(200);
});
```

#### status code 이름이 문자열이며 일치하는가

```javascript
pm.test("Status code name has String",function(){
    pm.response.to.have.status("Created");
});
```

#### POST 요청이 성공했는가

```javascript
pm.test("Successful POST request",function(){
    pm.expectI(pm.response.code).to.be.oneOf([201,202]);
});
```

<br>

## 응용

#### JSON 데이터에 Tiny Validator 사용하기

```javascript
var schema = {
    "items": {
                "type":"boolean"
             }
};
var data1 = [true, false];
var data2 = [true, 123];
  
pm.test("Schema is valid", function(){
    pm.expect(tv4.validate(data1, schema)).to.be.true;
    pm.expect(tv4.validate(data2, schema)).to.be.true;
});
```

Tiny Validator는 JSON Schema 검증기로, 자세한 내용은 [Tiny Validator](https://github.com/geraintluff/tv4)에서 확인하실 수 있습니다. 

####  base64로 인코딩된 데이터 디코드

```javascript
var intermediate,
    base64Content,  //인코딩 데이터가 있다고 가정
    rawContent = base64Content.slice('data:application/octet-stream;base64,'.length);
  
intermediate = CryptoJS.enc.Base64.parse(base64content);
  
pm.test('Contents are valid', function(){
    pm.expect(CryptoJS.enc.Utf8.stringify(intermediate)).to.be.true;
}); 
```

#### 비동기 요청 보내기

```javascript
pm.sendRequest('https://postman-echo.com/get', function(){
    console.log(response.json());
}); 
```

#### XML 본문을 JSON 객체로 변환

```javascript
var jsonObject = xml2Json(responseBody); 
```

<br>

<br>

<br>

<br>

출처: https://kellis.tistory.com/23 [Flying Whale]