# 빌더 패턴(Builder Pattern)

객체 생성에 관한 디자인 패턴 입니다.

일반적으로 생성자를 통해 객체를 생성하는 방법보다 더 복잡한 객체를 생성하는 경우 유용하게 쓰입니다.

빌더 패턴은 객체 생성 과정을 나누어 각 단계별로 처리하는 방법 입니다.

객체 생성에 필요한 속성을 설정하기 위한 빌더 클래스를 만들고, 이 빌더 클래스에서 객체 생성 과정을 처리 합니다.

빌더 클래스는 생성할 객체의 속성을 설정하기 위한 메서드를 제공하며, 이를 통해 객체의 속성을 설정할 수 있습니다. 또한 빌더 클래스에서는 객체 생성 전에 유효성 검사 등의 과정을 수행할 수도 있습니다.

각 단계에서는 객체 생성에 필요한 속성을 설정하고, 최종적으로 build() 메서드를 호출하여 객체를 생성합니다. 이를 통해 객체 생성 과정이 복잡한 경우에도 객체 생성 과정을 단순화 할 수 있습니다.

### 빌더 패턴 구조



![](./images/빌더패턴구조.svg)

* Director
  * 객체 생성을 담당하는 클래스
  * 빌더 객체를 받아서 객체 생성 과정을 처리
  * 빌더 객체에 대한 자세한 내용은 알지 못하고, 빌더 인터페이스만을 사용.
* Builder
  * 객체 생성에 필요한 단계별로 처리하는 인터페이스
  * 이 인터페이스를 구현한 구체적인 빌더 클래스들이 실제로 객체를 생성
* ConcreteBuilder
  * Builder 인터페이스를 구현한 구체적인 빌더 클래스
  * 객체 생성 과정에서 필요한 모든 단계를 처리하며, 최종적으로 객체를 생성
* Product
  * 생성되는 객체를 나타내는 클래스
  * 이 클래스는 빌더 클래스에서 처리된 속성들을 가짐

### 샘플

```java
// Product 클래스
public class Salad {
    private String vegetables;
    private String dressing;

    public Salad(String vegetables, String dressing) {
        this.vegetables = vegetables;
        this.dressing = dressing;
    }

    public String getVegetables() {
        return vegetables;
    }

    public void setVegetables(String vegetables) {
        this.vegetables = vegetables;
    }

    public String getDressing() {
        return dressing;
    }

    public void setDressing(String dressing) {
        this.dressing = dressing;
    }

    @Override
    public String toString() {
        return "Salad [vegetables=" + vegetables + ", dressing=" + dressing + "]";
    }
}

// Builder 인터페이스
public interface SaladBuilder {
    void setVegetables(String vegetables);
    void setDressing(String dressing);
    Salad getSalad();
}

// ConcreteBuilder 클래스
public class Chef implements SaladBuilder {
    private Salad salad;

    public Chef() {
        this.salad = new Salad("", "");
    }

    @Override
    public void setVegetables(String vegetables) {
        salad.setVegetables(vegetables);
    }

    @Override
    public void setDressing(String dressing) {
        salad.setDressing(dressing);
    }

    @Override
    public Salad getSalad() {
        return salad;
    }
}

// Director 클래스
public class SaladMaker {
    private SaladBuilder builder;

    public SaladMaker(SaladBuilder builder) {
        this.builder = builder;
    }

    public Salad makeSalad() {
        builder.setVegetables("Lettuce, tomatoes, onions");
        builder.setDressing("Vinaigrette");
        return builder.getSalad();
    }
}

// 클라이언트 코드
public class Client {
    public static void main(String[] args) {
        SaladBuilder builder = new Chef();
        SaladMaker maker = new SaladMaker(builder);
        Salad salad = maker.makeSalad();
        System.out.println(salad);
    }
}
```

