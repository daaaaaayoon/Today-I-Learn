## ๐จ ํํ๋ฆฟ ๋ฉ์๋ ํจํด์ด๋?

- 'ํ์ ํจํด'์ ํ๋
- ์ด๋ค ์์์ ์ฒ๋ฆฌํ๋ **์ผ๋ถ๋ถ์ ์๋ธ ํด๋์ค๋ก ์บก์ํ**ํด ์ ์ฒด ์ผ์ ์ํํ๋ ๊ตฌ์กฐ๋ ๋ฐ๊พธ์ง ์์ผ๋ฉด์ **ํน์  ๋จ๊ณ ๋ด์์ ์ํํ๋ ๋ด์ญ์ ๋ฐ๊พธ๋ ํจํด**
- **์ ์ฒด์ ์ผ๋ก๋ ๋์ผ**ํ๋ฉด์ **๋ถ๋ถ์ ์ผ๋ก๋ ๋ค๋ฅธ ๊ตฌ๋ฌธ์ผ๋ก ๊ตฌ์ฑ๋ ๋ฉ์๋**์ **์ฝ๋ ์ค๋ณต์ ์ต์ํ ํ  ๋ ์ ์ฉ**
- ์์) ์๋ฃ๋ฅผ ๋ง๋ ๋ค๊ณ  ํ์ ๋,

    - ๊ฐ๊ฐ์ ์ปคํผ์ ํ์ฐจ๋ ์๋ฃ๋ก๋ถํฐ **์ค๋ณต๋๋ ์ฝ๋ ๋ฐ์**

    - <img width="356" alt="แแณแแณแแตแซแแฃแบ 2021-07-28 แแฉแแฎ 10 07 19" src="https://user-images.githubusercontent.com/53184797/127327563-cd053a96-526f-4b12-b3c6-b67886d9db2b.png">

    - **์ค๋ณต๋๋ ์ฝ๋๋ ์์ ์๋ฃ ํด๋์ค**์์ ๊ตฌํํ๊ณ  **๊ฐ๊ฐ์ ์ธ๋ถ๋ ํ์ ํด๋์ค**์์ ๊ตฌํํ์!

    - <img width="515" alt="แแณแแณแแตแซแแฃแบ 2021-07-28 แแฉแแฎ 10 07 25" src="https://user-images.githubusercontent.com/53184797/127327580-e64d6c86-e6ba-4e28-8e28-774d0583cbc1.png">

## ๐จ ํํ๋ฆฟ ๋ฉ์๋ ํจํด ์์

### 1๏ธโฃ ์ฌ๋ฌ ํ์ฌ์ ๋ชจํฐ ์ง์ํ๊ธฐ ์์

์๋ ๋ฒ ์ดํฐ์ ์ฌ์ฉ๋๋ ๋ชจํฐ๋ฅผ ๊ตฌ๋์์ผ๋ณด์.

### [ ํ๋ ๋ชจํฐ๋ฅผ ์ด์ฉํ๋ ์๋ ๋ฒ ์ดํฐ๋ผ๋ฉด? ]

- ์ํ๋ฅผ ๋ํ๋ด๊ธฐ ์ํ **Enum ์๋ฃ**

- <img width="554" alt="แแณแแณแแตแซแแฃแบ 2021-07-28 แแฉแแฎ 10 07 43" src="https://user-images.githubusercontent.com/53184797/127327596-71317cfc-8465-48f1-9175-6088db7a709e.png">

- ๋ฌธ ํด๋์ค
    ```java
    public class Door {
        private DoorStatus doorStatus;

        public Door() { doorStatus = DoorStatus.CLOSED; }
        public DoorStatus getDoorStatus() { return doorStatus; }
        public void open() { doorStatus = DoorStatus.OPENED; }
        public void close() { doorStatus = DoorStatus.CLOSED; }
    }
    ```

- ํ๋ ๋ชจํฐ ํด๋์ค
    ```java
    public class HyundaiMotor {

        private Door door;
        private MotorStatus motorStatus;

        public HyundaiMotor(Door door) {
            this.door = door;
            motorStatus = MotorStatus.STOPPED;
        }

        private void moveHyundaiMotor(Direction direction) {
            // Hyndai ๋ชจํฐ๋ฅผ ๊ตฌ๋์ํด
        }

        public MotorStatus getMotorStatus() { return motorStatus; }
        private void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

        public void move(Direction direction) {
            // ์ด๋ฏธ ๊ตฌ๋ ์ค์ด๋ผ๋ฉด, ์๋ฌด ์์๋ ํ์ง ์๊ณ  return
            MotorStatus motorStatus = getMotorStatus();
            if(motorStatus == MotorStatus.MOVING) return;
            // ๋ฌธ์ด ์ด๋ ค์๋ค๋ฉด, ๋ฌธ์ ๋ซ๊ธฐ
            DoorStatus doorStatus = door.getDoorStatus();
            if(doorStatus == DoorStatus.OPENED) door.close();
            // Hyndai ๋ชจํฐ๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ๋ฐ์ ๋ฐฉํฅ์ผ๋ก ์ด๋์ํด
            moveHyundaiMotor(direction);
            // ๋ชจํฐ ์ํ๋ฅผ ๊ตฌ๋ ์ค์ผ๋ก ๋ณํ์ํค๊ธฐ
            setMotorStatus(MotorStatus.MOVING);
        }

    }
    ```

### [ ๋ฌธ์ ์  : LG๋ชจํฐ๋ฅผ ๊ตฌ๋์ํค๋ ค๋ฉด?? ]

LG ๋ชจํฐ ํด๋์ค ์์ฑ โ ์์ ํ๋ ๋ชจํฐ ํด๋์ค์ ์ค๋ณต ์ฝ๋๊ฐ ๋๋ฌด ๋ง์!

```java
public class LGMotor {

    private Door door;
    private MotorStatus motorStatus;

    public LGMotor(Door door) {
        this.door = door;
        motorStatus = MotorStatus.STOPPED;
    }

    private void moveLGMotor(Direction direction) {
        // LG ๋ชจํฐ๋ฅผ ๊ตฌ๋์ํด
    }

    public MotorStatus getMotorStatus() { return motorStatus; }
    private void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

    public void move(Direction direction) {
        // ์ด๋ฏธ ๊ตฌ๋ ์ค์ด๋ผ๋ฉด, ์๋ฌด ์์๋ ํ์ง ์๊ณ  return
        MotorStatus motorStatus = getMotorStatus();
        if(motorStatus == MotorStatus.MOVING) return;
        // ๋ฌธ์ด ์ด๋ ค์๋ค๋ฉด, ๋ฌธ์ ๋ซ๊ธฐ
        DoorStatus doorStatus = door.getDoorStatus();
        if(doorStatus == DoorStatus.OPENED) door.close();
        // LG ๋ชจํฐ๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ๋ฐ์ ๋ฐฉํฅ์ผ๋ก ์ด๋์ํด
        moveLGMotor(direction); // ๐ ์ด ๋ถ๋ถ์ ์ ์ธํ๊ณ  HyndaiMotor ํด๋์ค์ ๋์ผ => ์ค๋ณต ์ฝ๋
        // ๋ชจํฐ ์ํ๋ฅผ ๊ตฌ๋ ์ค์ผ๋ก ๋ณํ์ํค๊ธฐ
        setMotorStatus(MotorStatus.MOVING);
    }

}
```

### 2๏ธโฃ ํด๊ฒฐ : ์์์ ์ด์ฉํ์ฌ ์ค๋ณต ์ฝ๋๋ฅผ ์ค์ธ ํํ๋ฆฟ ๋ฉ์๋ ํจํด ์ ์ฉ

1. **๊ณตํต๋(์ค๋ณต๋) ์ฝ๋๋ฅผ ๊ฐ์ง ์์ ํด๋์ค ์ ์**
    - ๊ณตํต๋ ์ฝ๋๋ฅผ ๋ฃ์ Motor ํด๋์ค

        ```java
        public abstract class Motor {

            // ๋ณ๊ฒฝ ๊ฐ๋ฅ์ฑ์ด ์๋ ์ ๋ค์ public์ด ์๋ protected๋ก
            protected Door door; // ์์ ํด๋์ค์์ ์ ๊ทผ ๊ฐ๋ฅ
            private MotorStatus motorStatus;

            public Motor(Door door) {
                this.door = door;
                motorStatus = MotorStatus.STOPPED;
            }

            public MotorStatus getMotorStatus() { return motorStatus; }
            protected void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

            public void move(Direction direction) {
                // ์ด๋ฏธ ๊ตฌ๋ ์ค์ด๋ผ๋ฉด, ์๋ฌด ์์๋ ํ์ง ์๊ณ  return
                MotorStatus motorStatus = getMotorStatus();
                if(motorStatus == MotorStatus.MOVING) return;
                // ๋ฌธ์ด ์ด๋ ค์๋ค๋ฉด, ๋ฌธ์ ๋ซ๊ธฐ
                DoorStatus doorStatus = door.getDoorStatus();
                if(doorStatus == DoorStatus.OPENED) door.close();
                // LG ๋ชจํฐ๋ฅผ ํ๋ผ๋ฏธํฐ๋ก ๋ฐ์ ๋ฐฉํฅ์ผ๋ก ์ด๋์ํด
                **moveMotor(direction);
                // ๋ชจํฐ ์ํ๋ฅผ ๊ตฌ๋ ์ค์ผ๋ก ๋ณํ์ํค๊ธฐ
                setMotorStatus(MotorStatus.MOVING);
            }

            protected abstract void moveMotor(Direction direction);

        }
        ```

2. **๊ฐ๊ฐ์ ํด๋์ค๊ฐ ์์๋ฐ๊ณ , ๋ค๋ฅธ ๋ถ๋ถ์ ์ค๋ฒ๋ผ์ด๋ฉํ์ฌ ์์ฑ**
    - ํ๋ ๋ชจํฐ ํด๋์ค
        ```java
        public class HyndaiMotor extends Motor {

            public HyndaiMotor(Door door) {
                super(door);
            }

            @Override
            protected void moveMotor(Direction direction) {
                // Hyndai ๋ชจํฐ๋ฅผ ๊ตฌ๋์ํด
            }

        }
        ```

    - LG ๋ชจํฐ ํด๋์ค
        ```java
        public class LGMotor extends Motor {

            public LGMotor(Door door) {
                super(door);
            }

            @Override
            protected void moveMotor(Direction direction) {
                // LG ๋ชจํฐ๋ฅผ ๊ตฌ๋์ํด
            }

        }
        ```