# OS Introduction

## 🖥️ 운영체제란?

- Operating System, OS
- 컴퓨터 하드웨어 바로 위에 설치되어 사용자 및 다른 모든 소프트웨어와 하드웨어를 연결하는 소프트웨어 계층
- <img width="520" alt="스크린샷 2021-08-03 오후 9 14 21" src="https://user-images.githubusercontent.com/53184797/128013853-f7ba8b3f-288d-40af-b772-9fc8f0bed9e3.png">

- **좁은 의미의 OS = 커널**

    : 운영체제의 핵심 부분으로 메모리에 상주하는 부분

- 넓은 의미의 OS

    : 커널 뿐 아니라 각종 주변 시스템 유틸리티를 포함한 개념

## 🖥️ 운영체제의 목표

1. **컴퓨터 시스템을 편리하게 사용할 수 있는 환경을 제공**
    - 운영체제는 동시 사용자/프로그램들이 독자적으로 컴퓨터에서 수행되는 것 같은 느낌을 제공
      <img width="472" alt="스크린샷 2021-08-03 오후 9 15 08" src="https://user-images.githubusercontent.com/53184797/128013883-657cb9a3-8ba6-4d50-89ce-0a070cd48bee.png">
    - 하드웨어를 직접 다루는 복잡한 부분을 운영체제가 대행
2. **컴퓨터 시스템의 자원을 효율적으로 관리**
    - 프로세서, 기억장치, 입출력 장치 등의 효율적 관리
    <img width="473" alt="스크린샷 2021-08-03 오후 9 15 18" src="https://user-images.githubusercontent.com/53184797/128013893-c696c790-52ee-4d75-b56d-440d33219797.png">

## 🖥️ 운영체제의 분류

1. 동시 작업 가능 여부
    - **단일 작업 (single tasking)**

        : 한 번에 하나의 작업만 처리

        ex) MS-DOS 프롬프트 상에서는 한 명령의 수행을 끝내기 전에 다른 명령을 수행시킬 수 없음

    - **다중 작업 (multi tasking)**

        : 동시에 두 개 이상의 작업을 처리

        ex) UNIX, MS Windows 등에서는 한 명령의 수행이 끝나기 전에 다른 명령이나 프로그램을 수행할 수 있으

2. 사용자 수
    - 단일 사용자 (single user) : MS-DOS, MS Windows
    - 다중 사용자 (multi user) : UNIX, NT Server
3. 처리 방식
    - **일괄 처리 (batch processing)**

        : 작업 요청의 일정량 모아서 한꺼번에 처리하고 작업이 완전히 종료될 때까지 기다려야 함

        <img width="469" alt="스크린샷 2021-08-03 오후 9 15 24" src="https://user-images.githubusercontent.com/53184797/128013900-5df418c1-91ce-4a02-83df-6cf4a80cc7d2.png">

    - **시분할 (time sharing)**

        : 여러 작업을 수행할 때 컴퓨터 처리 능력을 **일정한 시간 단위로 분할**하여 사용

        : 일괄 처리 시스템에 비해 **짧은 응답 시간**을 가짐

        ex) UNIX

        : interactive한 방식

        <img width="471" alt="스크린샷 2021-08-03 오후 9 15 29" src="https://user-images.githubusercontent.com/53184797/128013904-27ffa9db-7882-42a9-9fc9-0cb2160fe442.png">

    - **실시간 (Realtime OS)**

        : deadline이 있음

        : 정해진 시간 안에 어떠한 일이 반드시 종료됨이 보장되어야하는 실시간 시스템을 위한 OS

        ex) 미사일 제어, 반도체 장비, 원자로/공장 제어

## 🖥️ 운영체제의 예

### 1) 유닉스(UNIX)

- 코드의 대부분을 C언어로 작성
- 높은 이식성
- 최소한의 커널 구조
- 복잡한 시스테에 맞게 확장 용이
- 소스 코드 공개, 프로그램 개발에 용이
- 다양한 버전 존재
    - System V, FreeBSD, Solaris , SunOS
    - Linux (공개 sw opensource)
        - 조금 더 개인 컴퓨터 vs Unix : 대형 컴퓨터(서버)를 위함

### 2) DOS(Disk Operating System)

- MS사 단일 사용자용 운영체제
- 메모리 관리 능력의 한계 (주기억장치 : 640KB)

### 3) MS Windows

- MS사의 다중 작업용 GUI 기반 운영체제

## 🖥️ 관련 용어

컴퓨터에서 **여러 작업을 동시에 수행**하는 것은?

- = Multitasking
- = Multiprogramming : 여러 프로그램이 메모리에 올라가 있음을 강조
- = Time sharing : CPU의 시간을 분할하여 나누어 쓴다는 의미를 강조
- = Multiprocess

vs Multiprocessor : 하나의 컴퓨터에 CPU(processor)가 여러개 붙어있음을 의미

## 🖥️ 운영체제의 구조

<img width="528" alt="스크린샷 2021-08-03 오후 9 15 36" src="https://user-images.githubusercontent.com/53184797/128013911-efc70188-e3ba-45a5-98db-f73b424617e6.png">

### Reference
[운영체제 강의 - 이화여자대학교 반효경](http://www.kocw.net/home/search/kemView.do?kemId=1046323)