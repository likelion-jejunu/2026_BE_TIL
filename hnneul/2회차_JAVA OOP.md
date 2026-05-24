# 📘 Today I Learned

### 1. 오늘 배운 내용

- 절차 지향 프로그래밍과 객체 지향 프로그래밍의 차이 학습함
- 객체의 구성 요소(상태, 기능) 이해함
- 절차 지향 코드를 객체 지향으로 변경하는 과정 실습함
- 메서드 분리와 클래스 분리의 필요성 학습함
- 오버로딩과 생성자 개념 학습함
- 캡슐화와 접근 제어자 이해함
- Getter, Setter, this 키워드 학습함

---

### 2. 핵심 정리 (내 언어로)

1. 절차 지향 vs 객체 지향
   절차 지향은 프로그램의 실행 흐름(어떻게)에 집중하는 방식
   객체 지향은 객체의 역할과 상호작용(무엇을)에 집중하는 방식 

2. 객체의 구성
   객체는 상태와 기능으로 이루어짐

- 상태: 객체의 현재 값 (전원 상태, 볼륨 등)
- 기능: 객체가 수행할 수 있는 행동 (켜기, 끄기, 볼륨 조절 등) 

3. 절차 지향 코드의 문제점
    1) 기능이 추가될수록 코드가 계속 길어지고 복잡해짐
    2) 변수를 직접 수정할 수 있어 잘못된 값이 들어갈 수 있음
    3) 객체가 여러 개일 경우 동일한 변수를 반복해서 사용해야 함 

4. 객체 지향으로 변경하는 이유
   상태와 기능을 하나의 객체로 묶어서 객체가 자신의 상태를 직접 관리하도록 하기 위함임
   기존에는 외부에서 데이터를 변경했지만, 객체 지향에서는 객체 내부 메서드를 통해서만 상태를 변경함

5. 캡슐화
   : 데이터를 외부에서 직접 접근하지 못하게 하고, 정해진 방법을 통해서만 접근하도록 만드는 것
   -> 이를 통해 잘못된 값이 들어가는 것을 방지하고 안정성을 높일 수 있음

6. 접근 제어자 
    : 이 변수나 메서드에 누가 접근할 수 있는지
    [private]
        : 외부에서 변수에 직접 접근하지 못하도록 만드는 접근 제어자
            - 같은 클래스 내부에서만 접근 가능
            - 변수는 숨김
            - 메서드를 통해서만 값을 변경하도록 설계

7. Getter / Setter
- Getter: 값을 가져오는 메서드
- Setter: 값을 설정하는 메서드
  직접 접근 대신 메서드를 사용하여 데이터를 안전하게 관리함

8. this 키워드
    : 객체 안 변수라는 의미
   메서드의 매개변수와 클래스 변수 이름이 같을 때 이를 구분하기 위해 사용

---

### 3. 실습 / 과제 / 결과물

> 객체 지향으로 변경된 코드

```java
// class MusicPlayer

package oop.prac1.music_final;

public class MusicPlayer {

    // private으로 설정해서 외부에서 직접 수정하지 못하게 함
    private int volume = 0;
    private boolean isOn = false;

    // 외부에서 전달받을 값으로 볼륨 설정
    void setVolume(int volume){
        this.volume = volume;
        //this -> 객체 안 변수라는 의미
    }

    void on(){
        isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");
    }

    void off(){
        isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");
    }

    void volumeUp(){
        volume++;
        System.out.println("음악 플레이어 볼륨: "+volume);
    }

    void volumeDown(){
        volume--;
        System.out.println("음악 플레이어 볼륨: "+volume);
    }

    void showStatus(){
        System.out.println("음악 플레이어 상태 확인");
        if(isOn){
            System.out.println("음악 플레이어 ON, 볼륨: "+volume);
        }else{
            System.out.println("음악 플레이어 OFF");
        }
    }
}

```

```java
// main

package oop.prac1.music_final;

public class MusicPlayerFin {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();

        // 음악 플레이어 켜기
        player.on();

        player.setVolume(10);

        // 볼륨 증가
        player.volumeUp();
        // 볼륨 증가
        player.volumeUp();
        // 볼륨 감소
        player.volumeDown();
        // 음악 플레이어 상태
        player.showStatus();
        // 음악 플레이어 끄기
        player.off();
    }
}

```

- 핵심 변화

    1. 상태와 기능을 하나의 클래스에 묶은 구조임
    2. 외부에서 직접 값을 수정하지 못하도록 제한한 구조임
    3. 객체가 자신의 상태를 직접 관리하도록 변경된 구조임

---

### 4. 느낀 점 & 다음 계획

객체 지향이란걸 수업 때 들어서 알고 있었는데 이게 정확히 어떤 구조로 되어있고, 작동되는지 잘 몰랐는데 이번 세션을 통해서 객체 구조에대해서 잘 파악할 수 있었다.
변수를 외부에서 직접 수정할 수 있는 구조가 위험하다는 것을 느꼈고, 캡슐화를 사용하는 이유도 자연스럽게 이해되었다. 

코드를 주고 이걸 객체로 바꿔보라고 했을 때, 정확하고 빠르게 고칠 순 바꿀 수는 없지만 어떤 식으로 상태와 기능을 나누고 객체로 묶어야 하는지에 대한 방향성은 잡힌 것 같다. 

앞으로 다양한 예제들을 직접 객체 지향방식으로 바꿔가며 코드를 더 많이 연습하면서 익숙해지도록 노력해봐야겠다.