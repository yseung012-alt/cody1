# Linux & Docker 실습 프로젝트

## 1. 프로젝트 소개

Linux 기본 명령어와 Docker의 주요 기능을 직접 실습하고, Git과 GitHub를 이용해 학습 과정을 기록하는 프로젝트입니다.

**실습 목표**
- Linux 파일 및 디렉터리 관리 명령어 익히기
- 파일 접근 권한 이해하기
- Docker 이미지와 컨테이너 실행하기
- Dockerfile을 이용한 웹 서버 구축하기
- Bind Mount와 Volume의 차이 이해하기
- Git과 GitHub를 이용한 버전 관리

## 2. 실습 환경

| 구분 | 사용 환경 |
|---|---|
| 운영체제 | Windows + WSL2 |
| Linux | Ubuntu |
| 컨테이너 | Docker Desktop |
| 웹 서버 | Nginx |
| 편집기 | Visual Studio Code |
| 버전 관리 | Git / GitHub |

## 3. 프로젝트 구조

```text
cody1/
├── README.md
├── .gitignore
├── docker-info.txt
├── linux-evidence/
│   └── permission-test.txt
├── practice/
│   ├── hello.txt
│   ├── hello_backup.txt
│   └── perdem/
├── webserver/
│   ├── Dockerfile
│   └── index.html
├── bindmount/
│   └── index.html
└── screenshots/
    ├── nginx-webserver.png
    ├── bindmount-result.png
    └── docker-volume-result.png
```
## 4. Linux 기본 명령어 실습

### Linux 파일 및 디렉터리 관리 실행 결과

Linux에서 디렉터리와 파일을 생성하고, 이름을 변경한 뒤 삭제하는 과정을 실습했습니다.

**1. 디렉터리 생성**

```bash
mkdir linux-evidence
ls -ld linux-evidence
```

실행 결과:

```text
drwxr-xr-x 2 user user 4096 Nov 19 14:17 linux-evidence
```

**2. 파일 생성**

```bash
touch linux-evidence/test.txt
ls -l linux-evidence/
```

실행 결과:

```text
total 0
-rw-r--r-- 1 user user 0 Nov 19 14:18 test.txt
```

**3. 파일 이름 변경**

```bash
mv linux-evidence/test.txt linux-evidence/renamed.txt
ls -l linux-evidence/
```

실행 결과:

```text
total 0
-rw-r--r-- 1 user user 0 Nov 19 14:18 renamed.txt
```

**4. 파일 삭제**

```bash
rm linux-evidence/renamed.txt
ls -l linux-evidence/
```

실행 결과:

```text
total 0
```

**실습 결과**

`mkdir`, `touch`, `mv`, `rm` 명령어로 파일과 디렉터리를 관리하고, `ls` 명령어를 통해 각 작업의 결과를 확인했습니다.

### 파일 및 디렉터리 관리

```bash
mkdir practice
cd practice
touch hello.txt
echo "hello Linux!" > hello.txt
cat hello.txt
cp hello.txt hello_copy.txt
mv hello_copy.txt hello_backup.txt
ls -l
```

파일 생성, 내용 출력, 복사, 이름 변경 등의 명령어를 실습했습니다.

### 파일 권한 변경

```bash
chmod 755 hello.txt
chmod 644 hello.txt

mkdir perdem
chmod 700 perdem
ls -ld perdem
```

`chmod`를 이용하여 소유자, 그룹, 기타 사용자에게 부여되는 읽기(r), 쓰기(w), 실행(x) 권한을 확인했습니다.

### Linux 파일 권한 변경 및 확인

`chmod` 명령어로 파일 권한을 변경하고, `ls -l` 명령어로 실제 변경 결과를 확인했습니다.

**1. 권한 변경 전 (644)**

```bash
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rw-r--r-- 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**2. 권한을 755로 변경**

```bash
chmod 755 linux-evidence/permission-test.txt
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rwxr-xr-x 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**3. 권한을 다시 644로 변경**

```bash
chmod 644 linux-evidence/permission-test.txt
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rw-r--r-- 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**4. Linux 권한의 숫자 계산법**

| 권한 | 의미 | 숫자 |
|---|---|---|
| r | 읽기(Read) | 4 |
| w | 쓰기(Write) | 2 |
| x | 실행(Execute) | 1 |

Linux 권한은 소유자(User), 그룹(Group), 기타 사용자(Others)의 순서로 표시됩니다.

- **755** = 7(4+2+1), 5(4+1), 5(4+1) → `rwxr-xr-x`
- **644** = 6(4+2), 4, 4 → `rw-r--r--`

`chmod 755`로 실행 권한을 추가하고, `chmod 644`로 실행 권한을 제거하는 과정을 확인했습니다.

## 5. Docker 기본 실습

### Docker 설치 및 데몬 실행 확인

Docker 버전과 데몬 실행 상태를 확인했습니다.

**1. Docker 버전 확인**

```bash
sudo docker --version
```

실행 결과:

```text
Docker version 29.9.2, build a82e528
```

**2. Docker 데몬 상태 확인**

```bash
sudo docker info
```

실행 결과 중 주요 정보:

```text
Server:
 Containers: 5
  Running: 2
  Paused: 0
  Stopped: 3
 Images: 7
 Server Version: 29.9.2
```

Docker Client에서 Server 정보를 정상적으로 조회할 수 있으므로 Docker 데몬이 정상적으로 작동하고 있음을 확인했습니다.

전체 실행 로그: [docker-info.txt](docker-info.txt)

### Hello World 컨테이너 실행

```bash
sudo docker run hello-world
```

Docker가 정상적으로 설치되고 실행되는지 확인했습니다.

### hello-world 컨테이너 실행 결과

Docker가 정상적으로 컨테이너를 생성하고 실행할 수 있는지 확인하기 위해 다음 명령어를 실행했습니다.

```bash
sudo docker run --rm hello-world
```

**실제 실행 결과:**

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

실행 결과를 통해 Docker Client가 Docker Daemon과 통신하고, 이미지를 기반으로 컨테이너를 생성 및 실행할 수 있음을 확인했습니다.

`--rm` 옵션을 사용하여 테스트 컨테이너가 종료된 후 자동으로 삭제되도록 했습니다.

### Ubuntu 컨테이너 실행

```bash
sudo docker run -it --name ubuntu-test ubuntu bash
cat /etc/os-release
exit
```

Ubuntu 컨테이너에 접속하여 운영체제 정보를 확인하고 종료했습니다.

### Docker 컨테이너 및 이미지 목록 확인

Docker에서 생성된 컨테이너와 저장된 이미지를 확인했습니다.

**1. 전체 컨테이너 목록 확인**

```bash
sudo docker ps -a
```

실제 실행 결과:

```text
CONTAINER ID   IMAGE          STATUS                         PORTS                  NAMES
8bb0450f5e51   nginx:alpine   Exited (255) 20 minutes ago    0.0.0.0:8082->80/tcp   bindmount-test
9174e0130be2   cody1-web      Exited (255) 20 minutes ago    0.0.0.0:8081->80/tcp   cody1-web-container
67d3ad75d2d2   ubuntu         Exited (0) 2 days ago                                 ubuntu-test
33f58c03f9fb   hello-world    Exited (0) 2 days ago                                 stoic_buck
```

`docker ps -a` 명령어로 실행 중인 컨테이너뿐 아니라 종료된 컨테이너까지 확인했습니다.

**2. Docker 이미지 목록 확인**

```bash
sudo docker images
```

실제 실행 결과를 정리한 표입니다.

| IMAGE | IMAGE ID | DISK USAGE |
|---|---|---|
| cody1-web:latest | 076d22702503 | 93.7MB |
| hello-world:latest | c567b231f656 | 25.9kB |
| my-web:latest | 7ec3599729ab | 241MB |
| nginx:alpine | fed7e14be83c | 94.5MB |
| nginx:latest | 165ddde8751e | 243MB |
| ubuntu:latest | 6a06111c3b0a | 163MB |

**3. 이미지와 컨테이너의 차이**

Docker 이미지는 컨테이너를 생성하기 위한 실행 환경과 파일을 담고 있는 템플릿입니다.

Docker 컨테이너는 이미지를 기반으로 생성된 실행 인스턴스입니다.

이번 실습에서는 `cody1-web` 이미지를 빌드한 뒤 `cody1-web-container`라는 컨테이너를 생성했습니다.

따라서 하나의 이미지로 여러 컨테이너를 생성할 수 있으며, 컨테이너를 삭제해도 이미지는 별도로 삭제하지 않는 한 유지됩니다.

## 6. Dockerfile을 이용한 웹 서버 구축

`webserver/Dockerfile`을 작성했습니다.

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

Docker 이미지 빌드:

```bash
cd ~/cody1/webserver
sudo docker build -t cody1-web .
```

컨테이너 실행:

```bash
sudo docker run -d --name cody1-web-container -p 8081:80 cody1-web
```

브라우저에서 `http://localhost:8081`로 접속하여 웹 페이지가 정상적으로 표시되는 것을 확인했습니다.

### 웹 서버 실행 결과

![Nginx 웹 서버 실행 화면](screenshots/nginx-webserver.png)

## 7. Docker Bind Mount 실습

Ubuntu의 `bindmount` 폴더를 Nginx 컨테이너의 웹 페이지 디렉터리에 연결했습니다.

```bash
sudo docker run -d \
  --name bindmount-test \
  -p 8082:80 \
  -v /home/user/cody1/bindmount:/usr/share/nginx/html:ro \
  nginx:alpine
```

VS Code에서 `bindmount/index.html`을 수정하고 저장한 뒤, `http://localhost:8082`를 새로고침했습니다.

### Bind Mount 실행 결과

![Docker Bind Mount 실행 화면](screenshots/bindmount-result.png)

**실습 결과:** 이미지를 다시 빌드하지 않아도 수정한 HTML 내용이 웹 페이지에 반영되는 것을 확인했습니다.

## 8. Docker Volume 실습

### Volume 생성

```bash
sudo docker volume create cody1-data
sudo docker volume ls
```

### 컨테이너에서 파일 저장

```bash
sudo docker run -it --name volume-test -v cody1-data:/data ubuntu bash
```

컨테이너 내부에서:

```bash
echo "Docker Volume Test" > /data/test.txt
cat /data/test.txt
exit
```

### 컨테이너 삭제 후 데이터 확인

```bash
sudo docker rm volume-test
sudo docker run --rm -v cody1-data:/data ubuntu cat /data/test.txt
```

출력 결과:

```text
Docker Volume Test
```

**실습 결과:** 기존 컨테이너를 삭제한 후에도 Docker Volume에 저장한 데이터가 유지되는 것을 확인했습니다.

### Docker Volume 실행 결과

![Docker Volume 데이터 유지 확인](screenshots/docker-volume-result.png)

## 9. Git 및 GitHub

Git 저장소를 초기화하고 `main` 브랜치에서 프로젝트 파일을 관리했습니다.

```bash
git init
git branch -m main
git add .
git commit -m "Initial commit: Linux and Docker practice"
```

GitHub 원격 저장소 연결:

```bash
git remote add origin https://github.com/yseung012-alt/cody1.git
git remote -v
```

### GitHub Push 성공 증빙

로컬 Git 저장소의 변경 사항을 GitHub 원격 저장소에 업로드했습니다.

```bash
git push
```

실제 실행 결과:

```text
To https://github.com/yseung012-alt/cody1.git
   8f2911a..b3f2fb4  main -> main
```

위 출력에서 `main -> main`은 로컬 `main` 브랜치의 커밋이 원격 저장소에 반영되었다는 의미입니다.

- GitHub 저장소: https://github.com/yseung012-alt/cody1
- 푸시 성공 커밋: https://github.com/yseung012-alt/cody1/commit/b3f2fb4

## 10. 오류 해결 과정 (Troubleshooting)

### Docker 명령어 실행 오류

**문제:** Ubuntu에서 Docker 명령어를 찾지 못하거나 Docker 소켓에 접근할 권한이 없었습니다.

**해결:** Docker Desktop의 WSL Integration을 활성화하고, 필요한 경우 `sudo docker`로 실행했습니다.

### 웹 서버 포트 연결 오류

**문제:** 웹 서버 컨테이너를 실행할 때 호스트 포트 `8080` 연결에 실패했습니다.

**해결:** 생성된 컨테이너를 정리한 뒤 호스트 포트를 `8081`로 변경하여 실행했습니다.

### Bind Mount 경로 오류

**문제:** 컨테이너 내부 연결 경로가 절대 경로가 아니어서 실행에 실패했습니다.

**해결:** `/usr/share/nginx/html`처럼 `/`로 시작하는 절대 경로로 수정했습니다.

### GitHub Push 인증 및 권한 오류

**문제:** `git push` 실행 시 인증 오류와 `403 Permission denied` 오류가 발생했습니다.

**원인:** GitHub는 일반 계정 비밀번호를 이용한 Git 인증을 지원하지 않으며, 처음 발급한 Personal Access Token(PAT)에 저장소 쓰기 권한이 없었습니다.

**해결:**
1. GitHub에서 Fine-grained Personal Access Token을 발급했습니다.
2. Repository access를 `Only select repositories`로 변경하고 `cody1`을 선택했습니다.
3. Repository permissions의 Contents 권한을 `Read and write`로 설정했습니다.
4. 수정한 토큰으로 다시 인증하여 `git push -u origin main`을 성공적으로 실행했습니다.

**결과:** README.md와 실습 파일을 GitHub 원격 저장소에 업로드했습니다.

## 11. 실습 결과 스크린샷

다음 실습 결과를 스크린샷으로 기록하고 각 실습 항목에 첨부했습니다.

- Nginx 웹 서버 접속 화면: 6번 항목
- Docker Bind Mount HTML 수정 결과: 7번 항목
- Docker Volume 데이터 유지 확인 결과: 8번 항목

Docker Hello World 실행 결과는 5번 항목에 실제 터미널 출력으로 기록했습니다.

## 12. Docker 심화 학습 및 문제 해결

### 1. Docker 이미지와 컨테이너의 차이 (#12)

Docker 이미지는 애플리케이션 실행에 필요한 파일과 설정을 포함한 템플릿이고, 컨테이너는 이미지를 기반으로 생성된 실행 인스턴스입니다.

이번 실습에서는 `webserver/Dockerfile`에 `FROM nginx:alpine`과 `COPY` 명령어를 작성하고, `docker build -t cody1-web .` 명령어로 이미지를 빌드했습니다.

이후 `docker run -d --name cody1-web-container -p 8081:80 cody1-web` 명령어로 컨테이너를 실행했습니다.

- **이미지 빌드:** Dockerfile을 기반으로 `cody1-web` 이미지 생성
- **컨테이너 실행:** 생성한 이미지로 Nginx 웹 서버 실행
- **파일 변경:** Dockerfile의 `COPY`로 포함한 HTML 파일을 변경하려면 이미지를 다시 빌드하고 컨테이너를 새로 생성해야 변경 사항이 반영됩니다.
- **Bind Mount:** 호스트의 HTML 파일을 연결하면 이미지를 다시 빌드하지 않고도 수정 사항을 반영할 수 있습니다.

### 2. 네트워크 네임스페이스와 포트포워딩 (#13)

Docker의 일반적인 브리지 네트워크 환경에서 컨테이너는 별도의 네트워크 네임스페이스를 사용합니다. 따라서 컨테이너는 호스트와 구분되는 네트워크 인터페이스와 IP 주소, 포트 공간을 가질 수 있습니다.

이번 실습에서는 다음 옵션을 사용했습니다.

```bash
-p 8081:80
```

- `8081`: 호스트에서 접근하는 포트
- `80`: 컨테이너 내부 Nginx가 사용하는 포트

브라우저에서 `http://localhost:8081`에 접속하면 Docker의 포트 게시 기능을 통해 컨테이너의 80번 포트로 연결됩니다.

즉, 호스트와 컨테이너의 포트 번호가 같을 필요는 없습니다.

### 3. 포트 충돌 진단 방법 (#16)

처음 웹 서버를 실행할 때 호스트의 8080번 포트를 사용하려 했으나 포트 충돌이 발생했습니다. 이후 호스트 포트를 8081로 변경하여 정상적으로 실행했습니다.

포트 충돌이 발생하면 다음 명령어로 점검할 수 있습니다.

```bash
sudo ss -ltnp
sudo docker ps
```

`ss -ltnp`는 TCP 수신 포트와 관련 프로세스를 확인하는 데 사용하고, `docker ps`는 실행 중인 Docker 컨테이너의 포트 매핑을 확인하는 데 사용합니다.

이 명령어는 포트 충돌을 진단하기 위한 방법이며, 최초 오류 당시의 실제 진단 로그는 확보하지 못했습니다.

### 4. Docker Volume 데이터 보호 방법 (#17)

Docker Volume은 컨테이너와 별도로 관리되므로 컨테이너를 삭제하더라도 Volume 자체가 유지되면 데이터를 보존할 수 있습니다.

하지만 Volume도 사용자가 삭제하거나 저장 장치에 장애가 발생하면 데이터가 손실될 수 있습니다.

따라서 중요한 데이터를 저장할 때는 다음과 같은 보호 방법이 필요합니다.

1. 정기적으로 Volume 데이터를 별도 파일로 백업합니다.
2. 백업 파일을 원본과 다른 저장 위치에 보관합니다.
3. 백업 파일을 이용해 실제로 복원할 수 있는지 검증합니다.
4. 데이터 중요도에 따라 백업 주기와 보관 기간을 정합니다.

이번 실습에서는 `cody1-data` Volume을 사용해 컨테이너 삭제 후에도 `test.txt` 파일이 유지되는 것을 확인했습니다.

## 13. 학습 내용

이번 실습을 통해 Linux 환경에서 파일과 권한을 관리하는 방법을 익혔습니다.

Docker를 이용해 컨테이너를 실행하고, Dockerfile로 이미지를 직접 빌드했으며, Bind Mount와 Volume을 활용해 파일을 연결하고 데이터를 유지하는 방법을 확인했습니다.

또한 Git을 이용해 실습 파일의 변경 이력을 관리하고 GitHub에 프로젝트를 정리하는 과정을 학습했습니다.
