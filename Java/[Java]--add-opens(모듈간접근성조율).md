# [Java]--add-opens(모듈간접근성조율)

업무 중 아래와 같은 오류를 만났습니다.

```
j.l.r.InaccessibleObjectException: Unable to make field private java.lang.String java.util.TimeZone.ID accessible: module java.base does not \"opens java.util\" to unnamed module @11dc3715
    j.l.r.AccessibleObject.checkCanSetAccessib le(AccessibleObject.java:354)
    j.l.r.AccessibleObject.checkCanSetAccessible(AccessibleObject.java:297)
    j.lang.reflect.Field.checkCanSetAccessible(Field.java:178)
    j.lang.reflect.Fie ld.setAccessible(Field.java:172)
    c.g.g.i.r.ReflectionHelper.makeAccessible(ReflectionHelper.java:20)
   ... 51 common frames omitted
 Wrapped by: c.g.g.JsonIOException: Failed making field 'java.util.TimeZone#ID' accessible; either change its visibility or write a custom TypeAdapter for its declaring type
    c.g.g.i.r.ReflectionHelper.makeAccessible(ReflectionHelper.java:23)
    c .g.g.i.b.ReflectiveTypeAdapterFactory.getBoundFields(ReflectiveTypeAdapterFactory.java:203)
    c.g.g.i.b.ReflectiveTypeAdapterFactory.create(ReflectiveTypeAdapterFactory.java:112)
    com.goog le.gson.Gson.getAdapter(Gson.java:531)
	 ...
```

이 문제는 java.util.TimeZone 클래스의 ID 필드에 접근 권한이 없기 때문에 발생하는 것으로 보입니다. 
Java 9 이후의 모듈 시스템에서는 모듈 간의 접근성이 더 엄격하게 제어됩니다.

해결 방법으로는 다음과 같은 옵션이 있을 수 있습니다:

* 해당 필드에 대한 접근성 변경: java.util.TimeZone.ID 필드에 대한 접근성을 변경해야 할 수 있습니다. 그러나 모듈 시스템의 제약 사항을 고려해야 합니다.
* 사용자 지정 TypeAdapter 작성: Gson이 해당 필드에 접근하지 못하는 경우, Gson이 사용하는 객체에 대한 사용자 지정 TypeAdapter를 작성하여 Gson이 필드에 액세스할 수 있도록 할 수 있습니다.

위 방법 중 모듈간 접근성 조율을 통해서 문제를 해결했습니다.
그때 사용된 옵션이 `--add-opens`입니다.

</br>
### --add-opens

`--add-opens` 옵션은 Java 9 이후의 모듈 시스템에서 모듈 간의 접근성을 조정하는 데 사용됩니다. 
특히, 지정된 패키지를 특정 모듈에 대해 열어서(opens) 모듈 간의 접근성을 허용하게 만듭니다.

옵션의 구문은 다음과 같습니다:

```bash
--add-opens <모듈>/<패키지>=<모듈>(,<모듈>/<패키지>=<모듈>,...)
```

여기서 <모듈>/<패키지>는 열려 있는 패키지의 경로이고, <모듈>은 해당 패키지를 열기로 허용하는 모듈입니다.

`--add-opens=java.base/java.util=ALL-UNNAMED` 옵션은 java.base 모듈에서 java.util 패키지를 ALL-UNNAMED 모듈에 대해 열게 만듭니다. 
`ALL-UNNAMED`은 모든 모듈에 대한 접근성을 의미합니다.

이것은 특정한 모듈에 대해 특정 패키지를 열기로 하여, 해당 패키지에 있는 클래스나 멤버에 대한 접근을 허용하게 하는 것입니다. 
이런 방식으로 모듈 시스템의 엄격한 접근 규칙을 우회할 수 있습니다.

그러나 이 옵션을 사용하는 것은 주의가 필요합니다. 모듈 간의 접근성을 무시하거나 우회하는 것은 시스템의 일관성을 해칠 수 있습니다.
 가능한 한 모듈 시스템의 규칙을 준수하는 것이 좋습니다. 이 옵션을 사용하는 것은 마지막 수단으로 고려해야 합니다.