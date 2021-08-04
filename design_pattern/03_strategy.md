## 🎨 스트래티지 패턴이란?

- '행위 패턴'의 하나
- **행위를 클래스로 캡슐화**해 동적으로 행위를 자유롭게 바꿀 수 있게 해주는 패턴
- **전략을 쉽게 바꿀 수 있도록** 해주는 디자인 패턴
- ex) 게임 캐릭터가 처한 상황에 따라 공격이나 행동하는 방식을 바꾸고 싶을 때
- <img width="403" alt="스크린샷 2021-07-27 오후 2 57 16" src="https://user-images.githubusercontent.com/53184797/127103318-85f3f2be-9bf2-4f83-8248-19e140c0ec50.png">

## 🎨 스트래티지 패턴의 예시

### 1️⃣ 로봇 만들기 예시

태권브이 로봇과 아톰 로봇을 만들어보자.

### [ 상속 ]

둘이 비슷한 기능이 있으니까 상속을 이용해서 만들면 되겠다!

- <img width="457" alt="스크린샷 2021-07-27 오후 2 57 25" src="https://user-images.githubusercontent.com/53184797/127103327-cd919193-f1c8-496f-a136-925a0358e9b0.png">

- Robot : 추상 클래스

    ```java
    public abstract class Robot {
      private String name;
      public Robot(String name) { this.name = name; }
      public String getName() { return name; }
      // 추상 메서드
      public abstract void attack();
      public abstract void move();
    }
    ```

- TaekwonV, Atom : 추상 클래스 Robot을 상속받는 클래스

    ```java
    public class TaekwonV extends Robot {
      public TaekwonV(String name) { super(name); }
      public void attack() { System.out.println("I have Missile."); }
      public void move() { System.out.println("I can only walk."); }
    }
    ```

    ```java
    public class Atom extends Robot {
      public Atom(String name) { super(name); }
      public void attack() { System.out.println("I have strong punch."); }
      public void move() { System.out.println("I can fly."); }
    }
    ```

- 실행 코드

    ```java
    public class Client {
        public static void main(String[] args) {

            Robot t = new TaekwonV("TaekwonV");
            Robot a = new Atom("Atom");

            System.out.println("I am "+t.getName());
            t.attack();
            t.move();

            System.out.println("I am "+a.getName());
            a.attack();
            a.move();

        }
    }
    ```

- 실행 결과
    - <img width="425" alt="스크린샷 2021-07-27 오후 2 57 43" src="https://user-images.githubusercontent.com/53184797/127103321-bdd0b60d-8fb0-4363-9900-532c06df30e1.png">

### [ 상속 문제점 ]

1. 기존 로봇의 행동을 수정하는 경우
    - 요구사항) TaekwonV도 날 수 있게 해줘!

        ```java
        public class TaekwonV extends Robot {
          public TaekwonV(String name) { super(name); }
          public void attack() { System.out.println("I have Missile."); }
          // 문제점 1. 수정하기 = 기존 코드의 내용을 변경한다 = 객체지향 원칙 OCP에 위배
        	// 문제점 2. 날 수 있게 수정 => Atom의 move() 메소드의 내용과 중복
        	public void move() { System.out.println("I can fly."); }
        }
        ```

2. 새로운 로봇을 만들어 행동을 추가/수정하는 경우
    - 새로운 로봇 Sungard : 미사일 기능과 걷기 기능을 가진 로봇

        ⇒ 새로 만들어서 또 작성해야됨... 기능이 수정/추가된다면 직접 작성하러가야함

        ```java
        public class Sungard extends Robot {
          public Sungard(String name) { super(name); }
          public void attack() { System.out.println("I have Missile."); } // 중복
          public void move() { System.out.println("I can only walk."); }
        }
        ```

### 2️⃣ 상속 문제의 해결 : 스트래티지 패턴 적용

### [ 적용 방법 ]

**변화되는 것이 무엇인지 찾은 후 이를 클래스로 캡슐화해야함**

1. 로봇 예제에서 **변화되는 부분**은?
    - 공격 방식 : attack()
    - 이동 방식 : move()
2. 공격 방식과 이동 방식을 **캡슐화**하자.

    **각각의 인터페이스를 만들고 이를 실제 구현한 클래스를 만들면 됨.**

### [ 적용하기 ]

1. 공격 방식과 이동 방식 캡슐화 (인터페이스)

    ```java
    // 공격 방식 인터페이스
    public interface AttackStrategy {
        public void attack();
    }
    // 이동 방식 인터페이스
    public interface MoveStrategy {
        public void move();
    }
    ```

2. 인터페이스 구현하기

    ```java
    // 공격방식 인터페이스 실제 구현 클래스
    public class MissileStrategy implements AttackStrategy {
        @Override
        public void attack() {
            System.out.println("I have Missile.");
        }
    }
    public class PunchStrategy implements AttackStrategy {
        @Override
        public void attack() {
            System.out.println("I have Strong punch.");
        }
    }

    // 이동방식 인터페이스 실제 구현 클래스
    public class FlyingStrategy implements MoveStrategy {
        @Override
        public void move() {
            System.out.println("I can fly.");
        }
    }
    public class WalkingStrategy implements MoveStrategy {
        @Override
        public void move() {
            System.out.println("I can only walk.");
        }
    }
    ```

3. Robot 클래스 만들기

    ```java
    public abstract class Robot {

        private String name;
        private AttackStrategy attackStrategy;
        private MoveStrategy moveStrategy;
        
        public Robot(String name) { this.name = name; }
        public String getName() { return name; }
        public void attack() { attackStrategy.attack(); }
        public void move() { moveStrategy.move(); }
        
    	// 전략(행동)을 변경하기 위해 쓰일 setter
        public void setAttackStrategy(AttackStrategy attackStrategy) {
            this.attackStrategy = attackStrategy;
        }
        public void setMoveStrategy(MoveStrategy moveStrategy) {
            this.moveStrategy = moveStrategy;
        }

    }
    ```

4. 구체적인 로봇들

    ```java
    public class Atom extends Robot {
        public Atom(String name) {
            super(name);
        }
    }
    public class TaekwonV extends Robot {
        public TaekwonV(String name) {
            super(name);
        }
    }
    ```

5. 클라이언트에서 사용하기

    ```java
    public class Client {

        public static void main(String[] args) {
            Robot t = new TaekwonV("TaekwonV");
            Robot a = new Atom("Atom");
    				
    		// 전략 설정(변경)
            t.setMoveStrategy(new WalkingStrategy());
            t.setAttackStrategy(new MissileStrategy());
            a.setMoveStrategy(new FlyingStrategy());
            a.setAttackStrategy(new PunchStrategy());

            System.out.println("I am " + t.getName());
            t.move();
            t.attack();

            System.out.println("I am " + a.getName());
            a.move();
            a.attack();
        }

    }
    ```

- 결과
    - <img width="435" alt="스크린샷 2021-07-27 오후 2 57 33" src="https://user-images.githubusercontent.com/53184797/127103329-8fa50704-6a89-4539-b692-5ffdf8789969.png">