📘 Today I Learned
1. 오늘 배운 내용
- Annotation의 개념과 Spring에서의 역할(컴포넌트 스캔 / 요청 처리 / DI(의존성 주입))
- @Controller 과 @RestController의 차이
- @PathVariable, @RequestParam, @RequestBody 사용법
- REST API, URI
- API 명세서 작성법

2. 핵심 정리 (내 언어로)
- What is Annotation??
@, 스프링이 이것을 읽고 Bean 등록, 요청 매핑, DI 처리함

어노테이션의 역할 
1. 컴포넌트 스캔(by @RestController, @Service, @Repository) 
2. 요청 처리(by @GetMapping, @PostMapping, @PathVariable, @RequestBody) 
3. DI(의존성 주입)(by @RequiredArgsConstructor, @Autowired)

- @PathVariable vs @RequestParam vs @RequestBody
@PathVariable - URL 경로
@RequestParam - 쿼리스트링
@RequestBody - HTTP Body

- Controller / Service / Repository 계층 분리
Controller -> 요청 받고 응답 돌려주는 역할 (홀 직원)
Service  -> 비즈니스 로직 처리 (주방장)
Repository -> DB 접근 전담 (창고 담당)

- REST API URI 규칙
/todos와 같이 동사 X, 소문자
/user-profile와 같이 _ 대신 -
/users/1와 같이 확장자 X
/todos와 같이 마지막 슬래시 X

3. 실습 / 과제 / 결과물
링크 - https://github.com/likelion-jejunu/2026_BE_Homework/pull/39

4. 느낀 점 & 다음 계획
- 어노테이션을 그냥 붙이면 되는구나라고 생각한 것보다 훨씬 더 많은 역할을 한다는 것을 보고 기술의 세계는 넓고 나는 얕구나를 배웠다.
- Controller가 비즈니스 로직을 한다면 훨씬 더 정신없어질 것이라는 걸 알았다. 역할 분리가 실제로 유지보수에 얼마나 중요한지 더 알게된 느낌이다. 
- 불과 인텔리제이를 만지고 자바를 배운지 2개월 정도인데, 뭔가 점점 엔지니어들의 관점을 이해하게 되는 것 같아서 스스로가 뿌듯한 느낌...!! 아직도 부족하고 부족한 것을 잘 알지만 조금 더!
