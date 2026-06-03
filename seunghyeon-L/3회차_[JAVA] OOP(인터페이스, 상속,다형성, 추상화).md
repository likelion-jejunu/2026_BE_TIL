# 📘 Today I Learned
## 2회차_[JAVA] OOP(클래스와 캡슐화)

### 상속
기존 클래스의 변수나 함수가 새로운 클래스에도 필요하다면?
새롭게 코드를 짜야할까?<br>
no 그냥 상속 하면 됨
```java
class 자식 extends 부모 { }
```

그럼 부모한테 물려받은 메서드를 자식에서 조금 다르게 변형하고 싶다면?<br>
->오버라이딩
```java
class Animal {
 void eat() { System.out.println("먹는다"); }
}


class Dog extends Animal {
 @Override
 void eat() { System.out.println("강아지가 사료를 먹는다"); }
}


class Cat extends Animal {
 @Override
 void eat() { System.out.println("고양이가 생선을 먹는다"); }
}
```
@붙이는 이유는 명시하기 위해서 + 실수를 방지하기 위해서(코드 단에서 오류 뜸)

오버로딩(같은 이름 다른 매개변수)
```java
class Dog extends Animal {
 @Override
 void eat() { System.out.println("사료를 먹는다"); }


 void eat(String food) { System.out.println(food + "를 먹는다"); } ///오버로딩
 void eat(int count) { System.out.println(count + "번 먹는다"); } ///오버로딩
}
```
### 실습2 STEP3에서 코드 에러가 발생하는 이유
Playable인터페이스와 Animal 클래스는 서로를 모름 그것을 상속 받고 보충 받는 실제 rabbit이나 다른 클래스들만이 그들을 앎
zookeeper클래스에서 animal.play나 animal.say등등을 해도 animal클래스에는 playable 메서드가 없기에 이를 알 수 없음, 그래서 ((Playable) animal).play()와 같이 바꿈으로서 다운캐스팅을 통해 컴파일러가 이를 찾을 수 있게 함

### List
순서 있고 중복 허용하는 가변 배열
```java
 List<String> names = new ArrayList<>();
  names.add("토끼");
  names.add("물개");
  names.add("토끼");   // 중복 OK

  System.out.println(names);        // [토끼, 물개, 토끼]
  System.out.println(names.get(0)); // 토끼  ← 인덱스로 접
  ```

#### List 구현체 3가지

  클래스: ArrayList<br>
  내부 구조: 배열<br>
  특징: 가장 많이 씀. 조회 빠름
  ────────────────────────────────────────
  클래스: LinkedList<br>
  내부 구조: 이중 연결 리스트<br>
  특징: 중간 삽입/삭제 빠름<br>
  ────────────────────────────────────────
  클래스: Vector<br>
  내부 구조: 배열<br>
  특징: ArrayList의 옛날 버전 (동기화). 거의 안 씀

### Map
키와 값으로 저장
```java
 Map<String, Integer> age = new HashMap<>();
  age.put("토끼", 2);     // 토끼 → 2살
  age.put("물개", 5);     // 물개 → 5살

  System.out.println(age.get("토끼"));  // 2  ← key로 value 꺼냄
```
  핵심 규칙:
  - key는 중복 불가 (같은 key에 또 put하면 덮어씀)
  - value는 중복 OK
  - 기본적으로 순서 보장 안 함 (HashMap 기준)
