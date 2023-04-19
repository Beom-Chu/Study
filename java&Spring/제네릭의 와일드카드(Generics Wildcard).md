# 제네릭 타입의 와일드카드(Generics Wildcard)

Java에서 제네릭 타입의 와일드카드(`Wildcard`)는 제네릭 타입을 사용할 때, 타입 파라미터를 더 유연하게 처리할 수 있는 방법 중 하나입니다. 

와일드카드는 타입 인자로 사용되며, `?` 기호로 표시됩니다.

- `<?>`: 제한 없음
- `<? extends T>`: T 또는 T의 하위 클래스
- `<? super T>`: T 또는 T의 상위 클래스

### 장점

1. 다형성 : 제네릭 타입을 사용하면, 다양한 타입을 처리할 수 있지만 제한된 타입만을 처리할 수 있습니다. 반면 와일드카드를 사용하면, 제한없이 다양한 타입을 처리할 수 있습니다. 이는 다형성을 높여줍니다.
2. 코드의 재사용성 : 와일드카드를 사용하면, 코드의 재사용성을 높일 수 있습니다. 와일드카드를 사용한 메소드나 클래스는 여러 타입에서 사용할 수 있으므로, 코드의 재사용성을 높여줍니다.
3. 타입 안전성을 보장 : 와일드카드를 사용하면, 타입 안전성을 보장할 수 있습니다. 예를 들어, `List<?> list`는 어떤 타입의 `List`든 받을 수 있지만, `List<Object>` list는 받을 수 없습니다. 와일드카드를 사용하면, 컴파일러는 타입 안정성을 체크하며, 안전한 코드를 작성할 수 있도록 도와줍니다.

와일드카드를 사용하는 것은 제네릭 타입을 더욱 유연하게 사용할 수 있도록 도와줍니다. 하지만 와일드카드를 사용할 때는, 코드의 가독성이나 복잡성이 높아질 수 있으므로, 신중하게 사용해야 합니다.

### 사용 샘플 코드

```java
import java.util.*;

public class WildcardExample {
    
    public static void printList(List<?> list) {
        for (Object elem : list)
            System.out.print(elem + " ");
        System.out.println();
    }

    public static void main(String[] args) {
        List<Integer> intList = Arrays.asList(1, 2, 3);
        List<String> strList = Arrays.asList("one", "two", "three");
        List<Object> objList = Arrays.asList("four", 5, 6.0);

        System.out.print("Integer List: ");
        printList(intList);

        System.out.print("String List: ");
        printList(strList);

        System.out.print("Object List: ");
        printList(objList);
    }
}
```

`printList` 메서드는 매개변수에 `<?>` 와일드 카드를 사용해서 어떤 타입의 `List`든지 받아서 처리 가능하도록 했습니다.