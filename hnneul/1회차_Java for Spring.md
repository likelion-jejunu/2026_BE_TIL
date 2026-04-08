# 📘 Today I Learned
오늘 배운 내용

1. Framework
: Frame(틀, 규칙) + Work(일, 스프트웨어의 목적)

- Spring Framework를 사용하면
-> 자바 기반의 어플리케이션을 만들 수 있음 + 개발자들은 비즈니스 로직에 집중할 수 있음

2. EJB
: Spring과 같은 프레임워크(2000년대 초)
-> 편리한 어플리케이션 개발이 목적

but, 너무 비쌈 + 비즈니스 로직보다 EJB를 사용하기 위한 코드가 너무 많음 
-> 기존 JAVA의 장점을 살리지 못함

3. spring
EJB의 복잡하으로 인해 개발자들이 고통받고 있을 때 등장
- 비즈니스 로직 & 어플리케이션 기술의 분리
- 객체 지향을 최대한 살려서 개발 가능

구조
![alt text](image.png)

4. IoC(Inversion of Control)
제어의 역전 - 객체의 생성과 생명주기 관리의 주체가 개발자 -> 스프링으로 바뀜

- Dependency 의존관계
의존대상 “B”가 변하면 “A”에 영향을 미친다.

> 적용 전
- 객체를 개발자가 직접 생성
- 필요한 시점에 개발자가 직접 호출
=> 프로그램의 흐름을 전부 개발자가 직접 제어

class MemberRepository { //데이터 저장
    public void save() {
        System.out.println("회원 저장");
    }
}

class MemberService {

    private MemberRepository memberRepository;
    //memberservice 내에서 memberrepository를 사용
    -> 의존관계 발생

    public MemberService() {
        this.memberRepository = new MemberRepository();
    } //-> 저장방식이 바뀌면 memberservice 코드도 같이 수정해야함

    public void register() {
        memberRepository.save();
    }
}

public class Main {
    public static void main(String[] args) {
        MemberService memberService = new MemberService();
        memberService.register();
        //실행 시점 + 흐름 제어 모두 개발자가 담당
    }
}

> 적용 후
class MemberRepository {
    public void save() {
        System.out.println("회원 저장");
    }
}

class MemberService {

    private MemberRepository memberRepository;

    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
    //main내부에서 만든 memberrepository를 전달받아 사용

    public void register() {
        memberRepository.save();
    }
}

public class Main {
    public static void main(String[] args) {

        MemberRepository memberRepository = new MemberRepository();
        //객체를 만드는 책임 -> main으로

        MemberService memberService = new MemberService(memberRepository);
        //DI(의존성 주입)

        memberService.register();
    }
}

5. DI(Dependency Injection)
의존성 주입
: 객체를 직접 만들지 않고 생성자, 세터 등을 통해 외부에서 전달받는 방식

- 생성자 주입: 객체를 만들 때 필요한 값을 처음부터 같이 넣어주는 방식
    필수 값이 항상 보장된다
    ## 실무에서는 생성자 주입을 선호!

    > 핵심
    객체 만들 때 바로 넣어줌
    필수 의존성을 처음부터 보장함

    public MemberService(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
    }

    - 사용
    MemberRepository repo = new MemberRepository();
    MemberService service = new MemberService(repo);

- 세터 주입: 객체를 먼저 만들고 나중에 값을 넣어주는 방식
    > 핵심
    객체를 먼저 만들고, 나중에 따로 넣어줌
    안 넣으면 빠질 수도 있음
    public void setMemberRepository(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
    }

    - 사용
    MemberService service = new MemberService();
    service.setMemberRepository(repo);

    ## 정리
    생성자 주입: 만들 때 바로 넣음
    세터 주입: 만든 뒤 나중에 넣음

    new MemberService(repo);   // 생성자 주입
    service.setMemberRepository(repo);   // 세터 주입

6. Spring Initializr
: Spring 프로젝트의 기본 구조와 설정을 자동으로 만들어주는 도구
    1. Project: 빌드 도구 선택
    2. Language: 개발 언어 선택
    3. Spring Boot: Spring Boot 버전 선택
    4. Dependencies: 프로젝트에서 필요한 기능 추가하는 라이브러리 ㄴ

7. 프로젝트 -> Repository에 올리기
> 터미널
git init
git add .
git commit -m "첫 커밋"
git remote add origin [깃허브 레포 주소]
git push -u origin main



### 느낀 점 & 다음 계획
일단 처음에 들을 때는 IoC DI 자체에 대한 것에 이해를 하나도 못했었는데 이렇게 배운걸 정리하니까 너무너무 어렵지만 그래도 정리가 조금은되는거같다...
왜 의존하는 것인지도 쓰면서 이해했고, 다음에는 더 빨리 복습을 해야겠다고 느꼈다 ㅠㅠ
git commit 하는게 이제는 너무나도 익숙해져버려서 git에 대해서 더 많이 알고싶어졌다 ! ㄴ
