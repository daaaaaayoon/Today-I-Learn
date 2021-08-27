# Ch6. Deadlock

## 1. Deadlock이란?

: 일련의 프로세스들이 **서로가 가진 자원을 기다리며 block된 상태**

각자 자원을 가지고 있으면서 상대방의 자원을 요청하여 더 이상 진행이 되지 않는 상황

어느 프로세스도 양보하지 않으면 더 이상 진행이 되지 않는다.

### (1) Resource (자원)

- 하드웨어, 소프트웨어 등을 포함하는 개념

    ex) I/O device, CPU cycle, 메모리 공간, semaphore 등

- 프로세스가 자원을 사용하는 절차 4단계
    - Request(요청), Allocate(획득), Use(사용), Release(반납)

### (2) Deadlock의 예

1. 하드웨어

    I/O device의 경우, 시스템에 2개의 tape drive가 있을 때,

    프로세스 P1과 P2는 각각 하나의 tape drive를 보유한 채, 다른 하나를 기다리고 있음

    → 진행 불가

2. 소프트웨어

    P1이 A를 획득하고 CPU를 빼앗김. P2가 B를 가진 상황에서 A를 가지려하는데 A는 이미 P1이 소유하고 있음.

    → 어느 쪽에서 CPU를 가지고 있던 진행 불가

### (3) Deadlock 발생 4가지 조건

: 교착상태는 왜 발생하는가?

- **Mutual exclusion (상호배제)** : 매 순간 하나의 프로세스만이 자원을 사용할 수 있다. (자원을 독점적으로 쓴다)
- **No preemption (비선점)** : 프로세스는 자원을 스스로 내어놓을 뿐 강제로 빼앗기지 않는다.
- **Hold and wait (보유대기)** : 자원을 가진 프로세스가 다른 자원을 기다릴 때, 보유 자원을 놓지 않고 계속 가지고 있음
- **circular wait (순환대기)** : 자원을 기다리는 프로세스 간에 사이클이 형성되어야함. ex) P0은 P1이 가진 자원을 기다릴 때, P1은 P2가 가진 자원을 기다리고 PN은 P0이 가진 자원을 기다리는 경우

### (4) 자원할당 그래프

: deadlock이 발생했는지 알아보기 위해 사용하는 그래프

- vertex : 동그라미는 프로세스. 네모는 자원. 네모 안의 점은 자원의 수
- edge
    - 자원 → 프로세스 : 이 프로세스가 해당 자원을 가지고 있다.
    - 프로세스 → 자원 : 이 프로세스가 해당 자원을 요청. 아직 획득 못함.

    <img width="694" alt="스크린샷 2021-08-27 오후 7 42 18" src="https://user-images.githubusercontent.com/53184797/131115207-104bdc18-f337-4e87-910a-4316b4ed4da0.png">

- 그래프에 cycle이 없으면 deadlock이 아니다.
- 그래프에 cycle이 있으면,
    - 자원당 instance(⚫️ 개수)가 하나 밖에 없으면, deadlock
    - 자원당 instance가 여러개면, deadlock일 수도 아닐 수도

두 그림 모두 cycle이 존재한다.

- 왼쪽 그림 : R(2) 자원을 P(1), P(2)가 갖고 있으면서, P(3)가 요청하는 상황이기에 deadlock.
- 오른쪽 그림 :  P(4)가 R(2)를 반납하거나, P(2)가 R(1)을 반납할 수 있기 때문에 deadlock 아님. P(4)와 P(2)는 deadlock에 관련된 프로세스가 아님.

## 2. Deadlock 처리 방법

- **미연에 방지하는 방법**
    1. Deadlock Prevention : 자원 할당 시, Deadlock의 4가지 필요 조건 중 어느 하나가 만족되지 않도록 하는 것.
    2.  Deadlock Avoidance : 자원 요청에 대한 부가적인 정보를 이용해서, deadlock의 가능성이 없는 경우에만 자원을 할당.

        시스템 state가 원래 state로 돌아올 수 있는 경우에만 자원 할당.

- **deadlock이 생기게 놔두는 방법**
    1. Deadlock Detection and recovery : deadlock 발생은 허용하되 그에 대한 detection 루틴을 두어 deadlock 발견 시 recover함
    2. **Deadlock Ignorance** : deadlock을 시스템이 책임지지 않음. 사용자가 알아서 강제종료 등 처리. UNIX를 포함한 대부분의 OS가 채택.

        ⇒ 현대 OS에서 채택 : **deadlock이 자주 발생하지 않기 때문에, 미연에 방지하는데 드는 overhead가더 크다.**