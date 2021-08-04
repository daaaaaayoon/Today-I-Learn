## 🎨 템플릿 메서드 패턴이란?

- '행위 패턴'의 하나
- 어떤 작업을 처리하는 **일부분을 서브 클래스로 캡슐화**해 전체 일을 수행하는 구조는 바꾸지 않으면서 **특정 단계 내에서 수행하는 내역을 바꾸는 패턴**
- **전체적으로는 동일**하면서 **부분적으로는 다른 구문으로 구성된 메서드**의 **코드 중복을 최소화 할 때 유용**
- 예시) 음료를 만든다고 했을 때,

    - 각각의 커피와 홍차는 음료로부터 **중복되는 코드 발생**

    - <img width="356" alt="스크린샷 2021-07-28 오후 10 07 19" src="https://user-images.githubusercontent.com/53184797/127327563-cd053a96-526f-4b12-b3c6-b67886d9db2b.png">

    - **중복되는 코드는 상위 음료 클래스**에서 구현하고 **각각의 세부는 하위 클래스**에서 구현하자!

    - <img width="515" alt="스크린샷 2021-07-28 오후 10 07 25" src="https://user-images.githubusercontent.com/53184797/127327580-e64d6c86-e6ba-4e28-8e28-774d0583cbc1.png">

## 🎨 템플릿 메서드 패턴 예시

### 1️⃣ 여러 회사의 모터 지원하기 예시

엘레베이터에 사용되는 모터를 구동시켜보자.

### [ 현대 모터를 이용하는 엘레베이터라면? ]

- 상태를 나타내기 위한 **Enum 자료**

- <img width="554" alt="스크린샷 2021-07-28 오후 10 07 43" src="https://user-images.githubusercontent.com/53184797/127327596-71317cfc-8465-48f1-9175-6088db7a709e.png">

- 문 클래스
    ```java
    public class Door {
        private DoorStatus doorStatus;

        public Door() { doorStatus = DoorStatus.CLOSED; }
        public DoorStatus getDoorStatus() { return doorStatus; }
        public void open() { doorStatus = DoorStatus.OPENED; }
        public void close() { doorStatus = DoorStatus.CLOSED; }
    }
    ```

- 현대 모터 클래스
    ```java
    public class HyundaiMotor {

        private Door door;
        private MotorStatus motorStatus;

        public HyundaiMotor(Door door) {
            this.door = door;
            motorStatus = MotorStatus.STOPPED;
        }

        private void moveHyundaiMotor(Direction direction) {
            // Hyndai 모터를 구동시킴
        }

        public MotorStatus getMotorStatus() { return motorStatus; }
        private void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

        public void move(Direction direction) {
            // 이미 구동 중이라면, 아무 작업도 하지 않고 return
            MotorStatus motorStatus = getMotorStatus();
            if(motorStatus == MotorStatus.MOVING) return;
            // 문이 열려있다면, 문을 닫기
            DoorStatus doorStatus = door.getDoorStatus();
            if(doorStatus == DoorStatus.OPENED) door.close();
            // Hyndai 모터를 파라미터로 받은 방향으로 이동시킴
            moveHyundaiMotor(direction);
            // 모터 상태를 구동 중으로 변화시키기
            setMotorStatus(MotorStatus.MOVING);
        }

    }
    ```

### [ 문제점 : LG모터를 구동시키려면?? ]

LG 모터 클래스 작성 ⇒ 위의 현대 모터 클래스와 중복 코드가 너무 많음!

```java
public class LGMotor {

    private Door door;
    private MotorStatus motorStatus;

    public LGMotor(Door door) {
        this.door = door;
        motorStatus = MotorStatus.STOPPED;
    }

    private void moveLGMotor(Direction direction) {
        // LG 모터를 구동시킴
    }

    public MotorStatus getMotorStatus() { return motorStatus; }
    private void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

    public void move(Direction direction) {
        // 이미 구동 중이라면, 아무 작업도 하지 않고 return
        MotorStatus motorStatus = getMotorStatus();
        if(motorStatus == MotorStatus.MOVING) return;
        // 문이 열려있다면, 문을 닫기
        DoorStatus doorStatus = door.getDoorStatus();
        if(doorStatus == DoorStatus.OPENED) door.close();
        // LG 모터를 파라미터로 받은 방향으로 이동시킴
        moveLGMotor(direction); // 🙈 이 부분을 제외하고 HyndaiMotor 클래스와 동일 => 중복 코드
        // 모터 상태를 구동 중으로 변화시키기
        setMotorStatus(MotorStatus.MOVING);
    }

}
```

### 2️⃣ 해결 : 상속을 이용하여 중복 코드를 줄인 템플릿 메소드 패턴 적용

1. **공통된(중복된) 코드를 가진 상위 클래스 정의**
    - 공통된 코드를 넣은 Motor 클래스

        ```java
        public abstract class Motor {

            // 변경 가능성이 있는 애들은 public이 아닌 protected로
            protected Door door; // 자식 클래스에서 접근 가능
            private MotorStatus motorStatus;

            public Motor(Door door) {
                this.door = door;
                motorStatus = MotorStatus.STOPPED;
            }

            public MotorStatus getMotorStatus() { return motorStatus; }
            protected void setMotorStatus(MotorStatus motorStatus) { this.motorStatus = motorStatus; }

            public void move(Direction direction) {
                // 이미 구동 중이라면, 아무 작업도 하지 않고 return
                MotorStatus motorStatus = getMotorStatus();
                if(motorStatus == MotorStatus.MOVING) return;
                // 문이 열려있다면, 문을 닫기
                DoorStatus doorStatus = door.getDoorStatus();
                if(doorStatus == DoorStatus.OPENED) door.close();
                // LG 모터를 파라미터로 받은 방향으로 이동시킴
                **moveMotor(direction);
                // 모터 상태를 구동 중으로 변화시키기
                setMotorStatus(MotorStatus.MOVING);
            }

            protected abstract void moveMotor(Direction direction);

        }
        ```

2. **각각의 클래스가 상속받고, 다른 부분은 오버라이딩하여 작성**
    - 현대 모터 클래스
        ```java
        public class HyndaiMotor extends Motor {

            public HyndaiMotor(Door door) {
                super(door);
            }

            @Override
            protected void moveMotor(Direction direction) {
                // Hyndai 모터를 구동시킴
            }

        }
        ```

    - LG 모터 클래스
        ```java
        public class LGMotor extends Motor {

            public LGMotor(Door door) {
                super(door);
            }

            @Override
            protected void moveMotor(Direction direction) {
                // LG 모터를 구동시킴
            }

        }
        ```