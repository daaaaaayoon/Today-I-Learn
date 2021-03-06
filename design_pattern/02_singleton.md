## ๐จ ์ฑ๊ธํค ํจํด์ด๋?

- '์์ฑ ํจํด'์ ํ๋
- ์ ์ญ ๋ณ์๋ฅผ ์ฌ์ฉํ์ง ์๊ณ  **๊ฐ์ฒด๋ฅผ ํ๋๋ง ์์ฑ**ํ๋๋ก ํ๋ฉฐ, **์์ฑ๋ ๊ฐ์ฒด๋ฅผ ์ด๋์์๋ ์ง ์ฐธ์กฐ**ํ  ์ ์๋๋ก ํ๋ ํจํด
- = "ํ๋์ ๊ฐ์ฒด๊ฐ ๋ชจ๋  ๊ฒ์ ๊ด๋ฆฌํ๋ค"
- <img width="434" alt="แแณแแณแแตแซแแฃแบ 2021-07-23 แแฉแแฎ 6 02 31" src="https://user-images.githubusercontent.com/53184797/126760620-103c61b5-c1de-4799-92bf-81f036f98eb1.png">
- getInstance() ๋ฉ์๋๋ฅผ ํตํด ๋ชจ๋  ํด๋ผ์ด์ธํธ์๊ฒ ๋์ผํ ์ธ์คํด์ค๋ฅผ ๋ฐํํ๋ ์์์ ์ํ

## ๐จ ์ฑ๊ธํค ํจํด์ ์์

### 1) ํ๋ฆฐํฐ ์์

- <img width="449" alt="แแณแแณแแตแซแแฃแบ 2021-07-23 แแฉแแฎ 6 02 39" src="https://user-images.githubusercontent.com/53184797/126760635-b1a520a8-52c6-4d3f-9076-211d45b705df.png">
- ํ๋ฆฐํฐ 1๋๋ฅผ 10๋ช์ด ๊ณต์ ํด์ ์ฌ์ฉํ๋ค๊ณ  ํ์

- P**rinter ํด๋์ค**
    ```java
    public class Printer {
        public Printer() {}                    // ์์ฑ์
        public void print(Resource r) { ... }  // ํ๋ฆฐํธ ๊ธฐ๋ฅ
    }
    ```

- โ ํ๋ฆฐํฐ๋ 1๋์ด๊ธฐ ๋๋ฌธ์ **Printer ์ธ์คํด์ค๋ ๋จ ํ๋๋ง ์กด์ฌํด์ผํจ**
- = new Printer() (=์์ฑ์)๋ฅผ ํตํด์ ์ธ์คํด์ค ์์ฑ์ ํ ๋ฒ๋ง ๊ฐ๋ฅํ๊ฒ ํด์ผํจ
- โ ํด๊ฒฐ : ๋์์ธ ํจํด ์ค์ "Singleton Pattern"์ ์ ์ฉํ๋ฉด ๋๊ฒ ๋ค!

### 2) ์ฑ๊ธํค ํจํด ์ ์ฉํ๊ธฐ

1. ํ์ ์กฐ๊ฑด
    - ์์ฑ์๋ฅผ ์ธ๋ถ์์ ํธ์ถํ  ์ ์๊ฒ private ์ ์ธ
    - ์ธ๋ถ์ ์ ๊ณตํ  ๋จ ํ๋์ ์ธ์คํด์ค๋ฅผ ์ ์ญ(static)์ผ๋ก ์ ์ธ
    - ์๊ธฐ ์์ ์ ๋ํ ์ธ์คํด์ค๋ฅผ ์ธ๋ถ์ ์ ๊ณตํ๊ธฐ ์ํ ๋ฉ์๋
2. ์ ์ฉ ์ฝ๋

    - <img width="428" alt="แแณแแณแแตแซแแฃแบ 2021-07-23 แแฉแแฎ 6 02 46" src="https://user-images.githubusercontent.com/53184797/126760649-9c7ed52b-10d8-46f5-a1d9-719b9a5df56e.png">
    - **Printer**

        ```java
        package singleton;

        public class Printer {

            // ์๊ธฐ ์์ ์ ์ธ์คํด์ค
            private static Printer printer = null;

            // ์์ฑ์
            private Printer() {};

            // ์ธ์คํด์ค๋ฅผ ์ธ๋ถ์ ์ ๊ณตํ  ๋ฉ์๋
            public static Printer getPrinter() {
                if(printer == null) {
                    printer = new Printer();
                }
                return printer;
            }

            public void print(String str) {
                System.out.println("๋์ :"+str);
            }

        }
        // ๋ง์ฝ new Printer()๊ฐ ํธ์ถ๋๊ธฐ ์ ์ด๋ฉด ์ธ์คํด์ค ๋ฉ์๋์ธ print() ๋ฉ์๋๋ ํธ์ถํ  ์ ์์
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
                printer.print(this.name + "๋ฒ ์ฌ์ฉ์๊ฐ ํ๋ฆฐํฐ "+printer.toString() +" ๋ฅผ ์ฌ์ฉ์ค์๋๋ค.");
            }

        }
        ```

    - **Client**

        ```java
        package singleton;

        public class Client {

            private static final int USER_NUM = 5;

            public static void main(String[] args) {

        				// USER_NUM๋ช์ ์ฌ์ฉ์ ๋ง๋ค๊ธฐ
                User[] user = new User[USER_NUM];
        				
        				// ํ๋์ ์ฌ์ฉ์๋ง๋ค ํ๋ฆฐํฐ ์ฌ์ฉํ๊ธฐ
                for(int i=0; i<USER_NUM; i++) {
                    user[i] = new User(String.valueOf(i+1)); // ์ ์  : 1~5
                    user[i].print();
                }
            }

        }
        ```

    - **๊ฒฐ๊ณผ : ๋ชจ๋ ๊ฐ์ ํ๋ฆฐํฐ ์ฌ์ฉ**

        **๊ฐ์ฒด๋ฅผ ํ๋๋ง ์์ฑ** โ **์์ฑ๋ ๊ฐ์ฒด๋ฅผ ์ด๋์์๋ ์ง ๊ฐ์ ธ๋ค ์ด๋ค.**

        <img width="403" alt="แแณแแณแแตแซแแฃแบ 2021-07-23 แแฉแแฎ 6 02 56" src="https://user-images.githubusercontent.com/53184797/126760661-da2b8f2b-a662-46ee-872f-94a0e9c1dfda.png">

## ๐จ  ์ฑ๊ธํค ํจํด์ ๋ฌธ์ ์ ๊ณผ ํด๊ฒฐ์ฑ

- **๋ค์ค ์ค๋ ๋**์์ Printer ํด๋์ค๋ฅผ ์ด์ฉํ  ๋, ์ธ์คํด์ค๊ฐ 1๊ฐ ์ด์ ์์ฑ๋  ๊ฐ๋ฅ์ฑ์ด ์์
- **ํด๊ฒฐ์ฑ**
    1. **์ ์  ๋ณ์์ ์ธ์คํด์ค๋ฅผ ๋ง๋ค์ด ๋ฐ๋ก ์ด๊ธฐํ**ํ๋ ๋ฐฉ๋ฒ

        ```java
        public class Printer {

           // static ๋ณ์์ ์ธ๋ถ์ ์ ๊ณตํ  ์๊ธฐ ์์ ์ ์ธ์คํด์ค๋ฅผ ๋ง๋ค์ด ์ด๊ธฐํ
           private static Printer** **printer = new Printer();
           
           private Printer() { }

           // ์๊ธฐ ์์ ์ ์ธ์คํด์ค๋ฅผ ์ธ๋ถ์ ์ ๊ณต
           public static Printer getPrinter(){
             return printer;
           }

           public void print(String str) {
             System.out.println(str);
           }
        }
        ```

    2. ์ธ์คํด์ค๋ฅผ ๋ง๋๋ **๋ฉ์๋**์ **๋๊ธฐํ**ํ๋ ๋ฐฉ๋ฒ

        ```java
        public class Printer {

           // ์ธ๋ถ์ ์ ๊ณตํ  ์๊ธฐ ์์ ์ ์ธ์คํด์ค
           private static Printer printer = null;

           private Printer() { }

           // ์ธ์คํด์ค๋ฅผ ๋ง๋๋ ๋ฉ์๋ ๋๊ธฐํ
           public synchronized static Printer getPrinter(){
             if (printer == null) {
               printer = new Printer(); // Printer ์ธ์คํด์ค ์์ฑ
             }
             return printer;
           }
           public void print(String str) {
             // ์ค์ง ํ๋์ ์ค๋ ๋๋ง ์ ๊ทผ์ ํ์ฉํจ (์๊ณ ๊ตฌ์ญ)
             // ์ฑ๋ฅ์ ์ํด ํ์ํ ๋ถ๋ถ๋ง์ ์๊ณ ๊ตฌ์ญ์ผ๋ก ์ค์ 
             synchronized(this) {
               System.out.println(str);
             }
           }
        }
        ```

## ๐จ ์ฑ๊ธํค๊ณผ ๋์ผํ ํจ๊ณผ

### 1) ์ ์  ํด๋์ค

์ ์  ๋ฉ์๋๋ก๋ง ์ด๋ฃจ์ด์ง ์ ์  ํด๋์ค ์ฌ์ฉ = ์ฑ๊ธํค๊ณผ ๋์ผํ ํจ๊ณผ

<**์ฑ๊ธํค๊ณผ์ ์ฐจ์ด์ >**

- ์ ์  ํด๋์ค๋ฅผ ์ฌ์ฉํ๋ฉด ๊ฐ์ฒด๋ฅผ ์ ํ ์์ฑํ์ง ์๊ณ  ๋ฉ์๋(static)๋ฅผ ์ฌ์ฉ

    โ '์ ์  ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋ ๊ฒ'์ '์คํํ  ๋ ๋ฐ์ธ๋ฉ๋๋(์ปดํ์ผ ํ์) ์ธ์คํด์ค ๋ฉ์๋๋ฅผ ์ฌ์ฉํ๋ ๊ฒ'๋ณด๋ค **์ฑ๋ฅ ๋ฉด์์ ์ฐ์**ํจ

```java
public class Printer {
      private static int counter = 0;
      // ๋ฉ์๋ ๋๊ธฐํ (์๊ณ ๊ตฌ์ญ)
      public synchronized static void print(String str) {
        counter++;
        System.out.println(str + counter);
      }
}
```

### 2) Enum ํด๋์ค

```java
public enum SingletonTest {
	INSTANCE;
  
	public static SingletonTest getInstance() {		
		return INSTANCE;
	}
}
```

- Thread-safety์ **Serialization**์ด ๋ณด์ฅ๋๋ค.
- **Reflection**์ ํตํ ๊ณต๊ฒฉ์๋ ์์ ํ๋ค.
- ๋ฐ๋ผ์ Enum์ ์ด์ฉํค์ ์ฑ๊ธํค์ ๊ตฌํํ๋ ๊ฒ์ด ๊ฐ์ฅ ์ข์ ๋ฐฉ๋ฒ