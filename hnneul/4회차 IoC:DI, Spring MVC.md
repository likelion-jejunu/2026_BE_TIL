# 📘 Today I Learned
오늘 배운 내용

### 1. 오늘 배운 내용 정리

- IoC / DI 개념 복습
- 객체를 직접 생성할 때 생기는 문제점과 생성자 주입 학습
- Spring IoC 컨테이너, Bean, Annotation 개념 학습
- Lombok과 `@RequiredArgsConstructor` 개념 학습
- Spring MVC 요청 처리 흐름 학습
- Controller, Service, Repository, DTO 역할 정리
- HTTP / HTTPS와 HTTP Method 개념 학습
- CommentController를 만들어 `/comments` 요청에 응답하는 실습

---

1. IoC / DI 복습

IoC(Inversion of Control)
: 제어의 역전
-> 객체의 생성과 호출의 권한을 개발자가 직접 가지는 것이 아니라 외부 컨테이너에 맡기는 것

DI(Dependency Injection)
: 의존성 주입
-> 객체가 필요한 의존 객체를 직접 만들지 않고 외부에서 전달받는 방식

- A가 B 없이는 일을 할 수 없다면
-> A는 B에 의존하고 있다

```java
class OrderService {
    PaymentProcessor p; // 의존
    EmailSender e;      // 의존
    OrderRepository r;  // 의존
}
```

OrderService는 결제, 메일, 저장소가 없으면 제대로 동작할 수 없음
-> 이 의존 객체들을 누가 만들어서 넣어줄 것인가가 DI의 핵심

---

2. 객체를 직접 생성할 때의 문제점

```java
public class OrderService {

    private final PaymentProcessor payment;

    public OrderService() {
        this.payment = new TossPayProcessor();
    }

    public void placeOrder(Order order) {
        payment.pay(order.getAmount());
        System.out.println("주문 완료");
    }
}
```

위 코드의 문제점

1) 결제 수단을 바꾸려면 OrderService를 직접 고쳐야 함
-> TossPayProcessor에서 KakaoPayProcessor로 바뀌면 같은 파일을 또 수정해야 함

2) 테스트가 어려움
-> 가짜 결제 객체를 넣어서 테스트하고 싶어도 OrderService 안에서 이미 직접 객체를 만들고 있음

3) 책임이 너무 많음
-> OrderService가 주문 처리도 하고 결제 객체 생성도 함

정리
-> 사용하는 책임과 만드는 책임이 한 곳에 묶이면 변경이 생겼을 때 코드가 같이 흔들림

---

3. 생성자 주입과 final

생성자 주입
: 객체를 만들 때 필요한 의존성을 생성자를 통해 넣어주는 방식

```java
public class OrderService {

    private final PaymentProcessor paymentProcessor;
    private final OrderRepository orderRepository;

    public OrderService(PaymentProcessor paymentProcessor, OrderRepository orderRepository) {
        this.paymentProcessor = paymentProcessor;
        this.orderRepository = orderRepository;
    }
}
```

final
: 한 번 값이 들어가면 다시 바뀌지 않도록 막는 키워드

- final을 붙이면
-> 실수로 의존성이 바뀌는 일을 막을 수 있음

- 생성자 주입을 사용하면
-> 테스트할 때 원하는 객체를 직접 넣을 수 있음

```java
PaymentProcessor fakePayment = new FakePayProcessor();
OrderRepository fakeRepository = new FakeOrderRepository();

OrderService orderService = new OrderService(fakePayment, fakeRepository);
```

결론
-> 생성자 주입 + final을 사용하면 더 안전하고 테스트하기 쉬운 구조가 됨

---

4. 주입하는 세 가지 방법

1) 생성자 주입
: 객체를 만들 때 생성자로 의존성을 넣어주는 방식
-> 필요한 값이 처음부터 들어오기 때문에 안정적임
-> 수업에서는 생성자 주입을 사용하자고 정리함

2) Setter 주입
: 객체를 먼저 만들고 나중에 Setter 메서드로 의존성을 넣어주는 방식
-> Spring 컨테이너가 객체를 만든 다음 @Autowired가 붙은 Setter를 자동으로 호출함

but, 객체가 만들어진 직후부터 Setter가 호출되기 전까지는 의존성이 비어 있을 수 있음

3) 필드 주입
: 필드에 바로 의존성을 넣는 방식

---

5. Spring IoC 컨테이너

Spring IoC 컨테이너
: Spring이 객체를 만들고 필요한 곳에 넣어주는 역할을 하는 곳

정식 이름
-> Application Context

- Application Context는
    1. 필요하다고 선언된 객체를 직접 만들고
    2. 컨테이너 안에 가지고 있다가
    3. 필요한 곳에 가져다 줌

입력
-> 설정 + annotation

출력
-> 이미 연결된 객체들

즉, 개발자가 직접 모든 객체를 new로 만들 필요 없이
Spring이 객체들을 만들고 연결해줌

---

6. Bean

Bean
: Spring 컨테이너 안에서 관리되는 객체

평소에 new로 만든 Java 객체와 다른 점

1) 컨테이너가 직접 생성함
2) 앱 전체에서 기본적으로 하나만 존재함

-> 같은 객체를 여기저기서 계속 새로 만드는 것이 아니라
-> 컨테이너가 하나를 가지고 있다가 필요한 곳에서 공유해서 사용함

이런 구조
-> 싱글톤

예시
OrderService가 Bean으로 등록되어 있으면
-> 여러 Controller가 같은 OrderService 인스턴스를 공유해서 사용 가능

---

7. Annotation

Annotation
: 클래스나 메서드 위에 붙이는 메모 / 라벨

Spring은 Annotation을 보고
-> 어떤 클래스를 Bean으로 등록할지 판단함

```java
@Component
public class OrderService {
}
```

@Component
: 클래스 위에 붙이면 Spring이 시작할 때 찾아서 객체를 만들고 Bean으로 등록

역할별 Annotation

- @Service
-> 비즈니스 로직 계층

- @Repository
-> DB 계층

- @Controller
-> HTTP 요청을 받는 계층

이 세 가지는 Bean으로 등록된다는 점에서는 @Component와 비슷하게 동작함
but, 코드를 읽는 사람에게 이 클래스가 어떤 역할인지 더 잘 알려줌

주의
인터페이스와 추상 클래스는 new가 불가능함
-> Spring이 직접 객체를 만들 수 없기 때문에 @Component를 붙여도 직접 객체로 만들 수 없음

---

8. Lombok

Lombok
: 반복되는 코드를 줄여주는 라이브러리

@RequiredArgsConstructor
: final이 붙은 필드들을 모아서 생성자를 자동으로 만들어주는 annotation

```java
@Service
@RequiredArgsConstructor
public class OrderService {

    private final PaymentProcessor paymentProcessor;
    private final OrderRepository orderRepository;
}
```

위 코드에서 Lombok이 자동으로 만들어주는 것
-> paymentProcessor, orderRepository를 받는 생성자

Spring Initializr에서 Dependency에 Lombok을 추가해서 사용할 수 있음

---

9. Spring IoC 컨테이너 동작 흐름

Spring IoC 컨테이너는 설정만 보고 객체 그래프 전체를 조립할 수 있는 시스템

1. @Component, @Service, @Repository, @Controller 등이 붙은 클래스를 찾음
2. 찾은 클래스들을 new로 객체로 만듦
3. 만든 객체를 Bean으로 등록해서 컨테이너 안에 보관함
4. 생성자가 필요로 하는 의존성을 이미 가지고 있는 Bean 중에서 찾아 넣어줌
5. 개발자는 완성된 Bean을 받아서 메서드만 호출하면 됨

Bean을 보관하는 느낌
-> key-value처럼 라벨을 붙여 저장해두는 느낌

---

10. Spring MVC

Spring MVC
: 요청을 역할별로 나누어 처리하는 구조

Controller가 모든 일을 다 처리하는 것이 아님
-> Controller / Service / Repository가 역할을 나누어 처리함

구조

```text
Controller
Service
Repository
DB
```

---

11. Spring MVC 전체 흐름

Spring이 만들어 놓은 부분
-> DispatcherServlet, HandlerMapping 등

개발자가 직접 구현하는 부분
-> Controller, Service, Repository

요청 처리 흐름

1. 브라우저에서 요청이 들어옴
2. DispatcherServlet이 요청을 가장 먼저 받음
3. HandlerMapping이 어떤 Controller가 처리해야 하는지 찾음
4. Controller가 요청을 받고 Service에 로직 처리를 요청함
5. Service가 비즈니스 로직을 처리함
6. DB 데이터가 필요하면 Repository에 데이터를 요청함
7. Repository가 DB에서 데이터를 가져와 Service에 전달함
8. Service가 처리 결과를 Controller에 돌려줌
9. Controller가 결과를 응답으로 반환함

DispatcherServlet
: 모든 요청을 가장 먼저 받는 존재
-> Front Controller라고 부름

HandlerMapping
: 요청 URL과 Controller 메서드를 연결해주는 역할

```java
@RestController
public class PostController {

    @GetMapping("/posts")
    public String getPosts() {
        return "게시글 목록";
    }
}
```

브라우저에서 /posts 요청이 들어오면
-> HandlerMapping이 @GetMapping("/posts")를 보고 실행할 메서드를 찾음

---

12. HTTP

HTTP
: Hypertext Transfer Protocol
-> 서로 다른 시스템들 사이에서 통신을 주고받게 하는 가장 기본적인 프로토콜

브라우저와 서버가 데이터를 주고받을 때
-> 아무 규칙 없이 주고받는 것이 아니라 HTTP 규칙을 따름

예시
브라우저에서 localhost:8080/posts로 접속
-> 서버가 "게시글 목록"을 응답으로 돌려줌

---

13. HTTP vs HTTPS

HTTP
: 데이터를 그대로 주고받는 방식
-> 중간에서 누군가 데이터를 가로채면 내용을 볼 수 있음

HTTPS
: 데이터를 암호화해서 주고받는 방식
-> 중간에 누가 데이터를 가져가도 암호를 풀 수 없기 때문에 더 안전함

로그인이나 결제처럼 보안이 중요한 정보를 입력할 때
-> 반드시 HTTPS가 필요함

HTTP와 HTTPS는 보안 차이가 있지만
-> 요청을 보내는 방식 자체는 동일함

---

14. HTTP Method

HTTP Method
: 같은 주소로 요청을 보내더라도 어떤 행동을 원하는지 구분하기 위해 사용

예를 들어 /posts라는 주소 하나로도
-> 게시글 조회, 작성, 수정, 삭제를 나눌 수 있음

GET
: 데이터를 가져올 때 사용
-> 브라우저 주소창에 입력해서 요청하는 것은 대부분 GET 방식

POST
: 데이터를 생성할 때 사용
-> 게시글 작성처럼 사용자가 입력한 데이터를 서버에 보낼 때 사용

PUT
: 데이터를 수정할 때 사용

DELETE
: 데이터를 삭제할 때 사용

```java
@GetMapping("/posts")
public String getPosts() {
    return "게시글 목록";
}
```

```java
@PostMapping("/posts")
public String writePost(@RequestBody String content) {
    return content + " 게시글이 작성되었습니다.";
}
```

@GetMapping
-> GET 요청이 오면 해당 메서드를 실행하라는 뜻

@PostMapping
-> POST 요청이 오면 해당 메서드를 실행하라는 뜻

@RequestBody
-> 브라우저가 보내준 데이터를 변수에 넣어주는 역할

POST 요청은 브라우저 주소창에 단순히 입력해서 확인하기 어려움
-> Postman 같은 툴로 테스트해야 함

---

### 3. 추가 공부

1. Controller
: 요청을 받고 응답을 돌려주는 역할

- 사용자가 브라우저나 Postman으로 요청을 보내면
-> Controller가 가장 먼저 요청을 받음

- Controller는 모든 로직을 직접 처리하지 않음
-> 실제 로직은 Service에게 맡김

예시
-> `/posts` 요청을 받으면 게시글 목록을 응답으로 돌려줌

```java
@RestController
public class PostController {

    @GetMapping("/posts")
    public String getPosts() {
        return "게시글 목록";
    }
}
```

정리
-> Controller는 요청과 응답을 담당하는 입구 역할

---

2. Service
: 실제 비즈니스 로직을 처리하는 역할

비즈니스 로직
-> 프로그램에서 실제로 처리해야 하는 핵심 규칙이나 기능

예시
-> 게시글 작성, 댓글 작성, 주문 처리, 결제 처리 등

- Controller가 요청을 받으면
-> Service에게 실제 처리를 맡김

- Service에서 데이터가 필요하면
-> Repository에게 DB 데이터를 요청함

정리
-> Service는 실제 기능이 어떻게 동작해야 하는지 처리하는 계층

---

3. Repository
: DB에서 데이터를 꺼내오는 역할

- Service가 데이터가 필요하다고 요청하면
-> Repository가 DB에서 데이터를 가져옴

- 가져온 데이터는 다시 Service로 전달함

예시
-> 게시글 목록 조회 시 DB에서 게시글 데이터를 가져오는 역할

정리
-> Repository는 DB와 가까운 계층
-> 데이터 조회, 저장 같은 일을 담당함

---

4. DTO
: 계층 간 데이터를 주고받을 때 쓰는 객체

DTO(Data Transfer Object)
-> 데이터를 전달하기 위한 객체

- Controller에서 Service로 데이터를 넘길 때
- Service에서 Controller로 결과를 돌려줄 때
- 요청 데이터나 응답 데이터를 담을 때

DTO를 사용할 수 있음

정리
-> DTO는 계층 사이에서 데이터를 전달하기 위한 상자 같은 역할

전체 흐름

```text
Client
-> Controller
-> Service
-> Repository
-> DB
```

---

### 4. 실습 / 과제 / 결과물

HTTP GET 메소드를 사용해서 CommentController 만들기

요구사항
-> localhost:8080/comments 주소로 접속하면 "댓글 목록"이 보이도록 만들기

- 코드:

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class CommentController {

    @GetMapping("/comments")
    public String getComments() {
        return "댓글 목록";
    }
}
```

정리

- @RestController
-> HTTP 요청을 받을 수 있는 Controller로 만들어줌

- @GetMapping("/comments")
-> /comments GET 요청과 메서드를 연결함

- return "댓글 목록";
-> 브라우저 화면에 댓글 목록이라는 문자열이 보임

- 링크:
-> 없음

- 스크린샷:
-> 없음

---

### 5. 느낀 점 & 다음 계획

이번 세션은 1회차 때 배웠던 IoC, DI가 다시 나와서 처음에는 또 어렵게 느껴졌다.
그래도 이번에는 OrderService 예시랑 결제 객체를 바꾸는 상황을 보면서 왜 직접 new로 만들면 불편한지 조금 더 이해할 수 있었다.

Application Context, Bean, 싱글톤 같은 말은 아직 완전히 익숙하진 않지만,
Spring이 객체를 직접 만들고 가지고 있다가 필요한 곳에 넣어준다는 흐름은 정리하면서 조금 알 것 같다.
특히 @Service, @Repository, @Controller 같은 annotation이 그냥 이름표가 아니라 Spring이 객체를 찾는 기준이 된다는 점이 신기했다.

Spring MVC는 DispatcherServlet, HandlerMapping처럼 이름이 길고 낯선 개념이 많아서 헷갈렸다.
그래도 Controller가 모든 일을 다 하는 게 아니라 Service, Repository로 역할을 나눠서 처리한다는 것은 확실히 기억해야겠다고 느꼈다.

다음에는 오늘 배운 Controller, Service, Repository 역할을 다시 한 번 정리해보고,
간단한 예제 코드에서 요청이 어떤 순서로 이동하는지 직접 따라가보면서 더 익숙해져야겠다.