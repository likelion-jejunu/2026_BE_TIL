# 1회차 Java for Spring TIL

---

## 📚 배운 것 중 기억에 남는 것

### 1️⃣ Framework란?

Framework는 **소프트웨어 개발을 할 때 기본적인 구조와 흐름을 제공해주는 개발 틀**이다.
개발자는 Framework가 제공하는 구조 안에서 필요한 기능을 구현하여 프로그램을 만든다.
Framework를 사용하면 개발 속도가 빨라진다. 또 코드 구조가 일정해지고 유지보수가 쉬워진다는 장점이 있다.
대표적인 예로 **Spring Framework**가 있다.

---

## ⚙️ IoC 적용

IoC는 **제어의 역전**이라는 의미로
객체의 생성과 관리 권한을 개발자가 직접 하는 것이 아니라 **Spring 컨테이너가 관리하는 방식**이다.

즉, 개발자가 객체를 직접 생성하는 것이 아니라
Spring이 객체를 생성하고 필요한 곳에 주입해준다.

이렇게 하면 객체 간 결합도가 낮아지고, 유지보수가 쉬워진다.

---

## 🔧 의존성 주입 방식

### 1️⃣ 생성자 주입

객체를 생성할 때 **생성자를 통해 의존성을 주입하는 방식**이다.

```java
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

특징

* 불변성 유지 가능
* 테스트하기 쉬움
* Spring에서 가장 권장되는 방식

---

### 2️⃣ Setter 주입

Setter 메서드를 통해 의존성을 주입하는 방식이다.

```java
public class UserService {

    private UserRepository userRepository;

    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}
```

특징

* 선택적인 의존성 주입 가능
* 객체 생성 이후에도 변경 가능
