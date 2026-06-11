# 📘 Today I Learned
## 6회차_[Spring] DTO와 CRUD 실습

### Entity를 그대로 쓰면 안 되는 이유
- 비밀번호처럼 보여주면 안 되는 정보까지 노출됨
- DB(Entity)가 바뀌면 API 응답도 같이 깨짐
- 들어올 때/나갈 때 필요한 필드가 다름

→ 계층 사이를 건너는 전용 그릇 **DTO** 가 필요

### Request DTO vs Response DTO
방향이 다르면 그릇도 나눈다
- 받을 때(요청) : Request DTO _(title만)_
- 보낼 때(응답) : Response DTO _(id, title, done … 노출할 것만)_

### 직렬화 / 역직렬화
- 역직렬화 : JSON → 자바 객체 (@RequestBody)
- 직렬화 : 자바 객체 → JSON (응답)

JSON 키 = 필드명 이 일치해야 매칭됨. 안 맞으면 null

### Entity ↔ DTO 변환
타입이 다르면 못 넘기니까 변환이 필요.<br>
Controller에서 직접 변환하면 지저분 → 변환을 DTO 안 메서드로 넘김 (책임 분리)

```java
request.toEntity();        // Request DTO → Entity
TodoResponse.from(saved);  // Entity → Response DTO (static)
```
- `from()`이 static인 이유 : 객체 없이 클래스 이름으로 바로 호출
- Response DTO에 @Getter 없으면 응답이 `{ }` 로 빔 (직렬화 때 Getter로 값 읽음)

### API 명세서
만들기 전에 어떤 요청을 받고 무엇을 줄지 먼저 정리 (메뉴판 역할)

- Path Variable : 특정 대상 하나 지정 `/items/3`
- Query Parameter : 검색·필터 `/items?date=2024-10-20`
- Request Body : POST/PUT은 필요, GET/DELETE는 보통 없음
- Status : 200 정상 / 201 등록 / 204 삭제 / 400 요청오류 / 404 없음

### CRUD 실습 — 상품(Item) API
작성 순서는 안쪽(데이터)부터 바깥(입구)으로<br>
**Entity → Repository → DTO → Service → Controller**

- Entity : 데이터 모양만 (계산·로직 없음)
- Repository : DB 대신 HashMap에 저장 (save/findAll/findById/update/delete)
- DTO : 받을 땐 id 없고 보낼 땐 id 있어야 해서 2개로 분리
- Service : 직접 저장 안 하고 Repository를 시킴, 변환·흐름만 담당
- Controller : `@RestController` + `@RequestMapping("/api/items")`, 메서드별 매핑

|기능|Method|URL|
|---|---|---|
|목록 조회|GET|/api/items|
|단건 조회|GET|/api/items/{id}|
|등록|POST|/api/items|
|수정|PUT|/api/items/{id}|
|삭제|DELETE|/api/items/{id}|

Postman으로 테스트 (POST/PUT은 Body raw → JSON, Content-Type: application/json)
