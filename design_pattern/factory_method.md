## 🎨 팩토리 메서드 패턴이란?

- '생성 패턴'의 하나
- 객체 생성 처리를 서브 클래스로 분리해 처리하도록 캡슐화하는 패턴
- ⇒ 즉, **객체의 생성 코드를 별도의 클래스/메서드로 분리**함으로써 **객체 생성의 변화에 대비**
- 특정 기능의 구현은 개발 클래스를 통해 제공되는 것이 바람직한 설계다.
- **팩토리 메서드 패턴의 사용**
    1. 객체 생성을 전담하는 별도의 **Factory 클래스** **이용**
        - 스트래티지 패턴과 싱글턴 패턴 이용
    2. **상속** **이용** : 하위 클래스에서 적합한 클래스의 객체를 생성
        - 스트래티지 패턴, 싱글턴 패턴, 템플릿 메서드 패턴 이용

## 🎨 팩토리 메서드 패턴 예시

### 1️⃣ 엘레베이터 작동을 지원해보자.

### [ 상황 : 엘레베이터의 작동방식 스케줄링 지원 ]

**엘레베이터 스케줄링** : 요청(목적지 층과 방향)을 받았을 때, 여러대의 엘레베이터 중 하나를 선택하는 것

- 엘레베이터 내부 버튼 (Elevator Button) - 사용자가 탄 엘레베이터를 이동
- 엘레베이터 외부 버튼 (Floor Button) - **여러대의 엘레베이터 중 하나를 선택**해야함

**구현해보기**

- ElevatorController 클래스 : 엘레베이터 이동을 책임지는 클래스

    ```java
    // 엘레베이터 이동을 책임지는 ElevatorController
    public class ElevatorController {

        private int id;
        private int curFloor;

        public ElevatorController(int id) {
            this.id = id;
            curFloor = 1;
        }

        public void gotoFloor(int destination) {
            System.out.print("Elevator ["+id+"] Floor : "+curFloor);
            curFloor = destination;
            System.out.println(" ==> " + curFloor);
        }

    }
    ```

- ThroughputScheduler 클래스 : 움직일 엘레베이터를 선택하는 스케줄러

    ```java
    public class ThroughputScheduler {
        public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
            return 0; // 임의 선택
        }
    }
    ```

- ElevatorManager 클래스 : ElevatorController 인스턴스를 생성하고, 엘레베이터를 이동시키는 메서드를 가진 클래스

    ```java
    public class ElevatorManager {

        private List<ElevatorController> controllers;
        private ThroughputScheduler scheduler;

        // 주어진 수만큼의 ElevatorController를 생성
        public ElevatorManager(int controllerCount) {
            // 엘레베이터 이동을 책임지는 ElevatorController 객체 생성
            controllers = new ArrayList<>(controllerCount);
            for(int i=0; i<controllerCount; i++) {
                ElevatorController controller = new ElevatorController(1);
                controllers.add(controller);
            }
    				// 엘레베이터 스케줄링(엘레베이터 선택)하기 위한 ThroughputScheduler 객체 생성
    				scheduler = new ThroughputScheduler();
        }

        // 요청에 따라 엘레베이터를 선택하고 이동시킴
        void requestElevator(int destination, Direction direction) {
            // ThroughputScheduler를 이용해 엘레베이터를 선택함
            int selectedElevator = scheduler.selectElevator(this, destination, direction);
            // 선택된 엘레베이터를 이동시킴
            controllers.get(selectedElevator).gotoFloor(destination);
        }

    }
    ```

### [ 팩토리 메서드 패턴1. 별도의 Factory 클래스 이용 : 스트래티지, 싱글턴 패턴 ]

- 위 방법의 문제점 : 오후에만 ThroughputScheduler를 쓰고 오전에는 다른 스케줄러를 사용하고 싶다면?
- ⇒ 전략을 쉽게 바꿀 수 있는 스트래티지 패턴 사용
1. **스트래티지 패턴 적용**
    - 스케줄러 인터페이스 생성

        ```java
        public interface ElevatorScheduler {
            public int selectElevator(ElevatorManager manager, int destination, Direction direction);
        }
        ```

    - 시간에 따라 다른 스케줄러 적용

        ```java
        public class ElevatorManager {

            private List<ElevatorController> controllers;

            // 주어진 수만큼의 ElevatorController를 생성
            public ElevatorManager(int controllerCount) {
                // 엘레베이터 이동을 책임지는 ElevatorController 객체 생성
                controllers = new ArrayList<>(controllerCount);
                for(int i=0; i<controllerCount; i++) {
                    ElevatorController controller = new ElevatorController(1);
                    controllers.add(controller);
                }
            }

            // 요청에 따라 엘레베이터를 선택하고 이동시킴
            void requestElevator(int destination, Direction direction) {

                // 스트래티지 패턴 적용 !!!

                // 인터페이스
                ElevatorScheduler scheduler;

                // 오전에는 ResponseTimeScheduler 오후에는 ThroughputScheduler
                int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY); // 0~23
                if(hour < 12) {
                    scheduler = new ResponseTimeScheduler();
                } else {
                    scheduler = new ThroughputScheduler();
                }
        				
        				// 시간에 따라 선택된 스케줄러가 적용됨
                int selectedElevator = scheduler.selectElevator(this, destination, direction);
                // 선택된 엘레베이터를 이동시킴
                controllers.get(selectedElevator).gotoFloor(destination);
            }

        }
        ```

    - 다른 문제점 : 다른 스케줄러를 적용하고 싶다면, **requestElevator의 메서드를 수정해야하는 일이 발생**
    - ⇒ 클래스의 생성 작업을 별도의 클래스/메서드로 분리시키자

2. **별도의 클래스/메서드로 분리**
    - 스케줄링 전략을 변경할 enum 자료형

        ```java
        public enum SchedulingStrategyID { RESPONSE_TIME, THROUGHPUT, DYNAMIC }
        ```

    - SchedulerFactory : 스케줄링 전략에 맞는 객체를 생성하는 메서드를 가진 클래스

        ```java
        public class SchedulerFactory {
        		// 스케줄링 전략에 맞는 객체를 생성
            public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID){
                ElevatorScheduler scheduler = null;
                switch (strategyID) {
                    case RESPONSE_TIME: // 대기 시간 최소화 전략
                        scheduler = new ResponseTimeScheduler();
                        break;
                    case THROUGHPUT:    // 처리량 최대화 전략
                        scheduler = new ThroughputScheduler();
                        break;
                    case DYNAMIC: {     // 시간에 따라 전략 변경
                        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
                        if(hour < 12) {
                            scheduler = new ResponseTimeScheduler();
                        }
                        else{
                            scheduler = new ThroughputScheduler();
                        }
                        break;
                    }
                }
                return scheduler;
            }
        }
        ```

    - ElevatorManager의 requestElevator의 코드 변경

        ⇒ 이제 **스케줄러를 가져올땐 ShedulerFactory 클래스의 getScheduler() 메서드를 호출**하면 됨. (클래스/메서드의 생성을 분리)

        ```java
        public class ElevatorManager {
            private List<ElevatorController> controllers;
            private SchedulingStrategyID strategyID;

            public ElevatorManager(int controllerCount, SchedulingStrategyID strategyID){
                controllers = new ArrayList<ElevatorController>(controllerCount);
                for(int i = 0; i < controllerCount; i++){
                    ElevatorController controller = new ElevatorController(i);
                    controllers.add(controller);
                }
                this.strategyID = strategyID;
            }
        		// 실행 중에 다른 스케줄링 전략으로 지정 가능
            public void setStrategyID(SchedulingStrategyID strategyID){
                this.strategyID = strategyID;
            }
        		
            public void requestElevator(int destination, Direction direction){
                // 주어진 전략 ID에 해당되는 ElevatorScheduler로 변경
                ElevatorScheduler scheduler = SchedulerFactory.getScheduler(strategyID);
                System.out.println(scheduler);
                int selectedElevator = scheduler.selectElevator(this, destination, direction);
                controllers.get(selectedElevator).gotoFloor(destination);
            }
        }
        ```

    - 실행
        - Client

            ```java
            public class Client {
              public static void main(String[] args) {
                ElevatorManager emWithResponseTimeScheduler = new ElevatorManager(2, SchedulingStrategyID.RESPONSE_TIME);
                emWithResponseTimeScheduler.requestElevator(10, Direction.UP);

                ElevatorManager emWithThroughputScheduler = new ElevatorManager(2, SchedulingStrategyID.THROUGHPUT);
                emWithThroughputScheduler.requestElevator(10, Direction.UP);

                ElevatorManager emWithDynamicScheduler = new ElevatorManager(2, SchedulingStrategyID.DYNAMIC);
                emWithDynamicScheduler.requestElevator(10, Direction.UP);
              }
            }
            ```

        - 출력 결과

            : 서로 다른 엘레베이터 선택

            - <img width="632" alt="스크린샷 2021-07-29 오후 4 56 39" src="https://user-images.githubusercontent.com/53184797/127454000-731f7996-6444-436b-9887-c3b1c3fa352d.png">


    - 개선할 점 : 여러번 스케줄러를 생성하지 않고 한 번 생성하여 계속해서 사용하는 것이 바람직
    - ⇒ 싱글턴 패턴을 적용
3. **싱글턴 패턴 적용**
    - ThroughputScheduler

        ```java
        public class ThroughputScheduler implements ElevatorScheduler {

            private static ElevatorScheduler scheduler;

            private ThroughputScheduler() {}

            public static ElevatorScheduler getInstance() {
                if(scheduler == null) {
                    scheduler = new ThroughputScheduler();
                }
                return scheduler;
            }

            @Override
            public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
                return 0;
            }
        }
        ```

    - ResponseTimeScheduler

        ```java
        public class ResponseTimeScheduler implements ElevatorScheduler {

            private static ElevatorScheduler scheduler;

            private ResponseTimeScheduler() {}

            public static ElevatorScheduler getInstance() {
                if(scheduler == null) {
                    scheduler = new ResponseTimeScheduler();
                }
                return scheduler;
            }

            @Override
            public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
                return 1;
            }
        }
        ```

    - SchedulerFactory

        ```java
        public class SchedulerFactory {
            public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID){
                ElevatorScheduler scheduler = null;
                switch (strategyID) {
                    case RESPONSE_TIME:
                        scheduler = ResponseTimeScheduler.getInstance();
                        break;
                    case THROUGHPUT:
                        scheduler = ThroughputScheduler.getInstance();
                        break;
                    case DYNAMIC: {
                        int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
                        if(hour < 12) {
                            scheduler = ResponseTimeScheduler.getInstance();
                        }
                        else{
                            scheduler = ThroughputScheduler.getInstance();
                        }
                        break;
                    }
                }
                return scheduler;
            }
        }
        ```

    - 실행 : 단 1개의 ThroughputScheduler와 ResponseTimeScheduler 객체를 사용

        - <img width="654" alt="스크린샷 2021-07-29 오후 4 56 52" src="https://user-images.githubusercontent.com/53184797/127454027-c36b86dc-4cb7-4d36-aae1-0dd369f78b9f.png">

2. 결론
    - 객체 생성을 전담하는 별도의 Factory 클래스를 분리하여 객체 생성의 변화에 대비할 수 있다.
    - 이 방법은 스트래티지 패턴과 싱글턴 패턴을 이용하여 팩토리 메서드 패턴을 적용한다.