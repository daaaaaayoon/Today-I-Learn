## ๐จ ์คํธ๋ํฐ์ง ํจํด์ด๋?

- 'ํ์ ํจํด'์ ํ๋
- **ํ์๋ฅผ ํด๋์ค๋ก ์บก์ํ**ํด ๋์ ์ผ๋ก ํ์๋ฅผ ์์ ๋กญ๊ฒ ๋ฐ๊ฟ ์ ์๊ฒ ํด์ฃผ๋ ํจํด
- **์ ๋ต์ ์ฝ๊ฒ ๋ฐ๊ฟ ์ ์๋๋ก** ํด์ฃผ๋ ๋์์ธ ํจํด
- ex) ๊ฒ์ ์บ๋ฆญํฐ๊ฐ ์ฒํ ์ํฉ์ ๋ฐ๋ผ ๊ณต๊ฒฉ์ด๋ ํ๋ํ๋ ๋ฐฉ์์ ๋ฐ๊พธ๊ณ  ์ถ์ ๋
- <img width="403" alt="แแณแแณแแตแซแแฃแบ 2021-07-27 แแฉแแฎ 2 57 16" src="https://user-images.githubusercontent.com/53184797/127103318-85f3f2be-9bf2-4f83-8248-19e140c0ec50.png">

## ๐จ ์คํธ๋ํฐ์ง ํจํด์ ์์

### 1๏ธโฃ ๋ก๋ด ๋ง๋ค๊ธฐ ์์

ํ๊ถ๋ธ์ด ๋ก๋ด๊ณผ ์ํฐ ๋ก๋ด์ ๋ง๋ค์ด๋ณด์.

### [ ์์ ]

๋์ด ๋น์ทํ ๊ธฐ๋ฅ์ด ์์ผ๋๊น ์์์ ์ด์ฉํด์ ๋ง๋ค๋ฉด ๋๊ฒ ๋ค!

- <img width="457" alt="แแณแแณแแตแซแแฃแบ 2021-07-27 แแฉแแฎ 2 57 25" src="https://user-images.githubusercontent.com/53184797/127103327-cd919193-f1c8-496f-a136-925a0358e9b0.png">

- Robot : ์ถ์ ํด๋์ค

    ```java
    public abstract class Robot {
      private String name;
      public Robot(String name) { this.name = name; }
      public String getName() { return name; }
      // ์ถ์ ๋ฉ์๋
      public abstract void attack();
      public abstract void move();
    }
    ```

- TaekwonV, Atom : ์ถ์ ํด๋์ค Robot์ ์์๋ฐ๋ ํด๋์ค

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

- ์คํ ์ฝ๋

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

- ์คํ ๊ฒฐ๊ณผ
    - <img width="425" alt="แแณแแณแแตแซแแฃแบ 2021-07-27 แแฉแแฎ 2 57 43" src="https://user-images.githubusercontent.com/53184797/127103321-bdd0b60d-8fb0-4363-9900-532c06df30e1.png">

### [ ์์ ๋ฌธ์ ์  ]

1. ๊ธฐ์กด ๋ก๋ด์ ํ๋์ ์์ ํ๋ ๊ฒฝ์ฐ
    - ์๊ตฌ์ฌํญ) TaekwonV๋ ๋  ์ ์๊ฒ ํด์ค!

        ```java
        public class TaekwonV extends Robot {
          public TaekwonV(String name) { super(name); }
          public void attack() { System.out.println("I have Missile."); }
          // ๋ฌธ์ ์  1. ์์ ํ๊ธฐ = ๊ธฐ์กด ์ฝ๋์ ๋ด์ฉ์ ๋ณ๊ฒฝํ๋ค = ๊ฐ์ฒด์งํฅ ์์น OCP์ ์๋ฐฐ
        	// ๋ฌธ์ ์  2. ๋  ์ ์๊ฒ ์์  => Atom์ move() ๋ฉ์๋์ ๋ด์ฉ๊ณผ ์ค๋ณต
        	public void move() { System.out.println("I can fly."); }
        }
        ```

2. ์๋ก์ด ๋ก๋ด์ ๋ง๋ค์ด ํ๋์ ์ถ๊ฐ/์์ ํ๋ ๊ฒฝ์ฐ
    - ์๋ก์ด ๋ก๋ด Sungard : ๋ฏธ์ฌ์ผ ๊ธฐ๋ฅ๊ณผ ๊ฑท๊ธฐ ๊ธฐ๋ฅ์ ๊ฐ์ง ๋ก๋ด

        โ ์๋ก ๋ง๋ค์ด์ ๋ ์์ฑํด์ผ๋จ... ๊ธฐ๋ฅ์ด ์์ /์ถ๊ฐ๋๋ค๋ฉด ์ง์  ์์ฑํ๋ฌ๊ฐ์ผํจ

        ```java
        public class Sungard extends Robot {
          public Sungard(String name) { super(name); }
          public void attack() { System.out.println("I have Missile."); } // ์ค๋ณต
          public void move() { System.out.println("I can only walk."); }
        }
        ```

### 2๏ธโฃ ์์ ๋ฌธ์ ์ ํด๊ฒฐ : ์คํธ๋ํฐ์ง ํจํด ์ ์ฉ

### [ ์ ์ฉ ๋ฐฉ๋ฒ ]

**๋ณํ๋๋ ๊ฒ์ด ๋ฌด์์ธ์ง ์ฐพ์ ํ ์ด๋ฅผ ํด๋์ค๋ก ์บก์ํํด์ผํจ**

1. ๋ก๋ด ์์ ์์ **๋ณํ๋๋ ๋ถ๋ถ**์?
    - ๊ณต๊ฒฉ ๋ฐฉ์ : attack()
    - ์ด๋ ๋ฐฉ์ : move()
2. ๊ณต๊ฒฉ ๋ฐฉ์๊ณผ ์ด๋ ๋ฐฉ์์ **์บก์ํ**ํ์.

    **๊ฐ๊ฐ์ ์ธํฐํ์ด์ค๋ฅผ ๋ง๋ค๊ณ  ์ด๋ฅผ ์ค์  ๊ตฌํํ ํด๋์ค๋ฅผ ๋ง๋ค๋ฉด ๋จ.**

### [ ์ ์ฉํ๊ธฐ ]

1. ๊ณต๊ฒฉ ๋ฐฉ์๊ณผ ์ด๋ ๋ฐฉ์ ์บก์ํ (์ธํฐํ์ด์ค)

    ```java
    // ๊ณต๊ฒฉ ๋ฐฉ์ ์ธํฐํ์ด์ค
    public interface AttackStrategy {
        public void attack();
    }
    // ์ด๋ ๋ฐฉ์ ์ธํฐํ์ด์ค
    public interface MoveStrategy {
        public void move();
    }
    ```

2. ์ธํฐํ์ด์ค ๊ตฌํํ๊ธฐ

    ```java
    // ๊ณต๊ฒฉ๋ฐฉ์ ์ธํฐํ์ด์ค ์ค์  ๊ตฌํ ํด๋์ค
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

    // ์ด๋๋ฐฉ์ ์ธํฐํ์ด์ค ์ค์  ๊ตฌํ ํด๋์ค
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

3. Robot ํด๋์ค ๋ง๋ค๊ธฐ

    ```java
    public abstract class Robot {

        private String name;
        private AttackStrategy attackStrategy;
        private MoveStrategy moveStrategy;
        
        public Robot(String name) { this.name = name; }
        public String getName() { return name; }
        public void attack() { attackStrategy.attack(); }
        public void move() { moveStrategy.move(); }
        
    	// ์ ๋ต(ํ๋)์ ๋ณ๊ฒฝํ๊ธฐ ์ํด ์ฐ์ผ setter
        public void setAttackStrategy(AttackStrategy attackStrategy) {
            this.attackStrategy = attackStrategy;
        }
        public void setMoveStrategy(MoveStrategy moveStrategy) {
            this.moveStrategy = moveStrategy;
        }

    }
    ```

4. ๊ตฌ์ฒด์ ์ธ ๋ก๋ด๋ค

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

5. ํด๋ผ์ด์ธํธ์์ ์ฌ์ฉํ๊ธฐ

    ```java
    public class Client {

        public static void main(String[] args) {
            Robot t = new TaekwonV("TaekwonV");
            Robot a = new Atom("Atom");
    				
    		// ์ ๋ต ์ค์ (๋ณ๊ฒฝ)
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

- ๊ฒฐ๊ณผ
    - <img width="435" alt="แแณแแณแแตแซแแฃแบ 2021-07-27 แแฉแแฎ 2 57 33" src="https://user-images.githubusercontent.com/53184797/127103329-8fa50704-6a89-4539-b692-5ffdf8789969.png">