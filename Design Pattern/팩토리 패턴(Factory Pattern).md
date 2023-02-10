# 팩토리 패턴(Factory Pattern)

생성 패턴 중의 하나입니다.

인스턴스를 만드는 절차를 추상화하는 패턴입니다.

팩토리 패턴은 객체를 생성하는 인터페이스는 미리 정의하되, 인스턴스를 만들 클래스의 결정은 서브 클래스 쪽에서 내리는 패턴 입니다.

**여러 개의 서브 클래스를 가진 슈퍼 클래스(인터페이스)가 있을 때 인풋에 따라 하나의 자식 클래스의 인스턴스를 리턴해주는 방식** 입니다.

이 패턴은 인스턴스화에 대한 책임을 객체를 사용하는 클라이언트에서 팩토리 클래스로 가져 옵니다.

### 활용성

1. 어떤 클래스가 자신이 생성해야 하는 객체의 클래스를 예측 할 수 없을 때
2. 생성할 객체를 기술하는 책임을 자신의 서브 클래스가 지정하길 원할 때

### 장점

1. 클라이언트 코드로부터 서브 클래스의 인스턴스화를 제거하여 서로 간의 종속성을 낮추고, 결합도를 느슨하게 하며 확장을 쉽게 할 수 있습니다.
2. 팩토리 패턴은 클라이언트와 구현 객체들 사이에 추상화를 제공 합니다.

### 샘플 코드

```java
public interface Animal {
    void speak();
    AnimalType getAnimalType();
}
```

```java
public enum AnimalType {
    CAT, DOG
}
```

```java
public class Cat implements Animal {
    @Override
    public void speak() {
        System.out.println("야옹!!");
    }

    @Override
    public AnimalType getAnimalType() {
        return AnimalType.CAT;
    }
}
```

```java
public class Dog implements Animal {
    @Override
    public void speak() {
        System.out.println("멍멍!!");
    }

    @Override
    public AnimalType getAnimalType() {
        return AnimalType.DOG;
    }
}
```

```java
public class AnimalFactory {

    public Animal createAnimal(AnimalType animalType) throws Exception {
        if(Objects.equals(animalType, AnimalType.CAT)) {
            return new Cat();
        } else if(Objects.equals(animalType, AnimalType.DOG)) {
            return new Dog();
        } else {
            throw new Exception("No AnimalType!!");
        }
    }
}
```

