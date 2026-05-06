# 📘 Today I Learned
## 2회차_[JAVA] OOP(클래스와 캡슐화)

### 절차지향 vs 객체지향
|절차지향|객체지향|
|---|---|
|프로그램의 흐름을 순서대로 처리|객체들 간의 상호작용 중심|
|'어떻게?' 중심|'무엇을' 중심|

### 객체지향
상태(State) : 객체가 가지고 있는 데이터<br>
_ex)현재 전원이 켜져있는지 껴져있는지, 볼륨이 몇인지_ 

기능(Methods) : 객체가 할 수 있는 행동<br>
_ex)음악 플레이어가 전원을 키거나 끄거나, 볼륨이 높낮이 조절_ 

#### 오버로딩
```java
void setVolume(int volume) { }
void setVolume(int volume, boolean mute) { }
```
함수 이름은 같으나, 받는 매개 변수가 다름, 호출 시에 매개변수에 따라서 호출 되는 함수가 다름, 기존 함수를 보완하기 위해 사용

#### 생성자
```java
class MusicPlayer {
 int volume;

 MusicPlayer(int volume) {
    this.volume = volume;
 }
}
```
객체가 생성 시 자동으로 실행되는 특수한 함수 **(함수는 아님)**


#### 캡슐화
데이터를 직접 건드리지 못하게, 정해진 방법으로만 사용 가능하게!

#### 접근제어자
캡슐화 위해서 사용
private 이용하여 외부에서 직접 접근 제어<br>
getter나 setter와 같은 메서드를 이용하여서 값 제어


기존 코드
```java
public class MusicPlayerPractice {

    public static void main(String[] args) {

        int volume = 0;
        boolean isOn = false;

        // 음악 플레이어 켜기
        isOn = true;
        System.out.println("음악 플레이어를 시작합니다.");

        // 볼륨 증가
        volume += 1;
        System.out.println("음악 플레이어 볼륨:" + volume);

        volume += 1;
        System.out.println("음악 플레이어 볼륨:" + volume);

        // 볼륨 감소
        volume -= 1;
        System.out.println("음악 플레이어 볼륨:" + volume);

        // 음악 플레이어 상태 확인
        System.out.println("음악 플레이어 상태 확인");
        if (isOn) {
            System.out.println("음악 플레이어 ON, 볼륨:" + volume);
        } else {
            System.out.println("음악 플레이어 OFF, 볼륨:" + volume);
        }

        // 음악 플레이어 끄기
        isOn = false;
        System.out.println("음악 플레이어를 종료합니다.");
    }
}
```
객체지향 방식으로 수정
```java
public class MusicPlayer {
    private int volume = 0;
    private boolean isOn = false;

    void setVolume(int volume){
        this.volume=volume;
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


public class MusicPlayerFin {
    public static void main(String[] args) {
        MusicPlayer player = new MusicPlayer();

        // 음악 플레이어 켜기
        player.on();
        // 볼륨 증가
        player.volumeUp();
        // 볼륨 증가
        player.volumeUp();
        // 볼륨 감소
        player.volumeDown();
        // 음악 플레이어 상태
        player.setVolume(10);
        player.showStatus();
        // 음악 플레이어 끄기
        player.off();
    }
}
```