# 📘 Today I Learned
### 1회차_Java for Spring

## framework란?<br>
frame + work
일하는 틀을 만들어 놓은 것으로 개발자들이 손수 HTTP처리 로직을 짠다든지, 페이지 라우팅 로직을 짤 필요 없이 비즈니스 로직에만 집중할 수 있게 하는 것

그러나 이러한 틀이 짜져 있는 만큼 틀에 맞추지 않으면 코드가 작동하지 않음
ex)next.js 라우팅을 위한 폴더 구조

## EJB
2000년대 초 자바 프레임워크

but
무거운 서버가 필요하기에 비용이 비쌈
EJB컨테이너라는 특수 서버 위에서만 작동
비즈니스 로직 코드를 짜기 전 EJB를 짜기 위한 코드가 너무 많음

## Spring
*EJB시대의 겨울이 지나가고 Spring이라는 봄이 왔다.*

### 컨테이너 의존성 제거
과거 EJB의 경우 EJB컨테이너가 필수

### 1. POJO(Plain Old Java Object)기반으로 특정 클래스 상속 강제 X
##### EJB 방식
```java
// EJB는 이걸 반드시 implements
public class UserBean implements EntityBean {
    // EntityBean이 요구하는 메서드 전부 구현 강제
    public void ejbActivate() {}
    public void ejbPassivate() {}
    public void ejbRemove() {}
    // 내 비즈니스 로직은 어디에...?
}
```
##### Spring방식-POJO
```java
// 그냥 클래스
public class UserService {
    public User findUser(int id) {
        // 비즈니스 로직만
    }
}
```
## IoC
IoC란?<br>
제어의 역전. 객체 생성/관리 권한을 개발자가 아닌 
Spring이 가져가는 것.

필요한 이유?<br>
개발자가 직접 객체를 new로 만들면 의존하는 클래스가 
바뀔 때 수정할 곳이 너무 많아짐.

어떻게 해결?<br>
Spring이 객체를 대신 만들고 관리해줌.
개발자는 비즈니스 로직에만 집중.

## DI
DI(의존성 주입)란?<br>
객체를 직접 만들지 않고 생성자, 세터 등을 통해 외부에서 전달받는 방식

```java
public class UserService {
    UserRepository repo = new UserRepository();
}
```
의존성이란 위에 코드를 보면 UserService가 작동하려면 UserRespository가 필요하기
에 UserService는 UserRepository에 의존한다고 표현.

```java
// DI 이전 — 내가 직접 만듦
public class UserService {
    UserRepository repo = new UserRepository(); // new로 직접
}

// DI 이후 — 외부에서 받음
public class UserService {
    UserRepository repo;
    
    public UserService(UserRepository repo) {
        this.repo = repo; // 받아서 씀
    }
}
```

## Spring Boot
스프링 생태계가 확장하고 여러 기능들이 추가, 다양한 오픈소스들이 등장하면서 개발자가 직접 해야하는 설정들이 점점 많아짐
->Spring Boot등장!

## Spring Initiallizer
Spring 프로젝트는 초기 설정이 넘 많기에 이를 자동으로 생성해 주는 것

빌드 도구 선택 기준
||gradle|maven|
|-----|-----|-----|
|설정 파일|build.gradle|pom.xml|
|문법|코드|XML|
|속도|빠름|느림|
<br>

# 🔧 Windows IntelliJ 트러블슈팅

## 1. 망치 아이콘(Build) 이 없다

### 문제
맥북 IntelliJ에는 망치 아이콘이 기본으로 보이는데
Windows IntelliJ에는 보이지 않음

### 원인
Gradle 프로젝트로 인식되지 않아서 발생

### 해결
`build.gradle` 우클릭 → Link Gradle Project 클릭

---

## 2. git 기본 브랜치가 master

### 문제
맥북은 git 기본 브랜치가 `main`인데
Windows는 `master`가 기본값

### 원인
git 버전 또는 초기 설정 차이
macOS는 최신 git 설정으로 기본값이 `main`이지만
Windows는 버전에 따라 `master`가 기본값인 경우가 많음

### 해결
앞으로 새로 만드는 레포 기본값 변경:
```bash
git config --global init.defaultBranch main
```

현재 레포 브랜치 이름 변경:
```bash
git branch -m master main
git push origin main
```