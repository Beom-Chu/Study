# 메서드 시그니처의 throws Exception

메서드 시그니처에 `thorws Exception`을 붙이면 아래와 같은 이점이 있습니다.

* 예외 처리
  * 해당 메서드 내에서 발생하는 예외를 메서드를 호출한 곳으로 던질 수 있다는 것을 나타냅니다. 따라서, 해당 메서드를 호출하는 코드에서는 예외를 처리해야 합니다. 이는 예외를 명시적으로 처리하거나 상위 호출자로 전파하여 처리하는 방식으로 이루어집니다.
* 예외 전파 
  * `throws Exception`을 선언한 메서드를 호출하는 곳에서는 예외를 처리해야 합니다. 이는 해당 메서드를 호출한 상위 메서드에서도 같은 방식으로 예외를 처리해야 합니다. 이러한 예외의 전파는 메서드 호출 계층 구조 상에서 예외 처리를 위임할 수 있도록 합니다.
* 코드 가독성
  * `throws Exception`을 사용하면 메서드 시그니처에서 해당 메서드가 예외를 던질 수 있다는 정보를 명확히 전달할 수 있습니다. 이는 코드를 읽는 사람에게 메서드 동작에 대한 더 나은 이해를 제공합니다.

`throws Exception`을 선언하는 것은 예외 처리와 코드 가독성을 향상시키는 데 도움이 됩니다. 

그러나 모든 메서드에 `throws Exception`을 붙일 필요는 없으며, 실제로 예외를 처리해야 하는 경우에만 사용해야 합니다. 

예외를 처리할 수 있는 적절한 방법을 선택하여 예외를 처리하도록 권장합니다.