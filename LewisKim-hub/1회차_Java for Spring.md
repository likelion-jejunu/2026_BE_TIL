# 📘 Today I Learned 1회차 : 백엔드 첫 세션

## 1. 오늘 배운 내용
- **Spring 생태계에 대한 전반적인 이해**
  - What is Framework - Frame(틀, 규칙) + Work(일, 소프트웨어의 목적)
  - Spring Framework 사용 -> Java 기반 어플 + 비즈니스 로직에만 집중
  - Spring 1. 비즈니스 로직과 어플리케이션 기술의 분리 2. 객체 지향을 최대한 살려 개발

- **Spring 구조**
  - `Controller`(요청 처리), `Service`(기능 처리), `Repository`(데이터 저장), `Entity`(데이터 형태)

- **IoC(Inversion of Control)**
  - 객체의 생성과 생명주기 관리의 주체가 개발자에서 스프링으로 바뀌는 것
  - **Dependency(의존 관계)** : 의존대상 "B"가 변하면 "A"에 영향을 미친다.
  - **(Before)** 객체를 개발자가 "직접" 생성, 필요한 시점에 개발자가 "직접" 호출, 프로그램 흐름을 전부 개발자가 "직접" 제어하고 있는 구조

- **DI(Dependency Injection)**
  - 객체를 "직접" 만들지 않고 생성자, 세터 등을 통해 외부에서 전달받는 방식
  - **Good** : 필수 값이 항상 보장된다(객체 생성 시점에 의존성이 보장 + 불변성 유지)

---

## 2. 핵심 정리 (내 언어로)
> IoC의 개념을 생각해보면 과거에는 사장님이 모든 걸 다 하는 1인 식당이라면 IoC 개념을 도입한 이후에는 분업화된 프랜차이즈 식당이라는 생각을 했다. 원래는 사장님이 음식도 만들고 계약서도, 다른 것도 다 하는데, IoC는 사장님이 이제 요리에만 집중할 수 있게 되어서 초반에 JAVA framework에서 말한 비즈니스 로직에만 집중할 수 있다는 말로 연결되는 것 같다고 느꼈다. 

---

## 3. 실습 / 과제 / 결과물

### ✅ 생성자 주입
```java
public class UserService {

    private final UserRepository userRepository;

    public UserService(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}

✅ 새터 주입
public class UserService {

    private UserRepository userRepository;

    public void setUserRepository(UserRepository userRepository) {
        this.userRepository = userRepository;
    }
}