# 📘 Today I Learned

### 1. 오늘 배운 내용 3줄 요약!
- DTO(Data Transfer Object)가 필요한 이유는 Entity를 요청, 응답에 그대로 사용할 경우, 보안, 필드 불일치, DB 의존성 문제가 생겨서이다. 
- 요청이랑 응답에 따라 그릇인 DTO가 달라지는데, 들어올 때는 Request DTO, 나갈 때는 Response DTO로 분리한다.
- REST API 명세서의 작성법을 배우고 상품 CRUE API를 만들어봤다.

### 2. 오늘 막혔던 것 or 아직 이해가 안가는 것
- Request DTO에서는 toEntity()를, Response DTO에서는 from()을 사용하는 것은 알겠는데, 이것을 실제로 코드를 짤 때 바로바로 사용할 수 있을지는 모르겠다. 손에 익을 때까지의 시간이 조금은 더 필요할 것 같다고 느꼈다.
- static이 from() 메서드에 붙어서 사용되는지 조금 더 이해해봐야 할 것 같다.

### 3. REST API 설계 - 오늘 만든 5개 엔드포인트 표로 정리하기
기능        Method  URL       요청바디                   응답                       상태코드
-------------------------------------------------------------------------------
목록 조회    GET    /items       없음                     List<ItemResponse>     200
단건 조회    GET    /items/{id}  없음                     ItemResponse              200
등록          POST   /items       ItemCreatRequest    ItemResponse              201
수정          PUT     /items/{id} ItemUpdateRequest ItemResponse              200
삭제        DELETE  /items/{id}  없음                      없음                          204