# TIL - 멋사 6회차 세션

## 1. 오늘 배운 것

1. Entity(원본) ↔ DTO(사본) : 데이터가 오갈 때 필요한 것만 골라담은 그릇
2. 요청 흐름 : Browser(상품 추가) → DispatcherServlet(어느 Controller에 할당) → Controller(요청받아서 Service한테 시킴) → Service(로직) → Repository → DB
3. DTO 변환 : RequestDTO →toEntity()→ Entity →from()→ ResponseDTO

## 2. REST API 설계

### API 명세서 항목

| 이름 | 설명 |
| --- | --- |
| 카테고리 | 같은 자원을 다루는 API 분류 |
| Param (파라미터) | 입력 변수 |
| 사용자 | 이 API 누가 호출 ( 관리자는 전체를, 유저는 본인 것만 ) |
| Query Parameter | URL ? key=value1 & key=value2 , 거르는 조건 |
| Headers | Content-Type |
| Request Body | body에 담아 보내는 데이터 |
| Response | key, 설명, 타입, 옵션(제약), Nullable |
| Status | 동작 케이스 정리 |

### 5개 엔드포인트

| 기능 | Method | URL | 요청 바디 | 응답 | 상태코드 |
| --- | --- | --- | --- | --- | --- |
| 목록 조회 | GET | /items | X | 상품 목록 | 200 |
| 단건 조회 | GET | /items/{id} | X | 상품 1개 | 200 |
| 등록 | POST | /items | { name, price, num } | 등록 상품 | 201 |
| 수정 | PUT | /items/{id} | { name, price, num } | 수정 상품 | 200 |
| 삭제 | DELETE | /items/{id} | X | 없음 | 200 |