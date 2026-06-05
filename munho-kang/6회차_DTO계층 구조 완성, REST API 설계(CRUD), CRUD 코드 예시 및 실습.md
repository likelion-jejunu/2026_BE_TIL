1. 오늘 배운 것 3줄 요약 <br>
git 브렌치, merge, 충돌<br>
클라이언트부터 DB전까지는 "DTO", DB안에서는 "Entity"!!<br>
어떤 순서로 만들어야 할까? 안쪽(데이터)부터 바깥(입구)으로!!
Entity->Repository->DTO->Service->Controller <br>
<br>
<br>
2. 오늘 막혔던 것 or 아직 이해가 안 가는 것<br>
DTO를 Entity로 Entity를 DTO로 어떻게 변환시킬까?? --> DTO, Entity안에 함수(.toEntity, .from)를 사용하여 바꾼다~


<br>
<br>
3. Rest API 설계 - 오늘 만든 5개 엔드포인트 표로 정리하기 <br>

| 기능 | Method | URL | 요청 바디 | 응답 | 성공 |
| :--- | :---: | :--- | :--- | :--- | :---: |
| 목록 조회 | **GET** | `/api/items` | 없음 | 상품 목록(배열) | 200 |
| 단건 조회 | **GET** | `/api/items/{id}` | 없음 | 상품 1개 | 200 |
| 등록 | **POST** | `/api/items` | `{ itemName, price, quantity }` | 등록된 상품 | 201 |
| 수정 | **PUT** | `/api/items/{id}` | `{ itemName, price, quantity }` | 수정된 상품 | 200 |
| 삭제 | **DELETE** | `/api/items/{id}` | 없음 | 없음 | 204 |
