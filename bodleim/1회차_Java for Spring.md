# TIL - Spring 1주차 (IoC & Dependency)

## 오늘 배운 내용
- Spring Framework 개념
- Dependency (의존관계)
- IoC (제어의 역전)
- DI (의존성 주입)

---

## 1. Dependency (의존관계)

### 정의
- 객체 A가 객체 B를 사용하면 → A는 B에 의존한다
- B가 변경되면 A도 영향을 받는다

> “의존대상 B가 변하면 A에 영향을 미친다”

### 코드

    class MemberService {
        MemberRepository repository = new MemberRepository();
    }

### 문제점
- 객체 간 결합도가 높아짐
- 구현 변경 시 코드 수정 필요
- 유지보수 어려움

---

## 2. IoC (Inversion of Control)

### 정의
- 객체 생성과 생명주기 관리의 주체가  
  개발자 → Spring으로 변경되는 것

---

### IoC 적용 전

    class MemberService {
        MemberRepository repository = new MemberRepository();
    }

- 개발자가 직접 객체 생성
- 프로그램 흐름 직접 제어

---

### IoC 적용 후

    class MemberService {
        private MemberRepository repository;

        public MemberService(MemberRepository repository) {
            this.repository = repository;
        }
    }

- 객체를 외부에서 전달받음
- Spring이 객체 생성 및 관리

---

## 3. DI (Dependency Injection)

### 정의
- 객체를 직접 생성하지 않고 외부에서 주입받는 방식

---

### 생성자 주입

    public MemberService(MemberRepository repository) {
        this.repository = repository;
    }

장점
- 필수 의존성 보장
- 불변성 유지
- 실무에서 가장 많이 사용

---

### Setter 주입

    public void setRepository(MemberRepository repository) {
        this.repository = repository;
    }

단점
- null 가능성 존재
- 안정성 낮음

---

## 4. IoC vs DI

- IoC = 개념 (제어권을 넘김)
- DI = 구현 방법 (의존성 주입)

---

## 5. Spring 구조

| 계층 | 역할 |
|------|------|
| Controller | 요청 처리 |
| Service | 비즈니스 로직 |
| Repository | 데이터 저장 |
| Entity | 데이터 구조 |

---


