# Linux & Docker 실습 프로젝트

## 1. 프로젝트 소개

Linux 기본 명령어와 Docker의 주요 기능을 직접 실습하고, Git과 GitHub를 이용해 학습 과정을 기록하는 프로젝트입니다.

### 실습 목표

- Linux 파일 및 디렉터리 관리 명령어 익히기
- Linux 파일 권한과 `chmod` 사용법 이해하기
- Docker 설치 및 컨테이너 실행 확인하기
- Docker 이미지와 컨테이너의 차이 이해하기
- Dockerfile을 이용한 Nginx 웹 서버 구축하기
- Docker 포트 매핑과 네트워크 개념 이해하기
- Bind Mount와 Volume을 활용한 데이터 관리
- Git과 GitHub를 이용한 버전 관리
- 실습 과정에서 발생한 오류와 해결 방법 기록하기

---

## 2. 실습 환경

| 구분 | 사용 환경 |
|---|---|
| 운영체제 | Windows + WSL2 |
| Linux | Ubuntu |
| 컨테이너 환경 | Docker Desktop |
| 웹 서버 | Nginx |
| 코드 편집기 | Visual Studio Code |
| 버전 관리 | Git / GitHub |
| GitHub 저장소 | https://github.com/yseung012-alt/cody1 |

Windows에서 WSL2를 이용해 Ubuntu 환경을 구성하고, Ubuntu 터미널에서 Linux 명령어와 Docker 실습을 진행했습니다.

Docker Desktop의 WSL Integration 기능을 이용하여 Ubuntu 환경에서도 Docker 명령어를 실행했습니다.

---

## 3. 프로젝트 디렉터리 구조

```text
cody1/
├── README.md
├── .gitignore
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

### 디렉터리별 역할

| 디렉터리 및 파일 | 역할 |
|---|---|
| `README.md` | 실습 과정과 결과 기록 |
| `.gitignore` | Git 추적에서 제외할 파일 설정 |
| `linux-evidence/` | Linux 명령어 및 권한 실습 증빙 |
| `practice/` | Linux 파일·디렉터리 관리 실습 |
| `webserver/` | Nginx Docker 이미지 빌드 실습 |
| `bindmount/` | Bind Mount를 이용한 HTML 수정 실습 |
| `screenshots/` | 웹 서버, Bind Mount, Volume 실습 결과 이미지 |

---

## 4. Linux 기본 명령어 실습

### 4.1 파일 및 디렉터리 관리

Linux에서 디렉터리와 파일을 생성하고, 파일 내용을 확인하거나 이름을 변경하는 명령어를 실습했습니다.

**디렉터리 생성**

```bash
mkdir practice
cd practice
```

**파일 생성 및 내용 확인**

```bash
touch hello.txt
echo "hello Linux!" > hello.txt
cat hello.txt
```

**파일 복사 및 이름 변경**

```bash
cp hello.txt hello_copy.txt
mv hello_copy.txt hello_backup.txt
ls -l
```

위 명령어를 통해 파일 생성, 내용 출력, 복사, 이름 변경 방법을 학습했습니다.

### 4.2 파일 생성·이동·삭제 실행 결과

실제 명령어 실행 과정을 확인하기 위해 별도의 `linux-evidence` 디렉터리에서 추가 실습을 진행했습니다.

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

`mkdir`, `touch`, `mv`, `rm`, `ls` 명령어를 사용하여 파일과 디렉터리를 관리하고, 각 작업의 실행 결과를 확인했습니다.

### 4.3 Linux 파일 권한 변경

Linux 파일 권한은 소유자(User), 그룹(Group), 기타 사용자(Others)의 순서로 표시됩니다.

각 사용자에게 읽기, 쓰기, 실행 권한을 부여할 수 있습니다.

| 권한 | 의미 | 숫자 |
|---|---|---|
| r | 읽기(Read) | 4 |
| w | 쓰기(Write) | 2 |
| x | 실행(Execute) | 1 |

숫자 권한은 각 권한에 해당하는 숫자를 더하여 계산합니다.

**755 권한**

- 소유자: 4 + 2 + 1 = 7 → `rwx`
- 그룹: 4 + 1 = 5 → `r-x`
- 기타 사용자: 4 + 1 = 5 → `r-x`

결과: `rwxr-xr-x`

**644 권한**

- 소유자: 4 + 2 = 6 → `rw-`
- 그룹: 4 → `r--`
- 기타 사용자: 4 → `r--`

결과: `rw-r--r--`

### 4.4 chmod 실행 결과

**1. 변경 전 권한 확인**

```bash
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rw-r--r-- 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**2. 755 권한으로 변경**

```bash
chmod 755 linux-evidence/permission-test.txt
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rwxr-xr-x 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**3. 644 권한으로 변경**

```bash
chmod 644 linux-evidence/permission-test.txt
ls -l linux-evidence/permission-test.txt
```

실행 결과:

```text
-rw-r--r-- 1 user user 0 Nov 19 14:39 linux-evidence/permission-test.txt
```

**4. 디렉터리 권한 변경**

```bash
mkdir perdem
chmod 700 perdem
ls -ld perdem
```

`700`은 소유자에게만 읽기·쓰기·실행 권한을 부여하는 설정입니다.

**실습 결과**

`chmod`를 이용해 파일 권한을 변경하고, `ls -l` 명령어를 통해 실제 권한이 변경되었는지 확인했습니다.

---

## 5. Docker 기본 실습

### 5.1 Docker 설치 및 버전 확인

Docker Desktop을 설치하고 WSL2의 Ubuntu 환경에서 Docker 명령어가 실행되는지 확인했습니다.

실행 명령어:

```bash
sudo docker --version
```

실행 결과:

```text
Docker version 29.9.2, build a82e528
```

### 5.2 Docker 데몬 실행 확인

Docker는 명령을 전달하는 Client와 실제 컨테이너를 관리하는 Daemon(Server)으로 구성됩니다.

다음 명령어로 Docker Server 정보를 확인했습니다.

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

**실습 결과**

Docker Client에서 Server 정보를 정상적으로 조회할 수 있으므로 Docker 데몬이 동작하고 있음을 확인했습니다.

### 5.3 Hello World 컨테이너 실행

Docker가 정상적으로 이미지를 다운로드하고 컨테이너를 실행할 수 있는지 확인했습니다.

실행 명령어:

```bash
sudo docker run hello-world
```

실제 실행 결과:

```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
```

이 메시지는 Docker가 이미지를 기반으로 컨테이너를 생성하고 실행했다는 의미입니다.

테스트 컨테이너를 실행 후 자동 삭제하는 명령어도 사용했습니다.

```bash
sudo docker run --rm hello-world
```

`--rm` 옵션은 컨테이너가 종료되면 자동으로 삭제하도록 설정합니다.

### 5.4 Ubuntu 컨테이너 실행

Ubuntu 이미지를 사용하여 컨테이너 내부에 접속했습니다.

```bash
sudo docker run -it --name ubuntu-test ubuntu bash
```

컨테이너 내부에서 운영체제 정보를 확인했습니다.

```bash
cat /etc/os-release
```

이후 컨테이너에서 빠져나왔습니다.

```bash
exit
```

### 5.5 Docker 컨테이너 목록 확인

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

`docker ps -a`는 실행 중인 컨테이너와 종료된 컨테이너를 모두 표시합니다.

위 출력은 당시의 컨테이너 상태를 기록한 것으로, 웹 서버 실행을 확인했던 시점의 상태와는 다릅니다.

### 5.6 Docker 이미지 목록 확인

```bash
sudo docker images
```

실제 확인한 이미지 정보를 정리했습니다.

| 이미지 | IMAGE ID | 디스크 사용량 |
|---|---|---|
| cody1-web:latest | 076d22702503 | 93.7MB |
| hello-world:latest | c567b231f656 | 25.9kB |
| my-web:latest | 7ec3599729ab | 241MB |
| nginx:alpine | fed7e14be83c | 94.5MB |
| nginx:latest | 165ddde8751e | 243MB |
| ubuntu:latest | 6a06111c3b0a | 163MB |

**실습 결과**

Docker 이미지와 컨테이너를 목록으로 확인하고, 실행 중인 컨테이너와 종료된 컨테이너의 상태를 구분하는 방법을 학습했습니다.

---

## 6. Dockerfile을 이용한 Nginx 웹 서버 구축

### 6.1 Dockerfile 작성

`webserver` 디렉터리를 생성하고 Nginx 웹 서버를 실행하기 위한 Dockerfile을 작성했습니다.

파일 위치:

```text
webserver/Dockerfile
```

Dockerfile 내용:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
```

- `FROM nginx:alpine`: Nginx 기반 이미지 사용
- `COPY`: 작성한 HTML 파일을 컨테이너의 웹 페이지 디렉터리로 복사

### 6.2 Docker 이미지 빌드

```bash
cd ~/cody1/webserver
sudo docker build -t cody1-web .
```

`-t cody1-web`은 생성할 이미지에 이름을 지정하는 옵션입니다.

`.`은 현재 디렉터리를 빌드 컨텍스트로 사용한다는 의미입니다.

### 6.3 Nginx 컨테이너 실행

```bash
sudo docker run -d --name cody1-web-container -p 8081:80 cody1-web
```

옵션 설명:

| 옵션 | 의미 |
|---|---|
| `-d` | 백그라운드 실행 |
| `--name` | 컨테이너 이름 지정 |
| `-p 8081:80` | 호스트 8081번 포트와 컨테이너 80번 포트 연결 |
| `cody1-web` | 실행할 Docker 이미지 |

### 6.4 웹 서버 접속 확인

브라우저에서 다음 주소로 접속했습니다.

```text
http://localhost:8081
```

작성한 HTML 페이지가 정상적으로 표시되는 것을 확인했습니다.

![Nginx 웹 서버 실행 화면](screenshots/nginx-webserver.png)

**실습 결과**

Dockerfile을 이용해 이미지를 빌드하고, 생성한 이미지로 Nginx 컨테이너를 실행하여 웹 페이지에 접속했습니다.

---

## 7. Docker Bind Mount 실습

### 7.1 Bind Mount 개념

Bind Mount는 호스트 컴퓨터의 파일이나 디렉터리를 컨테이너 내부 경로에 연결하는 기능입니다.

호스트에서 파일을 수정하면 연결된 컨테이너에서도 변경된 내용을 사용할 수 있습니다.

### 7.2 Bind Mount 컨테이너 실행

Ubuntu의 `bindmount` 디렉터리를 Nginx 컨테이너의 웹 페이지 디렉터리에 연결했습니다.

```bash
sudo docker run -d \
  --name bindmount-test \
  -p 8082:80 \
  -v /home/user/cody1/bindmount:/usr/share/nginx/html:ro \
  nginx:alpine
```

옵션 설명:

- `/home/user/cody1/bindmount`: 호스트의 HTML 파일 디렉터리
- `/usr/share/nginx/html`: 컨테이너 내부의 Nginx 웹 페이지 디렉터리
- `:ro`: 컨테이너에서 해당 마운트를 읽기 전용으로 사용

### 7.3 HTML 파일 수정 및 확인

VS Code에서 다음 파일을 수정했습니다.

```text
bindmount/index.html
```

파일을 저장한 뒤 브라우저에서 다음 주소를 새로고침했습니다.

```text
http://localhost:8082
```

수정한 HTML 내용이 웹 페이지에 반영되는 것을 확인했습니다.

![Docker Bind Mount 실행 화면](screenshots/bindmount-result.png)

### 7.4 호스트 경로와 컨테이너 경로의 차이

호스트 경로는 실제 파일이 저장된 위치이고, 컨테이너 경로는 해당 파일을 컨테이너에서 사용할 위치입니다.

이번 실습에서는 Ubuntu의 HTML 파일을 Nginx가 웹 페이지를 제공하는 디렉터리에 연결했습니다.

Bind Mount 경로를 지정할 때는 실제 파일이 존재하는 호스트 경로와 애플리케이션이 사용하는 컨테이너 내부 경로를 확인해야 합니다.

**실습 결과**

이미지를 다시 빌드하지 않아도 호스트에서 수정한 HTML 파일의 내용이 웹 페이지에 반영되는 것을 확인했습니다.

---

## 8. Docker Volume 실습

### 8.1 Docker Volume 개념

Docker Volume은 Docker가 관리하는 데이터 저장 공간입니다.

컨테이너와 별도로 관리되므로 컨테이너를 삭제하더라도 Volume을 삭제하지 않으면 저장된 데이터를 유지할 수 있습니다.

### 8.2 Volume 생성

```bash
sudo docker volume create cody1-data
sudo docker volume ls
```

`cody1-data`라는 이름의 Volume을 생성하고 목록을 확인했습니다.

### 8.3 Volume에 데이터 저장

```bash
sudo docker run -it --name volume-test -v cody1-data:/data ubuntu bash
```

컨테이너 내부에서 파일을 생성했습니다.

```bash
echo "Docker Volume Test" > /data/test.txt
cat /data/test.txt
```

파일 내용을 확인한 뒤 컨테이너에서 나왔습니다.

```bash
exit
```

### 8.4 컨테이너 삭제 후 데이터 유지 확인

기존 컨테이너를 삭제했습니다.

```bash
sudo docker rm volume-test
```

이후 동일한 Volume을 새로운 컨테이너에 연결하여 파일을 확인했습니다.

```bash
sudo docker run --rm -v cody1-data:/data ubuntu cat /data/test.txt
```

실제 실행 결과:

```text
Docker Volume Test
```

![Docker Volume 데이터 유지 확인](screenshots/docker-volume-result.png)

**실습 결과**

컨테이너를 삭제해도 Docker Volume에 저장한 파일이 유지되는 것을 확인했습니다.

### 8.5 Bind Mount와 Volume 비교

| 구분 | Bind Mount | Volume |
|---|---|---|
| 저장 위치 | 사용자가 지정한 호스트 경로 | Docker가 관리하는 저장 공간 |
| 파일 수정 | 호스트에서 직접 수정 가능 | Docker를 통해 관리하는 것이 일반적 |
| 주요 용도 | 개발 중 파일 공유 | 데이터 영속성 관리 |
| 이번 실습 | HTML 수정 결과 반영 | 컨테이너 삭제 후 파일 유지 |

### 8.6 Docker Volume 데이터 보호

Volume은 컨테이너를 삭제해도 유지될 수 있지만, Volume 자체를 삭제하거나 저장 장치에 문제가 발생하면 데이터가 손실될 수 있습니다.

따라서 중요한 데이터는 다음과 같이 관리해야 합니다.

1. Volume 데이터를 정기적으로 별도 파일로 백업합니다.
2. 백업 파일을 원본과 다른 위치에 보관합니다.
3. 백업 파일로 실제 복원이 가능한지 확인합니다.
4. 데이터의 중요도에 따라 백업 주기와 보관 기간을 정합니다.

이번 실습에서는 데이터 유지 여부를 확인했으며, 별도의 백업·복구 실습은 진행하지 않았습니다.

---

## 9. Git 및 GitHub 실습

### 9.1 Git 저장소 초기화

프로젝트 디렉터리에서 Git 저장소를 초기화했습니다.

```bash
git init
git branch -m main
```

### 9.2 파일 추가 및 커밋

```bash
git add .
git commit -m "Initial commit: Linux and Docker practice"
```

Git을 이용하여 실습 파일의 변경 이력을 관리했습니다.

### 9.3 GitHub 원격 저장소 연결

```bash
git remote add origin https://github.com/yseung012-alt/cody1.git
git remote -v
```

### 9.4 GitHub Push

로컬 저장소의 커밋을 GitHub 원격 저장소에 업로드했습니다.

```bash
git push -u origin main
```

과거 Push 성공 로그:

```text
To https://github.com/yseung012-alt/cody1.git
   8f2911a..b3f2fb4  main -> main
```

`main -> main`은 로컬 `main` 브랜치의 커밋이 원격 저장소에 반영되었다는 의미입니다.

GitHub 저장소:

https://github.com/yseung012-alt/cody1

Push 성공 커밋:

https://github.com/yseung012-alt/cody1/commit/b3f2fb4

위 로그는 당시 Push에 성공한 기록이며, 이후 수정한 README의 업로드 성공을 의미하지는 않습니다.

---

## 10. Docker 심화 학습

### 10.1 Docker 이미지와 컨테이너의 차이

Docker 이미지는 애플리케이션 실행에 필요한 파일과 설정을 포함한 템플릿입니다.

Docker 컨테이너는 이미지를 기반으로 생성된 실행 인스턴스입니다.

이번 실습에서는 다음 과정을 진행했습니다.

**1. 이미지 생성**

```bash
sudo docker build -t cody1-web .
```

Dockerfile을 기반으로 `cody1-web` 이미지를 생성했습니다.

**2. 컨테이너 실행**

```bash
sudo docker run -d --name cody1-web-container -p 8081:80 cody1-web
```

이미지를 이용해 Nginx 컨테이너를 실행했습니다.

**3. 파일 변경**

Dockerfile의 `COPY` 명령으로 이미지에 포함한 HTML 파일을 수정하려면 일반적으로 이미지를 다시 빌드하고 새 컨테이너를 생성해야 변경 사항이 반영됩니다.

반면 Bind Mount를 이용하면 이미지를 다시 빌드하지 않고도 호스트에서 수정한 파일을 컨테이너에서 사용할 수 있습니다.

### 10.2 Docker 네트워크 네임스페이스

Docker의 일반적인 브리지 네트워크 환경에서 컨테이너는 별도의 네트워크 네임스페이스를 사용합니다.

따라서 컨테이너는 호스트와 구분되는 네트워크 인터페이스, IP 주소, 포트 공간을 가질 수 있습니다.

이번 실습에서는 다음 옵션을 사용했습니다.

```bash
-p 8081:80
```

- `8081`: 호스트에서 접근하는 포트
- `80`: 컨테이너 내부에서 Nginx가 사용하는 포트

브라우저에서 다음 주소로 접속했습니다.

```text
http://localhost:8081
```

Docker의 포트 게시 기능을 통해 호스트의 8081번 포트로 들어온 연결이 컨테이너의 80번 포트로 전달됩니다.

이를 통해 호스트와 컨테이너가 서로 다른 포트 번호를 사용할 수 있다는 것을 이해했습니다.

---

## 11. 오류 해결 과정 (Troubleshooting)

### 11.1 Docker 명령어 실행 오류

**문제**

Ubuntu에서 Docker 명령어를 찾지 못하거나 Docker 소켓에 접근할 권한이 없었습니다.

**해결**

Docker Desktop의 WSL Integration 기능을 활성화했습니다.

필요한 경우 `sudo docker` 명령어로 실행했습니다.

**결과**

Ubuntu 환경에서 Docker 명령어를 정상적으로 사용할 수 있었습니다.

### 11.2 웹 서버 포트 충돌

**문제**

처음 웹 서버를 실행할 때 호스트의 8080번 포트를 사용하려 했으나 포트 연결에 실패했습니다.

**해결**

호스트 포트를 8081로 변경하여 컨테이너를 실행했습니다.

```bash
sudo docker run -d --name cody1-web-container -p 8081:80 cody1-web
```

**결과**

브라우저에서 `http://localhost:8081`에 접속하여 웹 페이지를 확인했습니다.

### 11.3 포트 충돌 진단 방법

포트 충돌이 발생했을 때 사용할 수 있는 점검 명령어는 다음과 같습니다.

```bash
sudo ss -ltnp
sudo docker ps
```

- `ss -ltnp`: TCP 수신 포트와 관련 프로세스 확인
- `docker ps`: 실행 중인 Docker 컨테이너와 포트 매핑 확인

포트를 점유한 프로세스나 컨테이너를 확인한 뒤, 필요한 경우 해당 서비스를 중지하거나 다른 호스트 포트를 사용하여 충돌을 해결할 수 있습니다.

이번 실습에서는 호스트 포트를 변경하여 문제를 해결했습니다.

최초 오류 당시의 실제 포트 점유 진단 로그는 확보하지 못했습니다.

### 11.4 Bind Mount 경로 오류

**문제**

Bind Mount 실행 시 컨테이너 내부 연결 경로가 절대 경로가 아니어서 오류가 발생했습니다.

**해결**

컨테이너 내부 경로를 다음과 같이 절대 경로로 지정했습니다.

```text
/usr/share/nginx/html
```

호스트 경로도 다음과 같이 절대 경로를 사용했습니다.

```text
/home/user/cody1/bindmount
```

**결과**

Bind Mount가 정상적으로 연결되고 HTML 수정 내용이 웹 페이지에 반영되었습니다.

### 11.5 GitHub Push 인증 오류

**문제**

`git push` 실행 시 인증 오류와 `403 Permission denied` 오류가 발생했습니다.

**원인**

GitHub는 일반 계정 비밀번호를 이용한 HTTPS Git 인증을 지원하지 않습니다.

또한 처음 발급한 Personal Access Token(PAT)에 저장소 쓰기 권한이 없었습니다.

**해결 과정**

1. GitHub에서 Fine-grained Personal Access Token을 발급했습니다.
2. Repository access를 `Only select repositories`로 변경하고 `cody1`을 선택했습니다.
3. Repository permissions의 Contents 권한을 `Read and write`로 설정했습니다.
4. 수정한 토큰으로 다시 인증하여 `git push -u origin main`을 성공적으로 실행했습니다.

**결과**

GitHub 원격 저장소에 프로젝트 파일을 업로드했습니다.

이후 추가 수정 사항을 Push하는 과정에서 인증 오류가 다시 발생하여 인증 방식에 대한 추가 확인이 필요했습니다.

---

## 12. 실습 결과 및 증빙 자료

이번 프로젝트에서 진행한 주요 실습 결과를 README에 기록했습니다.

| 실습 항목 | 증빙 자료 |
|---|---|
| Linux 파일 생성·이동·삭제 | 4.2절 실행 결과 |
| Linux 파일 권한 변경 | 4.4절 `ls -l` 출력 |
| Docker 버전 및 데몬 확인 | 5.1~5.2절 출력 |
| Hello World 실행 | 5.3절 출력 |
| Docker 컨테이너 및 이미지 목록 | 5.5~5.6절 결과 |
| Nginx 웹 서버 구축 | 6.4절 스크린샷 |
| Docker Bind Mount | 7.3절 스크린샷 |
| Docker Volume | 8.4절 출력 및 스크린샷 |
| GitHub Push | 9.4절 성공 로그 |
| 오류 해결 과정 | 11절 Troubleshooting |

### 스크린샷 목록

**Nginx 웹 서버**

![Nginx 웹 서버 실행 화면](screenshots/nginx-webserver.png)

**Docker Bind Mount**

![Docker Bind Mount 실행 화면](screenshots/bindmount-result.png)

**Docker Volume**

![Docker Volume 데이터 유지 확인](screenshots/docker-volume-result.png)

---

## 13. 학습 내용 및 느낀 점

이번 실습을 통해 Linux 환경에서 파일과 디렉터리를 관리하고, 파일 권한을 변경하는 방법을 익혔습니다.

Docker를 이용하여 컨테이너를 실행하고, Dockerfile을 작성하여 Nginx 웹 서버 이미지를 직접 빌드했습니다.

또한 Docker 이미지와 컨테이너의 차이를 이해하고, 포트 매핑을 통해 컨테이너 내부에서 실행되는 웹 서버에 접근하는 방법을 학습했습니다.

Bind Mount를 이용해 호스트에서 수정한 HTML 파일을 컨테이너에 반영하고, Docker Volume을 이용해 컨테이너를 삭제한 후에도 데이터가 유지되는 것을 확인했습니다.

실습 과정에서는 Docker 명령어 실행 오류, 포트 충돌, Bind Mount 경로 오류, GitHub 인증 오류 등이 발생했습니다.

오류가 발생할 때마다 원인을 확인하고 설정이나 명령어를 수정하면서 문제를 해결하는 과정을 경험했습니다.

마지막으로 Git과 GitHub를 이용해 실습 내용을 기록하고 관리하면서, 개발 과정에서 버전 관리와 문서화가 중요하다는 점을 배웠습니다.
