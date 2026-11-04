# E1-1 내 컴퓨터에 개발자용 '작업실' 꾸미기

## 프로젝트 개요(미션 목표 요약)

터미널, Docker, Git을 이용하여 개발환경을 구성하고 Docker 기반 웹 서버 실행 및 검증 과정을 GitHub에 문서화합니다.

## 1. 제출 저장소 및 기술 문서

### 1) 실행 환경

 - OS: Ubuntu 26.04.1
 - Shell: bash
 - Docker: 29.9.3
 - Git: 2.53.0
 
### 2) 수행 체크리스트

 - [x] 터미널 기본 조작 및 폴더 구성
 - [x] 권한 변경 실습
 - [x] Docker 설치/점검
 - [x] hello-world 실행
 - [ ] Dockerfile 빌드/실행
 - [ ] 포트 매핑 접속(2회)
 - [ ] 바인드 마운트 반영
 - [ ] 볼륨 영속성
 - [ ] Git 설정 + VSCode GitHub 연동

## 2. 터미널 조작 로그 기록

### 1) 현재 위치 확인 

`pwd` 명령어를 사용하여 현재 작업 디렉터리의 경로를 확인하였다.

```text
kyu@localhost:~$ pwd
/home/kyu
```
### 2) 목록 확인(숨김 파일 포함)

`ls -al` 명령어를 사용하여 현재 디렉터리의 파일 및 디렉터리 목록을 숨김 파일까지 포함하여 확인하였다.

```text
kyu@localhost:~$ ls -al
total 40
drwxr-x--- 4 kyu  kyu  4096 Nov  4 09:40 .
drwxr-xr-x 3 root root 4096 Nov  4 08:51 ..
-rw------- 1 kyu  kyu   186 Nov  4 09:40 .bash_history
-rw-r--r-- 1 kyu  kyu   220 Nov  4 08:51 .bash_logout
-rw-r--r-- 1 kyu  kyu  3771 Nov  4 08:51 .bashrc
drwxr-x--- 4 kyu  kyu  4096 Nov  4 08:57 .cache
drwxr-x--- 3 kyu  kyu  4096 Nov  4 08:57 .config
-rw-rw-r-- 1 kyu  kyu     0 Nov  4 08:57 .motd_shown
-rw-r--r-- 1 kyu  kyu   807 Nov  4 08:51 .profile
-rw------- 1 kyu  kyu   860 Nov  4 09:39 .viminfo
-rw-r--r-- 1 kyu  kyu  2035 Nov  4 09:39 install_docker.sh
```

### 3) 생성, 이동, 복사, 이름변경, 삭제

#### 3-1) 생성

`mkdir` 명령어를 사용하여 `abc` 디렉터리를 생성하였다.  
이후 `ls` 명령어로 생성된 디렉터리를 확인하였다.

```text
kyu@localhost:~$ mkdir abc
kyu@localhost:~$ ls
abc  install_docker.sh
```
#### 3-2) 이동

`cd` 명령어를 사용하여 `abc` 디렉터리로 이동하였다.  
`pwd` 명령어로 현재 위치가 `/home/kyu/abc`로 변경되었는지 확인하였다.

```text
kyu@localhost:~$ cd ./abc
kyu@localhost:~/abc$ pwd
/home/kyu/abc
```

#### 3-3) 복사

`touch` 명령어로 빈 파일 `a1`을 생성한 뒤, `cp` 명령어를 사용하여 `a1`을 `a2`로 복사하였다.  
이후 `ls -l` 명령어로 두 파일이 존재하는지 확인하였다.


```text
kyu@localhost:~/abc$ touch a1
kyu@localhost:~/abc$ cp ./a1 ./a2
kyu@localhost:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a2
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
```

#### 3-4) 이름 변경

`mv` 명령어를 사용하여 `a2` 파일의 이름을 `b2`로 변경하였다.  
이후 `ls -l` 명령어로 파일명이 변경되었는지 확인하였다.

```text
kyu@localhost:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a2
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc

kyu@localhost:~/abc$ mv ./a2 ./b2
kyu@localhost:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 b2
```
#### 3-5) 삭제

`rm` 명령어를 사용하여 `b2` 파일을 삭제하였다.  
이후 `ls -l` 명령어로 `b2` 파일이 삭제되었는지 확인하였다.

```text
kyu@localhost:~/abc$ rm ./b2
kyu@localhost:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
```

### 4) 파일 내용 확인, 빈 파일 생성

#### 4-1) 빈 파일 생성

`touch` 명령어를 사용하여 `test`라는 빈 파일을 생성하였다.  
이후 `cat` 명령어로 파일 내용을 확인했지만, 빈 파일이기 때문에 출력되는 내용은 없었다.

```text
kyu@localhost:~/abc$ touch test
kyu@localhost:~/abc$ cat ./test
```

#### 4-2) 파일 내용 확인

`cat` 명령어를 사용하여 `install_docker.sh` 파일의 내용을 확인하였다.  
출력 내용이 길기 때문에 README에는 일부 내용을 생략하여 기록하였다.

```text
kyu@localhost:~$ cat ./install_docker.sh 
#!/usr/bin/env bash
set -e

echo "=== Docker 설치 스크립트 시작 ==="

# ... 중략 ...

echo "=== Docker 설치 완료 ==="
```

## 3. 권한 실습 및 증거 기록

### 1) 권한 확인/변경


리눅스 권한은 숫자로 표현할 수 있으며, 읽기 권한은 4, 쓰기 권한은 2, 실행 권한은 1이다.  

권한 숫자의 각 자리는 다음을 의미한다.

- 백의 자리: 소유자 권한
- 십의 자리: 그룹 권한
- 일의 자리: 기타 사용자 권한

예를 들어 `chmod 600 a1`은 소유자에게 읽기(4)와 쓰기(2) 권한을 부여하고, 그룹과 기타 사용자에게는 아무 권한도 부여하지 않는다는 의미이다.


#### 1-1) 파일

`ls -l` 명령어를 사용하여 파일의 권한을 확인하였다.  
이후 `chmod 600 a1` 명령어를 사용하여 `a1` 파일의 권한을  변경하였다.

```text
kyu@localhost:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:57 test
kyu@localhost:~/abc$ chmod 600 a1
kyu@localhost:~/abc$ ls -al
total 8
drwxr-xr-x 2 kyu kyu 4096 Nov  4 09:57 .
drwxr-x--- 5 kyu kyu 4096 Nov  4 09:47 ..
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
```
`a1` 파일의 권한이 `-rw-r--r--`에서 `-rw-------`로 변경된 것을 확인하였다.

#### 1-2) 폴더

`ls -al` 명령어를 사용하여 디렉터리의 권한을 확인하였다.  
이후 `chmod 700 ./aaa` 명령어를 사용하여 `aaa` 디렉터리의 권한을 소유자만 읽기/쓰기/실행 가능하도록 변경하였다.

```text
kyu@localhost:~/abc$ ls -al
total 12
drwxr-xr-x 3 kyu kyu 4096 Nov  4 10:25 ./
drwxr-x--- 5 kyu kyu 4096 Nov  4 09:47 ../
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
dr-x------ 2 kyu kyu 4096 Nov  4 10:27 aaa/
-rwx------ 1 kyu kyu    0 Nov  4 09:51 abc*
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
kyu@localhost:~/abc$ chmod 700 ./aaa
kyu@localhost:~/abc$ ls -l
total 4
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
drwx------ 2 kyu kyu 4096 Nov  4 10:27 aaa
-rwx------ 1 kyu kyu    0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
```

`aaa` 디렉터리의 권한이 `dr-x------`에서 `drwx------`로 변경된 것을 확인하였다.

## 4. Docker 설치 및 기본 점검

`docker --version` 명령어를 사용하여 Docker가 설치되어 있는지와 설치된 버전을 확인하였다.

### 1) 버전

```text
kyu@localhost:~/abc$ docker --version
Docker version 29.9.3, build 5457199
```

### 2) 데몬 동작 여부 확인

`docker info` 명령어를 사용하여 Docker 클라이언트와 서버 정보가 정상적으로 출력되는지 확인하였다.  
`Server` 항목이 출력되므로 Docker 데몬이 정상적으로 동작하고 있음을 확인할 수 있었다.

```text
kyu@localhost:~/abc$ docker info
Client: Docker Engine - Community
 Version:    29.9.3
 Context:    default
 Debug Mode: false
 Plugins:
  buildx: Docker Buildx (Docker Inc.)
    Version:  v0.38.1
    Path:     /usr/libexec/docker/cli-plugins/docker-buildx
  compose: Docker Compose (Docker Inc.)
    Version:  v5.7.0
    Path:     /usr/libexec/docker/cli-plugins/docker-compose

Server:
 Containers: 1
  Running: 0
  Paused: 0
  Stopped: 1
 Images: 1
 Server Version: 29.9.3

... 생략 ...
```

## 5. Docker 기본 운영 명령 수행

### 1) 이미지 다운로드/목록 확인

#### 1-1) 이미지 다운로드

`docker pull hello-world` 명령어를 사용하여 `hello-world` 이미지를 다운로드하였다.


```text
kyu@localhost:~/abc$ docker pull hello-world
Using default tag: latest
latest: Pulling from library/hello-world
4f55086f7dd0: Pull complete 
d5e71e642bf5: Download complete 
Digest: sha256:c567b231f65614688711a75f31518a064258a51d442117a3e771a3a8fdba78ef
Status: Downloaded newer image for hello-world:latest
docker.io/library/hello-world:latest
```

#### 1-2) 이미지 목록 확인

`docker images` 명령어를 사용하여 로컬에 저장된 Docker 이미지 목록을 확인하였다.

```text
kyu@localhost:~/abc$ docker images

IMAGE                ID             DISK USAGE   CONTENT SIZE   EXTRA
hello-world:latest   c567b231f656       25.9kB         9.49kB    U  
```
위 결과를 통해 hello-world:latest 이미지가 로컬에 저장된 것을 확인하였다.


### 2) 컨테이너 : 실행/중지/목록 확인

컨테이너 확인을 위해 `docker run hello-world` 명령어를 사용하여 `hello-world` 컨테이너를 실행하였다.

`docker ps`는 현재 실행 중인 컨테이너만 보여준다.  
`docker ps -a`는 실행 중인 컨테이너와 종료된 컨테이너를 모두 보여준다.

`hello-world` 컨테이너는 실행 후 바로 종료되므로 별도의 `docker stop` 명령어를 사용하지 않았다.  
따라서 `docker ps`에는 보이지 않고, `docker ps -a`에서 `Exited (0)` 상태로 확인할 수 있었다.

```text
kyu@localhost:~/abc$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
kyu@localhost:~/abc$ docker ps -a
CONTAINER ID   IMAGE         COMMAND    CREATED         STATUS                     PORTS     NAMES
af96a2c1d0ff   hello-world   "/hello"   8 minutes ago   Exited (0) 8 minutes ago             romantic_bell
```

### 3) 운영 : 로그 확인, 리소스 확인

#### 3-1) 로그 확인

`docker logs` 명령어를 사용하여 `hello-world` 컨테이너의 실행 로그를 확인하였다.

```text
kyu@localhost:~/abc$ docker logs af96a2c1d0ff

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 
 ... 생략 ...
```

#### 3-2) 리소스 확인

리소스 확인을 위해 Ubuntu 컨테이너를 실행하였다.

```bash
docker run -it --name ubuntu-test ubuntu bash
```

`docker stats --no-stream` 명령어를 사용하여 실행 중인 ubuntu 컨테이너의 리소스 사용량을 한 번만 출력하여 확인하였다.

```text
kyu@localhost:~$ docker stats --no-stream 059dfa2d84f9
CONTAINER ID   NAME          CPU %     MEM USAGE / LIMIT    MEM %     NET I/O         BLOCK I/O     PIDS
059dfa2d84f9   ubuntu-test   0.00%     3.02MiB / 15.25GiB   0.02%     1.53kB / 126B   20.2MB / 0B   1
```

`docker stats`를 통해 실행 중인 컨테이너의 CPU 사용률, 메모리 사용량, 네트워크 입출력, 블록 입출력, 프로세스 수를 확인하였다.

## 6. 컨테이너 실행 실습

### 1) hello-world 실행

```text
kyu@localhost:~/abc$ docker run hello-world

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

### 2) ubuntu 실행 및 명령어 입력
```text
kyu@localhost:~/abc$ docker run -it ubuntu bash
root@505fe645233a:/# pwd
/
root@505fe645233a:/# ls
bin   dev  home  lib64  mnt  proc  run   srv  tmp  var
boot  etc  lib   media  opt  root  sbin  sys  usr
root@505fe645233a:/# cd ./home
root@505fe645233a:/home# echo 'Hello'
Hello
```

### 3) 컨테이너 종료/유지 차이

#### 3-1) 종료되는 경우

Ubuntu 컨테이너 안에서 `exit`을 입력하면 컨테이너가 종료된다.

```text
kyu@localhost:~/abc$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED              STATUS                      PORTS     NAMES
65f994892265   ubuntu    "bash"    About a minute ago   Exited (0) 12 seconds ago             ubuntu-test1
```

#### 3-2) 유지되는 경우

컨테이너 안에서 `exit`을 입력하지 않고 `Ctrl + P`, `Ctrl + Q`로 빠져나오면 컨테이너는 계속 실행된다.

```text
kyu@localhost:~$ docker run -it --name ubuntu-test ubuntu bash
root@ceeac2d4b7cd:/# 
```

`Ctrl + P`, `Ctrl + Q`로 빠져나온 뒤 확인하였다.

```text
kyu@localhost:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED          STATUS          PORTS     NAMES
ceeac2d4b7cd   ubuntu    "bash"    32 seconds ago   Up 32 seconds             ubuntu-test
```
`STATUS`가 `Up`으로 표시되므로 컨테이너가 종료되지 않고 유지되고 있음을 확인하였다.


#### 3-3) attach/exec

`docker attach ubuntu-test` 명령어를 사용하여 실행 중인 컨테이너의 메인 프로세스에 접속하였다.  
이 상태에서 `exit` 명령어를 입력하면 컨테이너의 메인 프로세스가 종료되므로 컨테이너도 함께 종료됨을 확인하였다.

```text
kyu@localhost:~$ docker attach ubuntu-test
root@059dfa2d84f9:/# exit
exit
kyu@localhost:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
kyu@localhost:~$ docker ps -a
CONTAINER ID   IMAGE     COMMAND   CREATED             STATUS                     PORTS     NAMES
059dfa2d84f9   ubuntu    "bash"    About an hour ago   Exited (0) 4 seconds ago             ubuntu-test
```

종료된 컨테이너를 다시 실행하였다.

```bash
docker start ubuntu-test
```

`docker exec -it ubuntu-test <command>` 명령어를 사용하면 실행 중인 컨테이너 안에서 새로운 프로세스를 실행할 수 있다.  
예를 들어 `bash`를 실행한 경우, `exit`로 bash을 종료해도 컨테이너의 메인 프로세스는 종료되지 않으므로 컨테이너가 계속 실행됨을 확인하였다.

```text
kyu@localhost:~$ docker exec -it ubuntu-test bash
root@059dfa2d84f9:/# exit
exit
kyu@localhost:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED             STATUS          PORTS     NAMES
059dfa2d84f9   ubuntu    "bash"    About an hour ago   Up 55 seconds             ubuntu-test
```


## 7. 기존 Dockerfile 기반 커스텀 이미지 제작

## 8. 포트 매핑 및 접속 증거

## 9. Docker 볼륨 영속성 검증

## 10. Git 설정 및 GitHub 연동

## 11. 보안 및 개인정보 보호

## 보너스 과제
### 1) Docker Compose 기초
### 2) Docker Compose 멀티 컨테이너
### 3) compose 운영 명령어 습득
### 4) 환경 변수 활용
### 5) GitHub SSH 키 설정
