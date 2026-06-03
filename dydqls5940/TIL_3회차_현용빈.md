# 3회차 TIL

## 오늘 공부한 내용

- 오버라이딩(Overriding)
- 오버로딩(Overloading)
- 다형성(Polymorphism)
- 추상 메서드(Abstract Method)
- 인터페이스(Interface)
- 디폴트 메서드(Default Method)

---

## 오버라이딩 (Overriding)

오버라이딩은 부모 클래스의 메서드를 자식 클래스에서 다시 정의하여 사용하는 것이다.  
즉, 부모의 기능을 자식이 자신의 방식으로 덮어써서 사용한다.

```java
class Animal {
    void sound() {
        System.out.println("동물 소리");
    }
}

class Dog extends Animal {
    @Override
    void sound() {
        System.out.println("멍멍");
    }
}
```

오버라이딩이 없다면 모든 객체가 같은 동작만 수행하게 되므로 다양한 표현이 어려워진다.

---

## 오버로딩 (Overloading)

오버로딩은 같은 이름의 메서드를 여러 버전으로 만드는 것이다.  
매개변수의 타입이나 개수가 달라야 한다.

```java
class Calculator {
    int add(int a, int b) {
        return a + b;
    }

    double add(double a, double b) {
        return a + b;
    }
}
```

같은 이름의 메서드를 상황에 따라 다르게 사용할 수 있다.

---

## 다형성 (Polymorphism)

다형성은 같은 메서드를 호출하더라도 객체에 따라 다른 결과가 나오는 특성이다.

```java
Animal animal = new Dog();
animal.sound();
```

위 코드에서는 Animal 타입으로 선언했지만 실제 객체가 Dog이므로 "멍멍"이 출력된다.

다형성을 사용하면 여러 객체를 하나의 타입으로 관리할 수 있어 코드의 유연성이 높아진다.

---

## 추상 메서드 (Abstract Method)

추상 메서드는 내용이 없는 메서드이며 자식 클래스에서 반드시 구현해야 한다.

```java
abstract class Animal {
    abstract void sound();
}
```

공통 규칙만 정하고 실제 동작은 자식 클래스에서 구현하도록 만들 때 사용한다.

---

## 인터페이스 (Interface)

인터페이스는 클래스가 반드시 구현해야 하는 기능의 규칙을 정의한다.

또한 인터페이스는 여러 개를 동시에 구현할 수 있다.

```java

interface Playable {

    void play();

}

interface Runnable {

    void run();

}

class Robot implements Playable, Runnable {

    public void play() {

        System.out.println("play");

    }

    public void run() {

        System.out.println("run");

    }

}

```

---

## 디폴트 메서드 (Default Method)

디폴트 메서드는 인터페이스 내부에서 기본 기능을 제공하는 메서드이다.

구현 클래스마다 같은 메서드를 반복해서 작성하지 않아도 된다.

```java
interface Animal {
    default void sleep() {
        System.out.println("잠자기");
    }
}
```

하지만 여러 인터페이스에서 같은 디폴트 메서드를 가지면 충돌이 발생할 수 있다.

또한 상속과 인터페이스의 디폴트 메서드가 동시에 존재할 경우,
항상 부모 클래스의 메서드가 우선 실행된다.

---

## 코드가 에러가 났던 이유

```java

for (Animal animal : animals) {
    animal.play();
}

```

이렇게 하면 컴파일 에러가 났었다.

```java
for (Animal animal : animals) {
    ((Playable) animal).play();
}
```

이런 식으로 인터페이스 타입으로 형변환을 시켜줘야 반복문에 Playable기능을 추가시킬 수 있게 된다.

---

## 느낀점

오버라이딩과 다형성의 관계를 배우면서 객체지향 프로그래밍의 핵심 개념을 이해할 수 있었다.

특히 같은 메서드라도 객체에 따라 다른 결과가 나온다는 점이 인상적이었다.

또한 인터페이스와 디폴트 메서드를 활용하면 코드의 재사용성과 유지보수성이 높아진다는 것을 알게 되었다.
