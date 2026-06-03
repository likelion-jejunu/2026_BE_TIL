# 📘 Today I Learned
오늘 배운 내용

### 1. 오늘 배운 내용 정리

- Spring MVC 요청 처리 흐름 복습
- Annotation의 역할
- 컴포넌트 스캔 Annotation
- HTTP 요청 처리 Annotation
- `@PathVariable`, `@RequestParam`, `@RequestBody`
- DI와 Lombok
- Controller, Service, Repository 역할 분리
- REST API 개념과 URI 규칙
- API 명세서 작성
- Postman으로 요청 테스트

---

### 2. 핵심 정리

1. Spring MVC 요청 처리 흐름 복습

브라우저에서 요청이 들어오면 `DispatcherServlet`이 모든 요청을 가장 먼저 받는다.
그 다음 `HandlerMapping`이 요청 URL을 보고 어떤 Controller의 메서드를 실행할지 찾는다.

예를 들어 `GET /posts` 요청이 들어오면
`@GetMapping("/posts")`가 붙은 Controller 메서드가 실행된다.

```java
@RestController
public class PostController {

    @GetMapping("/posts")
    public String getPosts() {
        return "게시글 목록";
    }
}
```

Spring이 자동으로 해주는 부분
-> `DispatcherServlet`, `HandlerMapping`

개발자가 작성하는 부분
-> Controller, Service, Repository, DTO

---

2. Annotation

Annotation: 클래스나 메서드 위에 붙이는 표시

Annotation을 사용하면 긴 XML 설정 대신 코드 위에 역할을 바로 표시할 수 있다.

```java
@RestController
public class PostController { }

@Service
public class PostService { }

@Repository
public class PostRepository { }
```

Annotation의 세 가지 역할

1. 컴포넌트 스캔
-> Spring이 클래스를 찾아 Bean으로 등록할 수 있게 해준다.

2. 요청 처리
-> 어떤 요청을 어떤 메서드가 처리할지 연결해준다.

3. DI
-> 필요한 의존 객체를 주입할 수 있게 도와준다.

---

3. 컴포넌트 스캔

`@Component`: 가장 기본이 되는 Annotation
`@ComponentScan`이 `@Component`를 보고 Bean으로 등록한다.

```java
@Component
public class MyComponent {
    // Spring이 관리하는 Bean이 됨
}
```

컴포넌트 스캔과 관련된 Annotation

- `@SpringBootApplication`
- `@Component`
- `@RestController`
- `@Controller`
- `@Service`
- `@Repository`

`@Controller`와 `@RestController`는 비슷하지만 차이가 있다.
`@RestController`는 `@Controller + @ResponseBody`처럼 동작한다.

컴포넌트 스캔 Annotation만 봐도
이 클래스가 Controller인지, Service인지, Repository인지 알 수 있다.

---

4. HTTP 요청 처리

같은 URL이라도 HTTP Method에 따라 의도가 달라진다.

예를 들어 `/posts`라는 URL 하나로도 다음처럼 구분할 수 있다.

```text
GET /posts  -> 게시글 목록 조회
POST /posts -> 게시글 생성
```

요청 처리와 관련된 Annotation

- `@RequestMapping`
- `@GetMapping`
- `@PostMapping`
- `@PutMapping`
- `@DeleteMapping`
- `@PathVariable`
- `@RequestBody`

URL은 자원을 나타내고,
HTTP Method는 그 자원에 대해 어떤 행동을 할지 나타낸다.

---

5. 요청 데이터 받기

`@PathVariable`: URL 경로에 들어있는 값을 꺼낼 때 사용

```java
@GetMapping("/posts/{id}")
public String getPost(@PathVariable Long id) {
    return id + "번 게시글";
}
```

`{id}`와 `@PathVariable` 변수명이 일치해야 한다.
만약 URL은 `{id}`이고 Java 변수명을 `postId`로 쓰고 싶다면 이름을 따로 지정해야 한다.

`@RequestParam`: URL의 물음표 뒤에 붙는 query 값을 꺼낼 때 사용

```text
GET /posts?page=2&size=10
```

```java
@RequestParam(defaultValue = "1") int page
```

`defaultValue`를 사용하면 page 값을 보내지 않았을 때 기본값을 넣을 수 있다.

언제 무엇을 쓰는지 정리

- "1번 게시글 보여줘"
-> `@PathVariable`

- "게시글을 페이지별로 보여줘"
-> `@RequestParam`

---

6. DTO와 `@RequestBody`

DTO는 요청 데이터를 담는 그릇처럼 사용한다.

```json
{
    "title": "안녕하세요",
    "content": "반갑습니다"
}
```

JSON 데이터를 Java 객체로 받을 때 `@RequestBody`를 사용한다.

```java
@PostMapping("/posts")
public PostRequest createPost(@RequestBody PostRequest request) {
    return request;
}
```

`@RequestBody`
-> JSON을 Java 객체로 바꿔준다.

이 과정을 역직렬화라고 한다.
응답을 줄 때는 반대로 Java 객체를 JSON으로 바꿀 수 있다.

주의할 점
-> JSON key 이름과 DTO 필드명이 일치해야 한다.

GET 요청은 Body가 없기 때문에
`@RequestBody`는 주로 POST, PUT에서 사용한다.

---

7. DI와 Lombok

DI: 객체를 직접 만들지 않고 외부에서 받아오는 것

`@Autowired`: Spring이 타입을 보고 알맞은 Bean을 찾아 필드에 넣어주는 방식

동작 흐름

1. 필드의 타입을 본다
2. Container에서 같은 타입의 Bean을 찾는다
3. 찾은 Bean을 필드에 대입한다

하지만 필드 주입은 권장되지 않는다.
대신 생성자 주입을 사용할 수 있다.

생성자를 직접 작성하는 것은 번거로울 수 있기 때문에
Lombok의 `@RequiredArgsConstructor`를 사용할 수 있다.

```java
@Service
@RequiredArgsConstructor
public class PostService {

    private final PostRepository postRepository;
}
```

`@RequiredArgsConstructor`
-> `final`이 붙은 필드를 모아서 생성자를 자동으로 만들어준다.

Lombok: 반복적인 Java 코드를 Annotation으로 자동 생성해주는 라이브러리

대표 Annotation

- `@Getter`
- `@Setter`
- `@NoArgsConstructor`
- `@AllArgsConstructor`

---

8. Controller, Service, Repository

Controller가 혼자 모든 일을 하면 코드가 길어지고 복잡해진다.
그래서 Controller, Service, Repository로 책임을 나누어야 한다.

- Controller
-> 요청을 받고, Service에 넘기고, 결과를 응답으로 돌려준다.

- Service
-> 비즈니스 로직을 처리한다.

- Repository
-> 데이터를 꺼내고 저장한다.

흐름

```text
Controller
-> Service
-> Repository
```

Controller가 일을 안 한다는 것은 아무것도 하지 않는다는 뜻이 아니다.
Controller는 요청과 응답에 집중하고,
계산, 판단, 필터링, 정렬, DB 직접 접근 같은 일은 하지 않는다.

비즈니스 로직: 우리 서비스만의 규칙
예를 들어 개수 제한, 필터링, 중복 검사 같은 규칙을 Service 계층에서 처리한다.

Repository: 저장, 조회, 수정, 삭제를 담당하는 데이터 접근 전용 계층
DB가 바뀌어도 Service가 직접 DB 변경에 흔들리지 않도록 역할을 나눈다.

---

9. REST API

API: 프로그램끼리 대화하는 창구
REST API: REST를 기반으로 만들어진 API

REST: 웹의 기존 기술과 HTTP 프로토콜을 활용하는 아키텍처 스타일
현재는 웹, 모바일, 다양한 디바이스와 통신해야 하므로
여러 환경에서 공통으로 사용할 수 있는 서버 설계 방식이 중요하다.

REST의 구성

- 자원 Resource
-> URI로 표현한다.

- 행위 Verb
-> HTTP Method로 표현한다.

- 표현 Representations
-> JSON, XML 같은 형태로 주고받는다.

예시

```text
GET /todos        -> 할 일 목록 조회
POST /todos       -> 할 일 생성
GET /todos/3      -> 3번 할 일 조회
PUT /todos/3      -> 3번 할 일 수정
DELETE /todos/3   -> 3번 할 일 삭제
```

REST에서는 URL에 동사를 넣지 않는다.
무엇을 할지는 URL이 아니라 HTTP Method가 표현한다.

좋은 예시
-> `GET /todos/3`

나쁜 예시
-> `/getTodo/3`

---

10. REST URI 기본 규칙

REST URI 규칙

- `/`는 계층 관계를 표현한다.
- URI 마지막 문자로 `/`를 포함하지 않는다.
- URI는 소문자를 사용한다.
- `_` 대신 `-`를 사용한다.
- 파일 확장자는 URI에 포함하지 않는다.
- URL에 동사를 넣지 않는다.

예시

```text
좋은 예시: /users/1
나쁜 예시: /users/1.json

좋은 예시: /user-profile
나쁜 예시: /user_profile
```

`@RestController`는 Java 객체를 JSON으로 변환해서 응답할 수 있다.
응답에는 상태 코드도 함께 전달된다.

---

### 3. 실습 / 과제

> Todo REST API 명세서를 작성하고 구현하는 것

API 명세서는 문서가 먼저, 코드가 나중이라는 흐름으로 작성한다.
즉, 구현하기 전에 어떤 요청과 응답이 오갈지 먼저 정리한다.

명세서에서 정리할 수 있는 내용

- API 이름
- HTTP Method
- URL
- Path Variable
- Query Parameter
- Request Body
- Response Body
- 상태 코드

Todo REST API 요구사항

- `GET /todos`
-> 저장된 Todo 목록 전체 반환

- `POST /todos`
-> 요청 Body의 JSON 데이터를 Todo 객체로 받고 리스트에 저장

추가 미션

- Todo에 `id` 필드 추가
- `GET /todos/{id}` 구현

POST 요청은 브라우저 주소창으로 테스트하기 어렵기 때문에
Postman을 사용해서 Body에 JSON 데이터를 넣고 테스트한다.

---

### 4. 느낀 점 & 다음 계획

처음에는 Annotation이 그냥 코드 위에 붙이는 표시 정도라고만 생각했는데,
정리하면서 Spring이 어떤 클래스를 Bean으로 등록할지, 어떤 요청을 어떤 메서드로 연결할지 판단하는 데 사용된다는 것을 알게 되었다.
이전에 배웠던 DI도 다시 나와서 처음엔 조금 헷갈렸지만,
`@RequiredArgsConstructor`랑 같이 보니까 생성자 주입이 왜 편한지 조금 더 이해할 수 있었다.

`@PathVariable`, `@RequestParam`, `@RequestBody`는 이름도 비슷하고 다 요청 값을 받는 것 같아서 헷갈렸는데,
값이 URL 경로에서 오는지, query에서 오는지, Body에서 오는지에 따라 다르게 사용한다는 기준이 생긴 것 같다.
REST API도 처음에는 단순히 URL을 만드는 규칙이라고 생각했는데,
URI는 자원을 표현하고 HTTP Method가 행동을 표현한다는 점이 중요하다는 것을 알게 되었다.

아직 API 명세서를 보고 바로 구현하는 게 익숙하진 않지만,
이번 과제를 하면서 `GET /todos`, `POST /todos`, `GET /todos/{id}` 흐름을 직접 만들어보면 조금 더 감이 잡힐 것 같다.
다음에는 Controller, Service, Repository 역할을 더 확실히 나누면서 코드를 작성해봐야겠다.