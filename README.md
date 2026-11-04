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
 - [ ] Docker 설치/점검
 - [ ] hello-world 실행
 - [ ] Dockerfile 빌드/실행
 - [ ] 포트 매핑 접속(2회)
 - [ ] 바인드 마운트 반영
 - [ ] 볼륨 영속성
 - [ ] Git 설정 + VSCode GitHub 연동

## 2. 터미널 조작 로그 기록

### 1) 현재 위치 확인 
```text
kyu@GNIA-C202-E06:~$ pwd
/home/kyu
```
### 2) 목록 확인(숨김 파일 포함)
```text
kyu@GNIA-C202-E06:~$ ls -al
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

 - 생성

```text
kyu@GNIA-C202-E06:~$ mkdir abc
kyu@GNIA-C202-E06:~$ ls
abc  install_docker.sh
```
 - 이동

```text
kyu@GNIA-C202-E06:~$ cd ./abc
kyu@GNIA-C202-E06:~/abc$ pwd
/home/kyu/abc
```

 - 복사

```text
kyu@GNIA-C202-E06:~/abc$ touch a1
kyu@GNIA-C202-E06:~/abc$ cp ./a1 ./a2
kyu@GNIA-C202-E06:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a2
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
```

 - 이름 변경

```text
kyu@GNIA-C202-E06:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a2
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc

kyu@GNIA-C202-E06:~/abc$ mv ./a2 ./b2
kyu@GNIA-C202-E06:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 b2
```
 - 삭제

```text
kyu@GNIA-C202-E06:~/abc$ rm ./b2
kyu@GNIA-C202-E06:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
```

### 4) 파일 내용 확인, 빈 파일 생성

 - 빈 파일 생성

```text
kyu@GNIA-C202-E06:~/abc$ touch test
kyu@GNIA-C202-E06:~/abc$ cat ./test
```
 - 파일 내용 확인

```text
kyu@GNIA-C202-E06:~$ cat ./install_docker.sh 
#!/usr/bin/env bash
set -e

echo "=== Docker 설치 스크립트 시작 ==="

# ... 중략 ...

echo "=== Docker 설치 완료 ==="
```

## 3. 권한 실습 및 증거 기록

### 1) 권한 확인/변경
 - 파일

```text
kyu@GNIA-C202-E06:~/abc$ ls -l
total 0
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu 0 Nov  4 09:57 test
kyu@GNIA-C202-E06:~/abc$ chmod 600 a1
kyu@GNIA-C202-E06:~/abc$ ls -al
total 8
drwxr-xr-x 2 kyu kyu 4096 Nov  4 09:57 .
drwxr-x--- 5 kyu kyu 4096 Nov  4 09:47 ..
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
```

 - 폴더

```text
kyu@GNIA-C202-E06:~/abc$ ls -al
total 12
drwxr-xr-x 3 kyu kyu 4096 Nov  4 10:25 ./
drwxr-x--- 5 kyu kyu 4096 Nov  4 09:47 ../
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
dr-x------ 2 kyu kyu 4096 Nov  4 10:27 aaa/
-rwx------ 1 kyu kyu    0 Nov  4 09:51 abc*
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
kyu@GNIA-C202-E06:~/abc$ chmod 700 ./aaa
kyu@GNIA-C202-E06:~/abc$ ls -l
total 4
-rw------- 1 kyu kyu    0 Nov  4 09:53 a1
drwx------ 2 kyu kyu 4096 Nov  4 10:27 aaa
-rwx------ 1 kyu kyu    0 Nov  4 09:51 abc
-rw-r--r-- 1 kyu kyu    0 Nov  4 09:57 test
```

## 4. Docker 설치 및 기본 점검


![alt text](version-1.png)

```text
D:\>docker --version
Docker version 29.9.2, build a82e528
```

## 5. Docker 기본 운영 명령 수행

## 6. 컨테이너 실행 실습

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
