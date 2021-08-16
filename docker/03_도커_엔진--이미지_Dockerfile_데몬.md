# Docker Engine

## 🐳 도커 이미지, Dockerfile, 도커 데몬

## 🐳 도커 이미지

모든 컨테이너는 이미지를 기반으로 생성되므로 도커 이미지를 다루는 방법은 중요하다.

도커는 기본적으로 도커 허브 (중앙 이미지 저장소)에서 이미지를 내려받는다.

- 이미지 검색 : 도커 허브 사이트 or `docker search [이미지]`

### 1️⃣ 도커 이미지 생성

`docker commit -a [작성자] -m [커밋 메시지] [컨테이너 이름] [리포지토리[:태그]]`

예) 

- commit_test:first 이미지 = ubuntu 이미지 컨테이너 + first 파일 추가
- commit_test:second 이미지 = commit_test:first 이미지 컨테이너 + second 파일 추가

```docker
# ubuntu:14.04 이미지로 commit_test 컨테이너 생성
docker run -i -t --name commit_test ubuntu:14.04
echo test-first! >> first
# commit_test 컨테이너를 기반으로 commit_test:first 이미지 생성
docker commit -a "kim" -m "my first commit" commit_test commit_test:first

# commit_test:first 이미지로 commit_test2 컨테이너 생성
docker run -i -t --name commit_test2 commit_test:first
echo test-second! >> second
# commit_test2 컨테이너를 기반으로 commit_test:second 이미지 생성
docker commit -a "kim" -m "my second commit" commit_test commit_test:second

# 이미지 확인
docker images
```

### 2️⃣ 이미지 구조

: 레이어 구조 

레이어 구조 확인 : `docker history [리포지토리[:태그]]`

전체 이미지의 실제 크기 : 188MB(ubuntu) + first 파일 크기 + second 파일

❓ **중간 레이어의 이미지를 삭제한다면?**

`docker rmi commit_test:first`

출력 결과 : `Untagged: commit_test:first`

commit_test:first 기반인 하위 이미지 commit_test:second의 존재로 Untagged 출력

- Untagged - 실제 이미지 파일을 삭제하지 않고 이미지에 부여된 이름만 삭제한다.

삭제되는 이미지의 **부모 이미지가 존재하지 않아야만 해당 이미지의 파일이 실제로 삭제**됨.

### 3️⃣ 이미지 추출

도커 이미지를 별도로 저장하거나 옮기는 등 바이너리 파일로 저장해야할 때,

- 이미지의 모든 메타 데이터를 포함하여 하나의 파일로 추출

    `docker save -o [추출될 파일명] [리포지토리[:태그]]`

- 추출된 이미지 로드

    `docker load -i [파일명]`

이미지를 단일 파일로 저장하는 것은 **이미지의 용량을 각기 차지**하기 때문에 효율적인 방법이 아니다. 

예) ubuntu 188MB + commit_test:first 188MB = 376MB

### 4️⃣ 이미지 배포

1. 도커 허브 저장소
2. 도커 사설 레지스트리 : 개인 서버에 이미지를 저장할 수 있는 저장소 만들 수 있음

## 🐳 Dockerfile

: 컨테이너에 설치해야하는 패키지, 추가해야하는 소스코드, 셸 스크립트 등 **컨테이너에서 수행해야할 작업들을 명시한 하나의 파일**

빌드 명령어는 **Dockerfile을 읽어** 컨테이너에서 작업을 수행한 뒤 **이미지를 생성**한다.

→ 직접 컨테이너를 생성하고 이미지로 커밋해야하는 번거로움을 덜고, 개발 도구들을 통해 빌드 및 배포 자동화 가능

### 1️⃣ Dockerfile 작성

```docker
FROM ubuntu:14.04
LABEL "purpose"="practice"
RUN apt-get update
RUN apt-get install apache2 -y
ADD test.html /var/www/html
WORKDIR /var/www/html
RUN ["/bin/bash", "-c". "echo hello >> test2.html"]
EXPOSE 80
CMD apachectl -DFOREGROUND
```

- `FROM` : 생성할 이미지의 **베이스가 될 이미지**
- `LABEL` : 이미지에 메타데이터('키:값' 형태) 추가 - `docker inspect` 로 확인 가능
- `RUN` : 이미지를 만들기 위해 **컨테이너 내부에서 명령어 실행**
    - `apt-get install apache2 **-y**`

        → Dockerfile을 이미지로 빌드하는 과정에서는 **별도의 입출력이 불가능** 

        → 별도의 입력을 받아야 하는 RUN이 있다면 build 명령어는 오류로 간주하고 빌드를 종료함

        → **해결 : Y/N을 설정**

    - JSON 배열의 형태로 사용 : `RUN ["실행 가능한 파일", "명령줄 인자 1", "명령줄 인자 2", "... ]`

        예제) /bin/bash 셸을 이용하여 "echo hello >> test2.html" 실행

- `ADD` : 파일을 이미지에 추가 (지금 위치의 test.html 파일을 이미지의 /var/www/html 디렉터리에 추가)
    - JSON 배열의 형태로 사용 : `ADD ["추가할 파일 이름", ... "컨테이너에 추가될 위치"]`
- `WORKDIR` : 명령어를 실행할 디렉터리로 bash 셸에서 **cd 명령어**를 입력하는 것과 같은 기능
- `EXPOSE` : Dockerfile의 빌드로 생성된 이미지에서 **노출할 포트를 설정.** 호스트의 포트와 바인딩 되는 것이 아니라 **단지 컨테이너의 포트를 사용할 것**임을 나타내는 것이다.
- `CMD` : **컨테이너가 시작될 때마다 실행할 명령어(커맨드)를 설정.** Dockerfile에서 한번만 사용할 수 있다.

    → 기본적으로 내장된 커맨드가 덮어씌워짐

    - `CMD apachectl -DFOREGROUND` : 자동으로 아파치 웹 서버 실행
    - JSON 배열의 형태로 사용 : CMD ["실행 가능한 파일", "명령줄 인자 1", "명령줄 인자 2", ... ]

🧐 **detached 모드 다시보기**

**아파치 웹 서버**는 하나의 터미널을 차지하는 **포그라운드 모드로 실행되기 때문에** -d 옵션을 사용해 **detached 모드로 컨테이너를 생성해야만 한다**. `CMD apachectl -DFOREGROUND`가 커맨드로 설정된 이미지로 컨테이너를 생성할 때 docker run -d가 아닌 -i -t 옵션으로 컨테이너를 생성하면 상호 입출력이 불가능한 상태로 아파치 웹 서버가 포그라운드 모드로 동작하는 것만 지켜볼 수 있다.

### 2️⃣ Dockerfile 빌드

1. **이미지 생성** : `docker build -t [이미지 이름] [Dockerfile 저장 경로]`
    - -t 옵션을 사용X : 16진수 형태의 이름으로 이미지가 저장됨

    생성된 이미지로 컨테이너 실행 : `docker run -d -P [이미지 이름]`

    - **-P 옵션** : 이미지에 설정된 `EXPOSE`로 노출된 모든 포트를 호스트에 연결하도록 설정
2. **빌드 과정**
    - 빌드 컨텍스트(Dockerfile이 위치한 디렉터리) 읽어들임

        <img width="1293" alt="스크린샷 2021-08-13 오전 9 55 37" src="https://user-images.githubusercontent.com/53184797/129288168-b945696a-7a29-4458-8c27-c8ac29229729.png">

        → 컨텍스트는 (build 명령어의 맨 마지막까지 포함) 단순 파일 뿐 아니라 하위 디렉터리도 전부 포함하게 되므로 **빌드에 불필요한 파일이 포함된다면 속도가 느려지고 호스트의 메모리 지나치게 점유**

        → Dockerfile이 위치한 곳에는 이미지 빌드에 필요한 파일만 있는 것이 바람직

        → 방지 : **.dockerignore 파일** 작성 (Dockerfile과 같은 곳에 위치해야함) - .gitignore처럼,, 파일에 명시된 이름의 파일을 컨텍스트에서 제외

    - Dockerfile을 이용한 컨테이너 생성과 커밋

        <img width="1298" alt="스크린샷 2021-08-13 오전 9 55 44" src="https://user-images.githubusercontent.com/53184797/129288203-4db551ca-1e24-4f1e-81aa-8c252d515d7d.png">

        → Dockerfile에서 **명령어 한 줄**이 실행될 때마다 **이전 Step에서 생성된 이미지에 의해 새로운 컨테이너가 생성**되며, Dockerfile에 적힌 명령어를 수행하고 다시 새로운 이미지 **레이어**로 저장됨.

        → 이미지 빌드가 완료되면 **Dockerfile의 명령어 줄 수만큼의 레이어가 존재** (출력 결과 : Removing intermediate container ... = 중간 이미지 레이어를 생성하기 위해 임시로 생성된 컨테이너를 삭제)

    - 캐시를 이용한 이미지 빌드

        ![스크린샷 2021-08-13 오전 9 55 50](https://user-images.githubusercontent.com/53184797/129288210-5ec70fb0-de53-406f-8bf4-9355c85e4073.png)

        : **이전에 빌드했던 Dockerfile에 같은 내용이 있다면** 새로 빌드하지 않고 이전의 이미지 빌드에서 사용했던 **캐시를 사용**(이전에 사용한 이미지 레이어를 활용해 이미지 생성)

        - **캐시 기능이 불필요한 경우**

            : RUN git clone ... 을 사용해 이미지를 빌드했을 때,

            → RUN에 대한 이미지 레이어를 계속 캐시로 사용하기 때문에 새로운 소스코드가 적용되지 않는다.

            해결 : 빌드할 때,  `—no-cache` 옵션 / 캐시로 사용할 이미지 지정 : `—cache-from [캐시사용이미지]`

3. **멀티 스테이지**를 이용한 Dockerfile 빌드 

    실행파일의 크기는 작지만 소스코드에 사용된 각종 패키지 및 라이브러리가 불필요하게 이미지의 크기를 차지하는 경우가 있다.

    **멀티 스테이지 빌드** : 하나의 Dockerfile 안에 **여러 개의 FROM 이미지를 정의**함으로써 빌드 완료 시 최종적으로 생성될 **이미지의 크기를 줄이는 역할**

    예)

    ```docker
    FROM golang # as builder
    ADD main.go /root
    WORKDIR /root
    RUN go build -o /root/mainApp /root/main.go

    FROM alpine:latest # alpine: 우분투나 CentOS보다 이미지 크기가 매우 작지만 필수 요소는 다 있는 리눅스 배포판 이미지
    WORKDIR /root
    COPY --from=0 /root/mainApp # --from=builder
    CMD ["./mainApp"]
    ```

1. **기타 Dockerfile 명령어**
    1. ENV, VOLUME, ARG, USER
        - `ENV` : Dockerfile에서 사용될 **환경변수를 지정**. 설정한 환경변수는 ${ENV_NAME} 또는 $ENV_NAME 형태로 사용 가능
        - `VOLUME` : 빌드된 이미지로 컨테이너를 생성했을 때 **호스트와 공유할 컨테이너 내부의 디렉터리를 설정** (여러개 나열 가능)

            ex) `VOLUME /home/dir /home/dir2`

            - JSON 배열의 형태로 사용 : `VOLUME ["/home/dir", "/home/dir2"]`
        - `ARG` : **빌드 명령어를 실행할 때 추가로 입력을 받아** Dockerfile 내에서 **사용될 변수의 값을 설정**

            ex) `ARG my_arg` or `ARG my_arg=val2` (기본값 지정)

            ```docker
            FROM ubuntu:14.04
            ARG my_arg
            ARG my_arg_2=value2
            RUN touch ${my_arg}/mytouch

            docker build —build-arg my_arg=/home -t mybuild:0.0 ./
            # /home 디렉터리에 mytouch 파일 확인 가능
            ```

        - `USER` : 사용자 설정 & 권한 부여
    2. Onbuild, Stopsignal, HealthCheck, Shell

        **추가 예정 !!!**

    3. ADD, COPY
        - `COPY` : 로컬 디렉터리에서 읽어들인 컨텍스트로부터 이미지에 **파일을 복사**하는 역할

            vs ADD - COPY는 로컬의 파일만 이미지에 추가할 수 있지만 ADD는 외부 URL 및 tar 파일(자동으로 해제해서 추가)에서도 파일을 추가할 수 있음

            → COPY를 추천한다. ADD는 어떤 파일인지 잘 모를 수 있음.

    4. ENTRYPOINT, CMD

        ENTRYPOINT vs CMD

        - 공통점 : 컨테이너가 시작될 때 수행할 명령을 지정
        - 차이점 : entrypoint는 커맨드를 인자로 받아 사용할 수 있는 **스크립트의 역할**이 가능

        예)

        ![스크린샷 2021-08-13 오전 9 55 55](https://user-images.githubusercontent.com/53184797/129288214-8c35273c-3181-4271-a528-77082f3614e5.png)

        ```docker
        # cmd로 배시셸을 실행하도록 설정 (/bin/bash)
        docker run -i -t ubuntu:14.04 /bin/bash # 컨테이너 내부로 들어가서 표준입출력 가능

        docker run -i -t --entrypoint="echo" ubuntu:14.04 /bin/bash # /bin/bash 출력

        # entrypoint를 이용한 스크립트 실행
        # 단 스크립트 파일은 컨테이너 내부에 존재해야함 (= 이미지 내에 있어야함 : 파일 추가는 ADD, COPY)
        docker run -i -t --entrypoint="/test.sh" ubuntu:14.04 /bin/bash
        ```

        → entrypoint가 설정되면 cmd는 단지 entrypoint에 대한 인자의 기능을 함

### 3️⃣ Dockerfile로 빌드할 때 주의할 점

Dockerfile을 아무렇게나 작성하면 저장 공간을 불필요하게 차지하는 이미지나 레이어가 너무 많은 이미지가 생성될 수 있음

비효율적인 예) 빌드된 이미지의 크기가 ubuntu + 100MB

```docker
FROM ubuntu:14.04
RUN mkdir /test
RUN fallocate -l 100m /test/dummy
RUN rm /test/dummy # 결국엔 없는 폴더
```

→ 컨테이너의 변경된 사항만 새로운 이미지 레이어로 생성하는 방식의 단점 !

**방지) RUN이 하나의 이미지가 되므로 하나로 묶어서 이미지 레이어의 개수를 줄인다.**

```docker
FROM ubuntu:14.04
RUN mkdir /test && \
fallocate -l 100m /test/dummy && \
rm /test/dummy
```

## 🐳 도커 데몬

지금까지 도커를 사용하는 방법을 알았으니 이제는 도커 자체를 다뤄보자.

### 1️⃣ 도커의 구조

1. 도커 명령어의 위치 

    → 도커 명령어는 /usr/bin/docker에 위치한 파일을 통해 사용되고있음

    ```bash
    which docker # 명령어 파일이 위치한 경로 출력
    /usr/bin/docker
    ```

2. 실행중인 도커 프로세스 확인

    → 도커 엔진의 프로세스는 /usr/bin/dockerd 파일로 실행되고 있음

    ```bash
    ps aux | grep docker # ps aux : 실행중인 프로세스 목록 출력
    ```

    <img width="680" alt="스크린샷 2021-08-14 오후 12 29 29" src="https://user-images.githubusercontent.com/53184797/129432813-a4ef5006-4a7d-4ae3-8924-74a24874610b.png">

3. **도커의 구조**
    - 서버로서의 도커 - 실제 컨테이너를 생성,실행하며 이미지를 관리하는 주체 (dockerd 프로세서로 동작)

        ⇒ 도커 엔진은 외부에서 API를 입력 받아서 기능을 수행한다.

        ⇒ "**도커 데몬" : 도커 프로세스가 실행되어 서버로서 입력을 받을 준비가 된 상태**

    - 클라이언트로서의 도커 - 도커 엔진의 기능을 수행하는데 API(도커 데몬에게 입력)를 사용할 수 있도록 CLI를 제공하는 것, `docker`로 시작하는 명령어
4. **도커의 제어 과정**

    <img width="1275" alt="스크린샷 2021-08-14 오후 12 27 11" src="https://user-images.githubusercontent.com/53184797/129432763-e4188e69-2adf-45cc-a05b-52a59d860932.png">

    1. 터미널이나 PuTTY 등으로 도커가 설치된 호스트에 접속
    2. 사용자가 docker ps와 같은 도커 명령어 입력
    3. /usr/bin/docker는 /var/run/docker.sock 유닉스 소켓을 사용해 도커 데몬에게 명령어 전달
    4. 도커 데몬은 이 명령어를 파싱하고 명령어에 해당하는 작업을 수행
    5. 수행 결과를 도커 클라이언트에 반환하고 사용자에게 결과를 출력

### 2️⃣ 도커 데몬 실행

1. 서비스를 통해서 실행

    ```bash
    service docker start
    service docker stop
    ```

2. 직접 실행

    ```bash
    service docker stop
    dockerd
    ```

    <img width="679" alt="스크린샷 2021-08-14 오후 12 29 19" src="https://user-images.githubusercontent.com/53184797/129432814-1ba1bddb-5ac4-4735-8857-c17fd50bbbe0.png">

    → /var/run/docker.sock에서 입력을 받을 수 있는 상태라는 메세지 출력

⇒ 실제 운영 환경에서는 도커 데몬을 직접 실행하기보다는 service, systemctl 명령어로 서비스로 관리하는게 좋다. (디버깅, 도커 자체의 트러블 슈팅이 필요한 경우는 도커 데몬)

### 3️⃣ 도커 데몬 설정

1. 도커 데몬 제어 `-H` : 도커 데몬의 API를 사용할 수 있는 방법 추가

    ```bash
    dockerd -H unix:///var/run/docker.sock -H tcp://192.168.99.100:2375
    ```

2. 도커 데몬 보안 적용 `—tlsverify` : 보안 연결을 설정하는 옵션(기본적으로 설정되지 않음)
3. 도커 스토리지 드라이버 `—storage-driver`
    - AUFS, Devicemapper, OverlayFS, Btrfs, ZFS 등

### 4️⃣ 도커 데몬 모니터링

1. 도커 데몬 debug 모드 : `dockerd -D`
2. 도커가 제공하는 명령어 : `events` ,`stats`, `system df` 
3. 구글이 만든 컨테이너 모니터링 도구 : CAdvisor