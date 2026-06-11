### 1. 오늘 배운 내용 정리

- DTO가 필요한 이유와 Entity와 DTO의 차이
- Request DTO와 Response DTO를 나누는 이유와 변환 방법
- Entity, Repository, DTO, Service, Controller를 이용한 상품 CRUD 구현

---

### 2. 오늘 막혔던 것

CRUD 실습은 세션을 들을 때는 코드를 보면서 같이 작성해서 잘할 수 있을 것 같았는데,
실제로 혼자 해보니까 너무 헷갈렸다.

Entity, Repository, DTO, Service, Controller를 어떤 순서로 작성하고 연결해야 하는지
좀 더 공부해야겠다고 느꼈다.. ㅠㅠ

---

### 3. 핵심 정리

1. DTO가 필요한 이유

DTO: 계층이나 네트워크 사이에서 데이터를 전달하는 그릇

Entity를 요청, 응답, DB 저장에 모두 사용했을 때 생기는 문제

1. 응답에 비밀번호처럼 보여주면 안 되는 값까지 포함될 수 있다.
2. 요청마다 필요하지 않은 필드도 외부에 열리게 된다.
3. DB의 컬럼이 바뀌면 API의 응답도 함께 깨질 수 있다.

DTO를 사용하면 요청에 필요한 값과 응답으로 보여줄 값을 용도에 맞게 따로 담을 수 있다.

---

2. Entity와 DTO

Entity: DB 테이블의 한 행과 자바 객체 하나가 짝을 이루는 객체

Entity의 동작 흐름

1. Repository가 Entity를 사용해 DB와 데이터를 주고받는다.
2. Entity를 저장하면 DB 테이블에 한 행이 생긴다.
3. DB를 조회하면 테이블의 한 행이 Entity 객체로 돌아온다.

DTO: Entity에서 필요한 값만 골라 원하는 형태로 전달하는 객체

Entity와 DTO를 분리하면 DB의 구조와 API의 요청·응답 구조를 따로 관리할 수 있다.

---

3. Request DTO와 Response DTO

Request DTO: 클라이언트에서 들어오는 데이터를 받는 객체

Response DTO: 클라이언트에 내보낼 데이터를 담는 객체

Request DTO와 Response DTO를 나누는 이유

1. 데이터를 받을 때와 보낼 때 필요한 필드가 다르다.
2. 상품을 등록할 때는 아직 id가 만들어지지 않아 Request DTO에 id가 필요하지 않다.
3. 응답할 때는 생성된 id를 보여줘야 하므로 Response DTO에 id를 담는다.

주의할 점

JSON의 key와 DTO의 필드명이 일치하지 않으면 해당 필드에 `null`이 들어올 수 있다.

---

4. DTO와 Entity 변환

`toEntity()`: Request DTO를 Entity로 변환하는 메서드

`from()`: Entity를 Response DTO로 변환하는 메서드

변환 흐름

1. 요청으로 들어온 Request DTO를 `toEntity()`로 Entity로 바꾼다.
2. Repository에 Entity를 저장한다.
3. 조회한 Entity를 `from()`으로 Response DTO로 바꾼다.
4. Response DTO를 클라이언트에 응답한다.

변환 메서드를 DTO 안에 두면 Controller는 필요한 메서드만 호출할 수 있어 코드가 더 깔끔해진다.

---

5. DTO와 Getter

`@Getter`: Response DTO의 값을 읽을 수 있도록 Getter를 만들어주는 Lombok Annotation

응답 흐름

1. Spring이 Response DTO를 JSON으로 변환한다.
2. Getter를 통해 DTO의 필드 값을 읽는다.
3. Getter가 없으면 응답 결과가 빈 객체인 `{ }`로 나올 수 있다.

DTO는 데이터를 운반하는 역할을 하며 계산과 같은 로직은 넣지 않는다.
또한 Entity를 요청과 응답에 직접 노출하지 않고 DTO를 통해 전달한다.

---

6. API 명세서 구성 요소

API 명세서는 API가 어떤 입력을 받고 어떤 결과를 응답하는지 미리 정리하는 문서이다.

param: API가 일을 하기 위해 필요한 입력 변수의 이름

사용자: API를 호출하는 주체나 대상

Query Parameter: 조회 결과를 검색하거나 필터링하는 조건

Headers: 요청에 함께 보내는 정보

Request Body: 요청 본문에 담아 보내는 실제 데이터

Response: API가 반환하는 데이터

Status: 상황에 따라 반환할 상태 코드와 응답

API를 사용하는 대상에 따라 권한과 보여줄 데이터의 범위가 달라질 수 있다.
예를 들어 관리자는 전체 데이터를 보고, 일반 사용자는 자신의 데이터만 볼 수 있다.

---

7. Path Variable과 Query Parameter

Path Variable: 특정 대상을 하나 지정할 때 사용

```text
GET /api/items/3
```

Query Parameter: 검색, 날짜, 페이지, 정렬, 카테고리처럼 조회 결과를 좁힐 때 사용

특정 대상을 지정한다면 Path Variable을 사용하고,
검색이나 필터링 조건이라면 Query Parameter를 사용한다.

---

8. Headers와 Request Body

Headers: 요청에 붙이는 쪽지

`Content-Type`: 서버에 Request Body가 JSON 형식이라는 것을 알려주는 정보

Request Body: 서버에 전달할 실제 데이터를 담는 본문

HTTP Method에 따른 Request Body 사용

1. POST
-> 새로운 데이터를 등록해야 하므로 Request Body가 필요하다.

2. PUT
-> 수정할 내용을 보내야 하므로 Request Body가 필요하다.

3. GET
-> 조회할 대상만 알면 되므로 보통 Request Body를 사용하지 않는다.

4. DELETE
-> 삭제할 대상이 URL에 있으므로 보통 Request Body를 사용하지 않는다.

---

9. Response와 Status

key: JSON에 실제로 나가는 필드 이름

설명: key가 어떤 의미인지 작성하는 부분

타입: 데이터의 형태

옵션: 값에 제약이 있을 때 작성하는 부분

ENUM: 정해진 값만 받을 수 있다는 의미

Nullable: 값이 비어 있을 수 있는지 표시

Status는 어떤 상황에 어떤 상태 코드와 응답을 돌려줄지 정하는 부분이다.
실패 상황을 미리 정하면 프론트엔드에서도 응답에 따라 처리를 나눌 수 있다.

```text
2xx → 성공
4xx → 클라이언트 오류
5xx → 서버 오류
```

---

10. CRUD 계층 구조

Entity: 상품의 id, 이름, 가격, 수량을 담는 객체

Repository: 데이터를 저장, 조회, 수정, 삭제하는 계층

DTO: 계층 사이에서 요청과 응답 데이터를 전달하는 객체

Service: Repository에 저장과 조회를 요청하고 변환과 흐름을 담당하는 계층

Controller: 요청을 받아 Service에 전달하고 결과를 응답하는 계층

작성 순서

1. Entity
2. Repository
3. DTO
4. Service
5. Controller

DB가 없는 상태에서는 `HashMap`을 사용해 데이터를 저장할 수 있다.
상품 CRUD를 위해 등록, 전체 조회, 한 개 조회, 수정, 삭제 기능을 만든다.

데이터가 들어올 때는 Request DTO를 `toEntity()`로 Entity로 바꾸고,
나갈 때는 Entity를 `from()`으로 Response DTO로 바꾼다.

---

### 4. 실습

> 상품을 등록하고 조회, 수정, 삭제할 수 있는 REST API 구현

상품 한 개에 필요한 정보

- id: `Long`
- 이름: `String`
- 가격: `Integer`
- 수량: `Integer`

상품을 등록하는 시점에는 아직 id를 알 수 없기 때문에
id 없이 객체를 만들 수 있는 생성자를 사용한다.

Repository에서는 `HashMap`을 이용해 상품을 저장하고,
Service는 Repository를 통해 CRUD 기능을 실행한다.

구현한 기능

- 상품 등록
- 상품 목록 전체 조회
- 상품 한 개 조회
- 상품 수정
- 상품 삭제

#### REST API 설계

| 기능 | Method | URL | 요청 바디 | 응답 | 상태 코드 |
|---|---|---|---|---|---|
| 목록 조회 | GET | `/api/items` | 없음 | 상품 목록 `[{ id, itemName, price, quantity }]` | `200 OK` |
| 단건 조회 | GET | `/api/items/{id}` | 없음 | 상품 한 개 `{ id, itemName, price, quantity }` | `200 OK` |
| 등록 | POST | `/api/items` | `{ itemName, price, quantity }` | 등록된 상품 `{ id, itemName, price, quantity }` | `200 OK` |
| 수정 | PUT | `/api/items/{id}` | `{ itemName, price, quantity }` | 수정된 상품 `{ id, itemName, price, quantity }` | `200 OK` |
| 삭제 | DELETE | `/api/items/{id}` | 없음 | 없음 | `200 OK` |

화면에서는 `fetch`로 API를 호출하고,
Postman으로도 요청과 응답이 정상적으로 동작하는지 테스트한다.

---

### 5. 느낀 점 & 다음 계획

처음에는 Entity랑 DTO가 둘 다 데이터를 담는 객체라서 왜 따로 만들어야 하는지 헷갈렸는데,
정리하면서 Entity를 그대로 사용하면 보여주면 안 되는 값이 나가거나 필요하지 않은 값까지 받을 수 있다는 것을 알게 되었다.
Request DTO와 Response DTO도 굳이 나눠야 하나 싶었지만,
데이터를 받을 때와 보낼 때 필요한 값이 다르다는 것을 보니까 나누는 이유가 조금은 이해되는 것 같다.

`toEntity()`와 `from()`을 사용해서 DTO와 Entity를 변환하는 부분은 아직 바로 작성하기에는 어렵지만,
들어올 때는 Request DTO에서 Entity로 바꾸고 나갈 때는 Entity에서 Response DTO로 바꾼다는 흐름은 잡힌 것 같다.

Entity부터 Repository, DTO, Service, Controller 순서로 직접 CRUD를 만들어보면서
각 계층이 무슨 일을 하는지 더 익혀야겠다.
Postman으로 등록, 조회, 수정, 삭제 요청을 하나씩 테스트하면서 전체 흐름을 다시 복습해봐야겠다.
