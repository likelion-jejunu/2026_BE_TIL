# TIL - 백엔드 5회차: Annotation, REST API, API 명세서, Postman

> 2025년 월 일 | Likelion JNU 14th 백엔드 세션

---

## 📌 오늘 배운 것 요약

- Spring Annotation의 개념과 세 가지 역할
- Controller / Service / Repository 계층 분리
- REST API 개념과 URI 설계 규칙
- API 명세서 작성 방법
- Postman으로 API 테스트하기

---

## 1. Annotation이란?

Annotation은 사전적으로 "주석"을 뜻하지만, 일반 주석(`// comment`)과는 다르게 **컴파일러와 프레임워크가 읽고 동작하는 메타데이터**이다.

### 어노테이션이 없었다면?

과거 Spring에서는 XML 파일에 수백 줄을 직접 작성해야 했다.

```xml
<bean id="postController" class="com.example.PostController">
    <property name="postService" ref="postService"/>
</bean>
```

어노테이션 덕분에 이게 이렇게 줄어든다:

```java
@RestController
public class PostController { }
```

### Annotation의 세 가지 역할

| 역할             | 대표 어노테이션                                                                                                  |
| ---------------- | ---------------------------------------------------------------------------------------------------------------- |
| 컴포넌트 스캔    | `@SpringBootApplication`, `@Component`, `@RestController`, `@Service`, `@Repository`                             |
| 요청 처리        | `@GetMapping`, `@PostMapping`, `@PutMapping`, `@DeleteMapping`, `@PathVariable`, `@RequestBody`, `@RequestParam` |
| DI (의존성 주입) | `@Autowired`, `@RequiredArgsConstructor`                                                                         |

### @Controller vs @RestController

- `@RestController` = `@Controller` + `@ResponseBody`
- `@RestController`는 자바 객체를 자동으로 **JSON으로 변환**해서 응답한다.

---

## 2. DI (Dependency Injection, 의존성 주입)

객체를 직접 생성(`new`)하지 않고, **외부(Spring Container)에서 받아오는 것**.

```java
// 직접 생성 (지양)
MemberService service = new MemberService();

// DI (권장)
@Autowired
private MemberService memberService;
// "스프링아, 네가 가진 MemberService 객체 좀 빌려줘"
```

### @Autowired 동작 방식

1. 필드의 **타입**을 확인 → `PostService`
2. Spring Container를 탐색 → `PostService` 타입의 Bean 있어?
3. 찾은 Bean을 필드에 대입

> **필드 주입보다 생성자 주입이 권장된다.** → `@RequiredArgsConstructor` 사용

### Lombok - @RequiredArgsConstructor

반복적인 자바 코드를 어노테이션으로 자동 생성해주는 라이브러리.

```java
@RequiredArgsConstructor  // final 필드 생성자 자동 생성
public class PostController {
    private final PostService postService;
}
```

주요 Lombok 어노테이션:

- `@Getter` / `@Setter` - getter, setter 자동 생성
- `@NoArgsConstructor` - 기본 생성자
- `@AllArgsConstructor` - 전체 필드 생성자

---

## 3. Controller / Service / Repository 계층 분리

### 왜 Controller가 혼자 다 하면 안 될까?

모든 로직을 Controller에 몰아넣으면 한 메서드가 50줄이 넘어가고, 유지보수가 불가능해진다.

### 책임 분리

| 계층       | 역할                                    | 비유      |
| ---------- | --------------------------------------- | --------- |
| Controller | 요청을 받고, 응답을 돌려준다            | 홀 직원   |
| Service    | 비즈니스 로직 처리 (계산, 판단, 필터링) | 주방장    |
| Repository | DB 접근 전용 (저장, 조회, 수정, 삭제)   | 창고 담당 |

### 비즈니스 로직이란?

"우리 서비스만의 규칙"

- ex1) 한 사람당 할 일은 최대 10개까지
- ex2) 완료된 할 일은 목록에서 제외
- ex3) 같은 제목의 할 일은 만들 수 없음

→ 이런 규칙들을 처리하는 곳이 **Service 계층**

### Service의 장점

- **재사용성**: 여러 Controller에서 같은 Service 호출 가능
- **트랜잭션 관리**: 여러 DB 작업을 하나의 작업 단위로 묶음
- **테스트 용이성**: 웹 서버 없이 순수 Java 객체처럼 테스트 가능

---

## 4. REST API

### REST란?

- **RE**presentational **S**tate **T**ransfer
- 자원을 URI로 표현하고, HTTP Method로 행위를 구분하는 아키텍처 스타일
- 로이 필딩(Roy Fielding)의 박사학위 논문에서 등장

### REST의 구성 3요소

| 요소                  | 표현        | 예시                           |
| --------------------- | ----------- | ------------------------------ |
| 자원 (Resource)       | URI         | `/todos`, `/users`             |
| 행위 (Verb)           | HTTP Method | `GET`, `POST`, `PUT`, `DELETE` |
| 표현 (Representation) | 데이터 형식 | JSON, XML                      |

### 같은 URL, 다른 메서드로 의도를 구분

```
GET    /posts      → 게시글 목록 조회
POST   /posts      → 게시글 생성
PUT    /posts/{id} → 게시글 수정
DELETE /posts/{id} → 게시글 삭제
```

### REST URI 설계 규칙

```
❌ /GetTodos/       → 동사 사용 금지, 마지막 슬래시 금지
❌ /get-todos       → 동사 사용 금지
❌ /user_profile    → 언더스코어 대신 하이픈 사용
✅ /user-profile

❌ /users/1.json    → 파일 확장자 URI에 포함 금지
✅ /users/1
```

**핵심: URL은 "무엇을" 나타내고, HTTP Method가 "어떻게"를 나타낸다.**

---

## 5. 요청 데이터를 받는 방법

### @PathVariable - 경로에서 변수 추출

```java
@GetMapping("/posts/{id}")
public String getPost(@PathVariable Long id) { ... }
// "1번 게시글 보여줘" → /posts/1
```

### @RequestParam - 쿼리 파라미터 추출

```java
@GetMapping("/posts")
public String getPosts(@RequestParam(defaultValue = "1") int page) { ... }
// GET /posts?page=2&size=10
```

### 언제 뭘 쓸까?

- `@PathVariable` → 특정 리소스를 가리킬 때 ("1번 게시글")
- `@RequestParam` → 필터링, 페이징 ("2페이지, 10개씩")

### @RequestBody - 요청 Body에서 JSON 파싱

```java
@PostMapping("/posts")
public Post createPost(@RequestBody PostRequest request) { ... }
// JSON → Java 객체로 역직렬화
```

> GET에는 Body가 없으므로 주로 POST, PUT에서 사용

---

## 6. API 명세서

### 작성 시점

**문서가 먼저, 코드가 나중** — 개발 전에 명세서를 작성한다.

### 명세서에 포함할 항목

- HTTP Method
- URI (엔드포인트)
- 요청 파라미터 / 요청 Body
- 응답 Body
- HTTP 상태 코드

---

## 7. HTTP 상태 코드

| 코드                      | 의미        |
| ------------------------- | ----------- |
| 200 OK                    | 성공        |
| 201 Created               | 생성 성공   |
| 400 Bad Request           | 잘못된 요청 |
| 404 Not Found             | 리소스 없음 |
| 500 Internal Server Error | 서버 오류   |

---

## 💡 오늘의 핵심 인사이트

1. **어노테이션 하나만 봐도 이 클래스가 어떤 역할인지 바로 알 수 있다!**
2. Controller는 "교통정리"만 하고, 실제 로직은 Service에게 맡긴다.
3. REST에서 URL은 명사(자원), HTTP Method는 동사(행위)다.
4. DI 덕분에 `new`로 직접 객체를 만들 필요가 없다 — Spring이 알아서 주입해준다.

---

## 🔧 실습

- Todo REST API 명세서 작성 및 구현
  - `GET /todos` - 전체 목록 반환
  - `POST /todos` - 새 Todo 추가
  - (추가 미션) `GET /todos/{id}` - 특정 Todo 조회

---

## 📚 참고

- [Postman](https://www.postman.com/) - API 테스트 도구
- Spring Boot 공식 문서
- Lombok 공식 문서
