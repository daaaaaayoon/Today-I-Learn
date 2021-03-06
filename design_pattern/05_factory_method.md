## π¨ ν©ν λ¦¬ λ©μλ ν¨ν΄μ΄λ?

- 'μμ± ν¨ν΄'μ νλ
- κ°μ²΄ μμ± μ²λ¦¬λ₯Ό μλΈ ν΄λμ€λ‘ λΆλ¦¬ν΄ μ²λ¦¬νλλ‘ μΊ‘μννλ ν¨ν΄
- β μ¦, **κ°μ²΄μ μμ± μ½λλ₯Ό λ³λμ ν΄λμ€/λ©μλλ‘ λΆλ¦¬**ν¨μΌλ‘μ¨ **κ°μ²΄ μμ±μ λ³νμ λλΉ**
- νΉμ  κΈ°λ₯μ κ΅¬νμ κ°λ° ν΄λμ€λ₯Ό ν΅ν΄ μ κ³΅λλ κ²μ΄ λ°λμ§ν μ€κ³λ€.
- **ν©ν λ¦¬ λ©μλ ν¨ν΄μ μ¬μ©**
    1. κ°μ²΄ μμ±μ μ λ΄νλ λ³λμ **Factory ν΄λμ€** **μ΄μ©**
        - μ€νΈλν°μ§ ν¨ν΄κ³Ό μ±κΈν΄ ν¨ν΄ μ΄μ©
    2. **μμ** **μ΄μ©** : νμ ν΄λμ€μμ μ ν©ν ν΄λμ€μ κ°μ²΄λ₯Ό μμ±
        - μ€νΈλν°μ§ ν¨ν΄, μ±κΈν΄ ν¨ν΄, ννλ¦Ώ λ©μλ ν¨ν΄ μ΄μ©

## π¨ ν©ν λ¦¬ λ©μλ ν¨ν΄ μμ

**π§ μλ λ² μ΄ν° μλμ μ§μν΄λ³΄μ.**

### [ μν© : μλ λ² μ΄ν°μ μλλ°©μ μ€μΌμ€λ§ μ§μ ]

**μλ λ² μ΄ν° μ€μΌμ€λ§** : μμ²­(λͺ©μ μ§ μΈ΅κ³Ό λ°©ν₯)μ λ°μμ λ, μ¬λ¬λμ μλ λ² μ΄ν° μ€ νλλ₯Ό μ ννλ κ²

- μλ λ² μ΄ν° λ΄λΆ λ²νΌ (Elevator Button) - μ¬μ©μκ° ν μλ λ² μ΄ν°λ₯Ό μ΄λ
- μλ λ² μ΄ν° μΈλΆ λ²νΌ (Floor Button) - **μ¬λ¬λμ μλ λ² μ΄ν° μ€ νλλ₯Ό μ ν**ν΄μΌν¨

**κ΅¬νν΄λ³΄κΈ°**

- ElevatorController ν΄λμ€ : μλ λ² μ΄ν° μ΄λμ μ±μμ§λ ν΄λμ€

    ```java
    // μλ λ² μ΄ν° μ΄λμ μ±μμ§λ ElevatorController
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

- ThroughputScheduler ν΄λμ€ : μμ§μΌ μλ λ² μ΄ν°λ₯Ό μ ννλ μ€μΌμ€λ¬

    ```java
    public class ThroughputScheduler {
        public int selectElevator(ElevatorManager manager, int destination, Direction direction) {
            return 0; // μμ μ ν
        }
    }
    ```

- ElevatorManager ν΄λμ€ : ElevatorController μΈμ€ν΄μ€λ₯Ό μμ±νκ³ , μλ λ² μ΄ν°λ₯Ό μ΄λμν€λ λ©μλλ₯Ό κ°μ§ ν΄λμ€

    ```java
    public class ElevatorManager {

        private List<ElevatorController> controllers;
        private ThroughputScheduler scheduler;

        // μ£Όμ΄μ§ μλ§νΌμ ElevatorControllerλ₯Ό μμ±
        public ElevatorManager(int controllerCount) {
            // μλ λ² μ΄ν° μ΄λμ μ±μμ§λ ElevatorController κ°μ²΄ μμ±
            controllers = new ArrayList<>(controllerCount);
            for(int i=0; i<controllerCount; i++) {
                ElevatorController controller = new ElevatorController(1);
                controllers.add(controller);
            }
    				// μλ λ² μ΄ν° μ€μΌμ€λ§(μλ λ² μ΄ν° μ ν)νκΈ° μν ThroughputScheduler κ°μ²΄ μμ±
    				scheduler = new ThroughputScheduler();
        }

        // μμ²­μ λ°λΌ μλ λ² μ΄ν°λ₯Ό μ ννκ³  μ΄λμν΄
        void requestElevator(int destination, Direction direction) {
            // ThroughputSchedulerλ₯Ό μ΄μ©ν΄ μλ λ² μ΄ν°λ₯Ό μ νν¨
            int selectedElevator = scheduler.selectElevator(this, destination, direction);
            // μ νλ μλ λ² μ΄ν°λ₯Ό μ΄λμν΄
            controllers.get(selectedElevator).gotoFloor(destination);
        }

    }
    ```

### [ ν©ν λ¦¬ λ©μλ ν¨ν΄ 1οΈβ£ - λ³λμ Factory ν΄λμ€ μ΄μ© : μ€νΈλν°μ§, μ±κΈν΄ ν¨ν΄ ]

- μ λ°©λ²μ λ¬Έμ μ  : μ€νμλ§ ThroughputSchedulerλ₯Ό μ°κ³  μ€μ μλ λ€λ₯Έ μ€μΌμ€λ¬λ₯Ό μ¬μ©νκ³  μΆλ€λ©΄?
- β μ λ΅μ μ½κ² λ°κΏ μ μλ μ€νΈλν°μ§ ν¨ν΄ μ¬μ©
1. **μ€νΈλν°μ§ ν¨ν΄ μ μ©**
    - μ€μΌμ€λ¬ μΈν°νμ΄μ€ μμ±

        ```java
        public interface ElevatorScheduler {
            public int selectElevator(ElevatorManager manager, int destination, Direction direction);
        }
        ```

    - μκ°μ λ°λΌ λ€λ₯Έ μ€μΌμ€λ¬ μ μ©

        ```java
        public class ElevatorManager {

            private List<ElevatorController> controllers;

            // μ£Όμ΄μ§ μλ§νΌμ ElevatorControllerλ₯Ό μμ±
            public ElevatorManager(int controllerCount) {
                // μλ λ² μ΄ν° μ΄λμ μ±μμ§λ ElevatorController κ°μ²΄ μμ±
                controllers = new ArrayList<>(controllerCount);
                for(int i=0; i<controllerCount; i++) {
                    ElevatorController controller = new ElevatorController(1);
                    controllers.add(controller);
                }
            }

            // μμ²­μ λ°λΌ μλ λ² μ΄ν°λ₯Ό μ ννκ³  μ΄λμν΄
            void requestElevator(int destination, Direction direction) {

                // μ€νΈλν°μ§ ν¨ν΄ μ μ© !!!

                // μΈν°νμ΄μ€
                ElevatorScheduler scheduler;

                // μ€μ μλ ResponseTimeScheduler μ€νμλ ThroughputScheduler
                int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY); // 0~23
                if(hour < 12) {
                    scheduler = new ResponseTimeScheduler();
                } else {
                    scheduler = new ThroughputScheduler();
                }
        				
        				// μκ°μ λ°λΌ μ νλ μ€μΌμ€λ¬κ° μ μ©λ¨
                int selectedElevator = scheduler.selectElevator(this, destination, direction);
                // μ νλ μλ λ² μ΄ν°λ₯Ό μ΄λμν΄
                controllers.get(selectedElevator).gotoFloor(destination);
            }

        }
        ```

    - λ€λ₯Έ λ¬Έμ μ  : λ€λ₯Έ μ€μΌμ€λ¬λ₯Ό μ μ©νκ³  μΆλ€λ©΄, **requestElevatorμ λ©μλλ₯Ό μμ ν΄μΌνλ μΌμ΄ λ°μ**
    - β ν΄λμ€μ μμ± μμμ λ³λμ ν΄λμ€/λ©μλλ‘ λΆλ¦¬μν€μ

2. **λ³λμ ν΄λμ€/λ©μλλ‘ λΆλ¦¬**
    - μ€μΌμ€λ§ μ λ΅μ λ³κ²½ν  enum μλ£ν

        ```java
        public enum SchedulingStrategyID { RESPONSE_TIME, THROUGHPUT, DYNAMIC }
        ```

    - SchedulerFactory : μ€μΌμ€λ§ μ λ΅μ λ§λ κ°μ²΄λ₯Ό μμ±νλ λ©μλλ₯Ό κ°μ§ ν΄λμ€

        ```java
        public class SchedulerFactory {
        		// μ€μΌμ€λ§ μ λ΅μ λ§λ κ°μ²΄λ₯Ό μμ±
            public static ElevatorScheduler getScheduler(SchedulingStrategyID strategyID){
                ElevatorScheduler scheduler = null;
                switch (strategyID) {
                    case RESPONSE_TIME: // λκΈ° μκ° μ΅μν μ λ΅
                        scheduler = new ResponseTimeScheduler();
                        break;
                    case THROUGHPUT:    // μ²λ¦¬λ μ΅λν μ λ΅
                        scheduler = new ThroughputScheduler();
                        break;
                    case DYNAMIC: {     // μκ°μ λ°λΌ μ λ΅ λ³κ²½
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

    - ElevatorManagerμ requestElevatorμ μ½λ λ³κ²½

        β μ΄μ  **μ€μΌμ€λ¬λ₯Ό κ°μ Έμ¬λ ShedulerFactory ν΄λμ€μ getScheduler() λ©μλλ₯Ό νΈμΆ**νλ©΄ λ¨. (ν΄λμ€/λ©μλμ μμ±μ λΆλ¦¬)

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
        		// μ€ν μ€μ λ€λ₯Έ μ€μΌμ€λ§ μ λ΅μΌλ‘ μ§μ  κ°λ₯
            public void setStrategyID(SchedulingStrategyID strategyID){
                this.strategyID = strategyID;
            }
        		
            public void requestElevator(int destination, Direction direction){
                // μ£Όμ΄μ§ μ λ΅ IDμ ν΄λΉλλ ElevatorSchedulerλ‘ λ³κ²½
                ElevatorScheduler scheduler = SchedulerFactory.getScheduler(strategyID);
                System.out.println(scheduler);
                int selectedElevator = scheduler.selectElevator(this, destination, direction);
                controllers.get(selectedElevator).gotoFloor(destination);
            }
        }
        ```

    - μ€ν
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

        - μΆλ ₯ κ²°κ³Ό

            : μλ‘ λ€λ₯Έ μλ λ² μ΄ν° μ ν

            - <img width="632" alt="αα³αα³αα΅α«αα£αΊ 2021-07-29 αα©αα? 4 56 39" src="https://user-images.githubusercontent.com/53184797/127454000-731f7996-6444-436b-9887-c3b1c3fa352d.png">


    - κ°μ ν  μ  : μ¬λ¬λ² μ€μΌμ€λ¬λ₯Ό μμ±νμ§ μκ³  ν λ² μμ±νμ¬ κ³μν΄μ μ¬μ©νλ κ²μ΄ λ°λμ§
    - β μ±κΈν΄ ν¨ν΄μ μ μ©
3. **μ±κΈν΄ ν¨ν΄ μ μ©**
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

    - μ€ν : λ¨ 1κ°μ ThroughputSchedulerμ ResponseTimeScheduler κ°μ²΄λ₯Ό μ¬μ©

        - <img width="654" alt="αα³αα³αα΅α«αα£αΊ 2021-07-29 αα©αα? 4 56 52" src="https://user-images.githubusercontent.com/53184797/127454027-c36b86dc-4cb7-4d36-aae1-0dd369f78b9f.png">

2. κ²°λ‘ 
    - κ°μ²΄ μμ±μ μ λ΄νλ λ³λμ Factory ν΄λμ€λ₯Ό λΆλ¦¬νμ¬ κ°μ²΄ μμ±μ λ³νμ λλΉν  μ μλ€.
    - μ΄ λ°©λ²μ μ€νΈλν°μ§ ν¨ν΄κ³Ό μ±κΈν΄ ν¨ν΄μ μ΄μ©νμ¬ ν©ν λ¦¬ λ©μλ ν¨ν΄μ μ μ©νλ€.


### [ ν©ν λ¦¬ λ©μλ ν¨ν΄ 2οΈβ£ - μμ μ΄μ© : μ€νΈλν°μ§, μ±κΈν΄, ννλ¦Ώ λ©μλ ]

- ν©ν λ¦¬ λ©μλ ν¨ν΄ 2 = ν©ν λ¦¬ λ©μλ ν¨ν΄ 1 + μμ
- μμμ μ΄μ©νλ κ²μ λ­λ€? β ννλ¦Ώ λ©μλ
- μ¦, ν©ν λ¦¬ λ©μλ ν¨ν΄ 2λ μ€νΈλν°μ§, μ±κΈν΄, ννλ¦Ώ λ©μλ ν¨ν΄μ μ μ©ν ν¨ν΄μ΄λ€.
- **μμμ μ΄μ©νμ¬** νμ ν΄λμ€μμ μ ν©ν ν΄λμ€μ κ°μ²΄λ₯Ό μμ±νμ¬ **κ°μ²΄μ μμ± μ½λλ₯Ό λΆλ¦¬**νλ€.

- **μ μ©ν΄λ³΄κΈ°**
    - ElevatorManagerλ₯Ό μΆμ ν΄λμ€λ‘ λ³κ²½ + getScheduler()λ₯Ό μΆμ λ©μλλ‘ λ³κ²½

        ```java
        public abstract class ElevatorManager {

            private List<ElevatorController> controllers;

            public ElevatorManager(int controllerCount){
                controllers = new ArrayList<ElevatorController>(controllerCount);
                for(int i = 0; i < controllerCount; i++){
                    ElevatorController controller = new ElevatorController(i);
                    controllers.add(controller);
                }
            }
        		
        		// ν©ν λ¦¬ λ©μλ : μ€μΌμ€λ§ μ λ΅ κ°μ²΄λ₯Ό μμ±νλ κΈ°λ₯ μ κ³΅
            protected abstract ElevatorScheduler getScheduler();
        		
        		// ννλ¦Ώ λ©μλ : μμ²­μ λ°λΌ μλ λ² μ΄ν°λ₯Ό μ ννκ³  μ΄λμν΄
            public void requestElevator(int destination, Direction direction){
                // μ£Όμ΄μ§ μ λ΅ IDμ ν΄λΉλλ ElevatorSchedulerλ‘ λ³κ²½
                ElevatorScheduler scheduler = getScheduler();
                System.out.println(scheduler);
                int selectedElevator = scheduler.selectElevator(this, destination, direction);
                controllers.get(selectedElevator).gotoFloor(destination);
            }
        }
        ```

    - μλ²  μ€μΌμ€λ§ μ λ΅μ μν ElevatorManagerλ₯Ό μμλ°λ νμ ν΄λμ€ μμ±

        β λͺ¨λ μ€μΌμ€λ§ μ λ΅ κ°μ²΄λ₯Ό μμ±νλ getScheduler() λ©μλλ₯Ό κ΅¬νν΄μΌν¨ (= κ°μ²΄ μμ±μ λΆλ¦¬)

        μ°Έκ³  ) ννλ¦Ώ λ©μλ ν¨ν΄μ κ°λμ λ°λ₯΄λ©΄, νμ ν΄λμ€μμ μ€λ²λΌμ΄λλ  νμκ° μλ λ©μλλ primitive λλ hook λ©μλλΌκ³  λΆλ₯Έλ΅λλ€.

        - emWithThroughputManager

            ```java
            public class emWithThroughputManager extends ElevatorManager {
                public emWithThroughputManager(int controllerCount) {
                    super(controllerCount);
                }

                @Override
                protected ElevatorScheduler getScheduler() {
                    return ThroughputScheduler.getInstance();
                }
            }
            ```

        - emWithResponseTimeManager

            ```java
            public class emWithResponseTimeManager extends ElevatorManager {
                public emWithResponseTimeManager(int controllerCount) {
                    super(controllerCount);
                }

                @Override
                protected ElevatorScheduler getScheduler() {
                    return ResponseTimeScheduler.getInstance();
                }
            }
            ```

        - emWithDynamicManager

            ```java
            public class emWithDynamicManager extends ElevatorManager {
                public emWithDynamicManager(int controllerCount) {
                    super(controllerCount);
                }

                @Override
                protected ElevatorScheduler getScheduler() {
                    ElevatorScheduler scheduler = null;
                    int hour = Calendar.getInstance().get(Calendar.HOUR_OF_DAY);
                    if(hour < 12) {
                        scheduler = ResponseTimeScheduler.getInstance();
                    }
                    else{
                        scheduler = ThroughputScheduler.getInstance();
                    }
                    return scheduler;
                }
            }
            ```

    - μ€ν
        - Client

            ```java
            public class Client {

                public static void main(String[] args) {
                    ElevatorManager emWithResponseTimerScheduler = new emWithResponseTimeManager(2);
                    emWithResponseTimerScheduler.requestElevator(10, Direction.UP);

                    ElevatorManager emWithThroughputScheduler = new emWithThroughputManager(2);
                    emWithThroughputScheduler.requestElevator(10, Direction.UP);

                    ElevatorManager emWithDynamicScheduler = new emWithDynamicManager(2);
                    emWithDynamicScheduler.requestElevator(10, Direction.UP);
                }

            }
            ```

        - μΆλ ₯ κ²°κ³Ό
            - <img width="451" alt="αα³αα³αα΅α«αα£αΊ 2021-07-30 αα©αα₯α« 11 45 07" src="https://user-images.githubusercontent.com/53184797/127592378-c483b4e5-7891-4a31-bded-eb19837b23a8.png">
