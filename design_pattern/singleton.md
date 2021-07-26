## 🎨 싱글톤 패턴이란?

- '생성 패턴'의 하나
- 전역 변수를 사용하지 않고 **객체를 하나만 생성**하도록 하며, **생성된 객체를 어디에서든지 참조**할 수 있도록 하는 패턴
- = "하나의 객체가 모든 것을 관리한다"
- <img width="434" alt="스크린샷 2021-07-23 오후 6 02 31" src="https://user-images.githubusercontent.com/53184797/126760620-103c61b5-c1de-4799-92bf-81f036f98eb1.png">
- getInstance() 메서드를 통해 모든 클라이언트에게 동일한 인스턴스를 반환하는 작업을 수행

## 🎨 싱글톤 패턴의 예시

### 1) 프린터 예시

- <img width="449" alt="스크린샷 2021-07-23 오후 6 02 39" src="https://user-images.githubusercontent.com/53184797/126760635-b1a520a8-52c6-4d3f-9076-211d45b705df.png">
- 프린터 1대를 10명이 공유해서 사용한다고 하자

- P**rinter 클래스**
    ```java
    public class Printer {
        public Printer() {}                    // 생성자
        public void print(Resource r) { ... }  // 프린트 기능
    }
    ```

- ⇒ 프린터는 1대이기 때문에 **Printer 인스턴스는 단 하나만 존재해야함**
- = new Printer() (=생성자)를 통해서 인스턴스 생성은 한 번만 가능하게 해야함
- ⇒ 해결 : 디자인 패턴 중에 "Singleton Pattern"을 적용하면 되겠다!

### 2) 싱글톤 패턴 적용하기

1. 필요 조건
    - 생성자를 외부에서 호출할 수 없게 private 선언
    - 외부에 제공할 단 하나의 인스턴스를 전역(static)으로 선언
    - 자기 자신에 대한 인스턴스를 외부에 제공하기 위한 메서드
2. 적용 코드

    - <img width="428" alt="스크린샷 2021-07-23 오후 6 02 46" src="https://user-images.githubusercontent.com/53184797/126760649-9c7ed52b-10d8-46f5-a1d9-719b9a5df56e.png">
    - **Printer**

        ```java
        package singleton;

        public class Printer {

            // 자기 자신의 인스턴스
            private static Printer printer = null;

            // 생성자
            private Printer() {};

            // 인스턴스를 외부에 제공할 메서드
            public static Printer getPrinter() {
                if(printer == null) {
                    printer = new Printer();
                }
                return printer;
            }

            public void print(String str) {
                System.out.println("동작 :"+str);
            }

        }
        // 만약 new Printer()가 호출되기 전이면 인스턴스 메서드인 print() 메서드는 호출할 수 없음
        ```

    - **User**

        ```java
        package singleton;

        public class User {

            private String name;
            public User(String name) {
                this.name = name;
            }

            public void print() {
                Printer printer = Printer.getPrinter();
                printer.print(this.name + "번 사용자가 프린터 "+printer.toString() +" 를 사용중입니다.");
            }

        }
        ```

    - **Client**

        ```java
        package singleton;

        public class Client {

            private static final int USER_NUM = 5;

            public static void main(String[] args) {

        				// USER_NUM명의 사용자 만들기
                User[] user = new User[USER_NUM];
        				
        				// 하나의 사용자마다 프린터 사용하기
                for(int i=0; i<USER_NUM; i++) {
                    user[i] = new User(String.valueOf(i+1)); // 유저 : 1~5
                    user[i].print();
                }
            }

        }
        ```

    - **결과 : 모두 같은 프린터 사용**

        **객체를 하나만 생성** → **생성된 객체를 어디에서든지 가져다 쓴다.**

        <img width="403" alt="스크린샷 2021-07-23 오후 6 02 56" src="https://user-images.githubusercontent.com/53184797/126760661-da2b8f2b-a662-46ee-872f-94a0e9c1dfda.png">

## 🎨  싱글톤 패턴의 문제점과 해결책

- **다중 스레드**에서 Printer 클래스를 이용할 때, 인스턴스가 1개 이상 생성될 가능성이 있음
- **해결책**
    1. **정적 변수에 인스턴스를 만들어 바로 초기화**하는 방법

        ```java
        public class Printer {

           // static 변수에 외부에 제공할 자기 자신의 인스턴스를 만들어 초기화
           private static Printer** **printer = new Printer();
           
           private Printer() { }

           // 자기 자신의 인스턴스를 외부에 제공
           public static Printer getPrinter(){
             return printer;
           }

           public void print(String str) {
             System.out.println(str);
           }
        }
        ```

    2. 인스턴스를 만드는 **메서드**에 **동기화**하는 방법

        ```java
        public class Printer {

           // 외부에 제공할 자기 자신의 인스턴스
           private static Printer printer = null;

           private Printer() { }

           // 인스턴스를 만드는 메서드 동기화
           public synchronized static Printer getPrinter(){
             if (printer == null) {
               printer = new Printer(); // Printer 인스턴스 생성
             }
             return printer;
           }
           public void print(String str) {
             // 오직 하나의 스레드만 접근을 허용함 (임계 구역)
             // 성능을 위해 필요한 부분만을 임계 구역으로 설정
             synchronized(this) {
               System.out.println(str);
             }
           }
        }
        ```

## 🎨 싱글톤과 동일한 효과

### 1) 정적 클래스

정적 메서드로만 이루어진 정적 클래스 사용 = 싱글톤과 동일한 효과

<**싱글톤과의 차이점>**

- 정적 클래스를 사용하면 객체를 전혀 생성하지 않고 메서드(static)를 사용

    ⇒ '정적 메서드를 사용하는 것'은 '실행할 때 바인딩되는(컴파일 타임) 인스턴스 메서드를 사용하는 것'보다 **성능 면에서 우수**함

```java
public class Printer {
      private static int counter = 0;
      // 메서드 동기화 (임계 구역)
      public synchronized static void print(String str) {
        counter++;
        System.out.println(str + counter);
      }
}
```

### 2) Enum 클래스

```java
public enum SingletonTest {
	INSTANCE;
  
	public static SingletonTest getInstance() {		
		return INSTANCE;
	}
}
```

- Thread-safety와 **Serialization**이 보장된다.
- **Reflection**을 통한 공격에도 안전하다.
- 따라서 Enum을 이용헤서 싱글톤을 구현하는 것이 가장 좋은 방법