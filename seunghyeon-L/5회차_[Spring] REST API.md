# 📘 Today I Learned
## 5회차_[Spring] REST API

### Controller vs RestController
||@Controller|@RestController|
|---|---|---|
|반환|View(HTML 페이지)|데이터(문자열/JSON)|
|용도|화면 렌더링|API 서버|

@Controller는 "게시글 목록" 같은 **화면(HTML)** 을 띄우고<br>
@RestController는 그냥 **데이터(문자열/JSON)** 를 반환<br>
_(사실 @RestController = @Controller + @ResponseBody)_

### REST API란?
자원을 **URI**로, 그 자원에 대한 행위를 **HTTP Method**로 표현하는 방식

3요소
- 자원(Resource) : URI _ex) /todos_
- 행위(Verb) : HTTP Method _ex) GET, POST_
- 표현(Representation) : 주고받는 데이터 형식 _ex) JSON_

### HTTP Method — 행위 구분
같은 URL이어도 **Method가 다르면 다른 동작**<br>
즉 "무엇을(URL)" + "어떻게(Method)" 로 동작을 구분

|Method|동작|예시|
|---|---|---|
|GET|조회|GET /todos|
|POST|생성|POST /todos|
|PUT/PATCH|수정|PUT /todos/1|
|DELETE|삭제|DELETE /todos/1|

### URI 설계 규칙
- 동사 쓰지 않기 (행위는 Method가 담당) ❌ `/getTodos` ⭕ `GET /todos`
- 소문자 사용
- 단어 연결은 하이픈(-)
- 파일 확장자(.json 등) 쓰지 않기
- 자원은 복수형 명사 (todos)

_ex) "할 일 목록 조회"는 url에 동사를 못 쓰니 → `GET /todos`_

### @PathVariable vs @RequestParam
`/todos/1?page=1` 이라면<br>
- `1` → 경로(path)의 일부 = **@PathVariable**
- `page=1` → 쿼리스트링(?뒤) = **@RequestParam**

```java
// /todos/1
@GetMapping("/todos/{id}")
public Todo getTodo(@PathVariable Long id) { ... }

// /todos?page=1
@GetMapping("/todos")
public List<Todo> getTodos(@RequestParam int page) { ... }
```

### @RequestBody
요청 본문(body)에 담긴 JSON을 객체로 변환해서 받음
```java
@PostMapping("/todos")
public Todo create(@RequestBody Todo todo) { ... }
```
**GET에는 body가 없음** → 조회 조건은 @RequestParam(쿼리스트링)으로 전달

### DI 다시 — new vs @Autowired
```java
// new : 내가 직접 만듦 → 강하게 엮임 (결합도 ↑)
private TodoService service = new TodoService();

// @Autowired : Spring이 관리하는 Bean을 주입받음
@Autowired
private TodoService service;
```
@Autowired로 주입받으려면 그 객체가 **Spring에 Bean으로 등록**돼 있어야 함<br>
_(@Component, @Service, @Repository, @Controller 등이 붙으면 Bean으로 등록됨)_

### 계층 구조 (Layered Architecture)
요청이 흐르는 순서<br>
**Controller → Service → Repository**

- Controller : 요청 받고 응답 보내기 (@RestController)
- Service : 비즈니스 로직 처리 (@Service)
- Repository : 데이터 저장/조회 (@Repository)

역할을 나눠서 유지보수를 쉽게!

### @Transactional
하나의 작업 묶음을 **"전부 성공 or 전부 실패"** 로 처리

_ex) 주문 저장 + 재고 감소_<br>
재고 감소가 실패하면 → 주문 저장도 같이 **롤백**되어 처음 상태로 되돌아감

### 실습 — Todo REST API
메모리(List)에 할 일을 저장하는 간단한 API 만들기

```java
@RestController
public class TodoController {

    private final List<Todo> todos = new ArrayList<>();
    private Long sequence = 1L;

    // 전체 조회
    @GetMapping("/todos")
    public List<Todo> getTodos() {
        return todos;
    }

    // 생성
    @PostMapping("/todos")
    public Todo createTodo(@RequestBody Todo todo) {
        todo.setId(sequence++);   // id 자동 부여
        todos.add(todo);
        return todo;
    }

    // 단건 조회 (보너스)
    @GetMapping("/todos/{id}")
    public Todo getTodo(@PathVariable Long id) {
        for (Todo todo : todos) {
            if (todo.getId().equals(id)) {
                return todo;
            }
        }
        return null;
    }
}
```

|Method|URL|설명|
|---|---|---|
|GET|/todos|전체 할 일 조회|
|POST|/todos|할 일 생성|
|GET|/todos/{id}|id로 단건 조회|
