
# 내 컴퓨터에 개발자용 작업실 꾸미기

## 1. 프로젝트 소개

이 프로젝트는 Windows 환경에서 개발자용 작업실을 구성하고,
Linux 명령어와 Docker의 기본 기능을 실습하기 위해 진행했습니다.

실습을 통해 개발 환경을 설정하고, 컨테이너를 실행하며,
웹 서버 구축과 데이터 저장 방법을 익혔습니다.

## 2. 개발 환경

| 항목 | 사용 환경 |
|---|---|
| 운영체제 | Windows |
| Linux 환경 | WSL2 Ubuntu |
| 컨테이너 도구 | Docker Desktop |
| 개발 도구 | Visual Studio Code |
| 버전 관리 | Git |

## 3. Linux 명령어 실습

WSL2 Ubuntu 환경에서 기본적인 Linux 명령어와 파일 및 디렉터리 관리 방법을 실습했습니다.

### 3-1. 기본 명령어

| 명령어 | 기능 |
|---|---|
| `pwd` | 현재 작업 디렉터리 확인 |
| `ls -la` | 파일 및 디렉터리 목록 확인 |
| `mkdir` | 디렉터리 생성 |
| `touch` | 빈 파일 생성 |
| `echo` | 문자열 출력 및 파일에 기록 |
| `cat` | 파일 내용 확인 |
| `cp` | 파일 복사 |
| `mv` | 파일 이동 및 이름 변경 |
| `rm` | 파일 삭제 |

### 3-2. 파일 권한 실습

`chmod` 명령어를 사용하여 파일 권한을 변경했습니다.

- `chmod 600`: 소유자에게만 읽기 및 쓰기 권한 부여
- `chmod 644`: 소유자에게 읽기 및 쓰기, 그 외 사용자에게 읽기 권한 부여

### 3-3. 실습 결과

- 디렉터리 생성 및 이동
- 파일 생성, 복사, 이름 변경 및 삭제
- 파일 내용 입력 및 확인
- 파일 권한 변경

## 4. Docker 기본 실습

### 4-1. Docker 설치 및 버전 확인

Docker Desktop을 설치하고 Docker 엔진이 정상적으로 실행되는지 확인했습니다.

```powershell
docker --version
docker version
```

### 4-2. Hello World 컨테이너 실행

```powershell
docker run hello-world
```

Docker Hub에서 이미지를 내려받아 컨테이너를 실행하고,
`Hello from Docker!` 메시지가 출력되는 것을 확인했습니다.

### 4-3. Ubuntu 컨테이너 실행

```powershell
docker run -it --name ubuntu-practice ubuntu bash
```

컨테이너 내부에서 다음 작업을 수행했습니다.

- Ubuntu 운영체제 정보 확인
- 현재 디렉터리 확인
- 파일 및 디렉터리 목록 확인
- `docker-test.txt` 파일 생성
- 파일에 `Hello, Docker!` 입력 및 확인

컨테이너를 종료한 후 다시 시작하여 파일이 유지되는 것도 확인했습니다.

```powershell
docker start -ai ubuntu-practice
```

## 5. Docker를 이용한 웹 서버 구축

### 5-1. 웹 페이지 작성

`site/index.html` 파일을 생성하고 HTML과 CSS를 사용하여 웹 페이지를 작성했습니다.

웹 페이지에는 다음 내용을 포함했습니다.

- Docker 웹 서버 안내 문구
- HTML 기본 구조
- CSS를 활용한 배경 및 카드 디자인

### 5-2. Dockerfile 작성

Nginx 기반의 웹 서버 이미지를 만들기 위해 Dockerfile을 작성했습니다.

```dockerfile
FROM nginx:alpine

COPY site/index.html /usr/share/nginx/html/index.html

EXPOSE 80
```

### 5-3. Docker 이미지 빌드

작성한 Dockerfile을 이용하여 웹 서버 이미지를 생성했습니다.

```powershell
docker build -t my-web-server:1.0 .
```

### 5-4. 컨테이너 실행 및 포트 연결

컨테이너의 80번 포트를 컴퓨터의 8082번 포트와 연결했습니다.

```powershell
docker run -d --name my-web-server-container -p 8082:80 my-web-server:1.0
```

웹 브라우저에서 다음 주소로 접속하여 웹 페이지가 정상적으로 표시되는지 확인했습니다.

`http://localhost:8082`

### 5-5. 실습 결과

- Dockerfile을 이용한 이미지 생성 성공
- Nginx 웹 서버 컨테이너 실행 성공
- 포트 매핑을 통한 로컬 웹 페이지 접속 성공


## 6. 데이터 연결 및 저장 실습

### 6-1. 바인드 마운트(Bind Mount)

내 컴퓨터의 `site` 폴더를 Docker 컨테이너의 웹 서버 디렉터리와 연결했습니다.

```powershell
docker run -d --name my-web-bind-container -p 8083:80 -v "${PWD}\site:/usr/share/nginx/html:ro" nginx:alpine
```

웹 브라우저에서 다음 주소로 접속하여 페이지가 표시되는 것을 확인했습니다.

`http://localhost:8083`

이후 내 컴퓨터에서 `site/index.html` 파일의 내용을 수정하고 브라우저를 새로고침했습니다.

Docker 이미지를 다시 빌드하거나 컨테이너를 재시작하지 않아도 수정된 내용이 반영되는 것을 확인했습니다.

### 6-2. Docker 볼륨(Volume)

Docker 볼륨을 생성하여 컨테이너와 독립적으로 데이터를 보관하는 방법을 실습했습니다.

```powershell
docker volume create my-web-data
```

볼륨에 테스트 파일을 생성하고 내용을 확인했습니다.

```powershell
docker run --rm -v my-web-data:/data ubuntu bash -c "echo 'Docker Volume 데이터 보존 테스트 성공!' > /data/test.txt && cat /data/test.txt"
```

별도의 컨테이너를 실행하여 볼륨에 저장된 파일을 확인했습니다.

```powershell
docker run -d --name volume-test-container -v my-web-data:/data ubuntu sleep infinity
```

이후 테스트 컨테이너를 삭제했습니다.

```powershell
docker rm -f volume-test-container
```

새로운 임시 컨테이너에서 동일한 볼륨을 연결하여 데이터를 다시 확인했습니다.

```powershell
docker run --rm -v my-web-data:/data ubuntu cat /data/test.txt
```

### 6-3. 실습 결과

- 바인드 마운트로 호스트 컴퓨터와 컨테이너의 디렉터리 연결
- 호스트 파일 수정 시 컨테이너 웹 페이지에 변경 사항 반영 확인
- Docker 볼륨 생성 및 데이터 저장
- 컨테이너 삭제 후에도 볼륨 데이터가 유지되는 것 확인

### 6-4. 바인드 마운트와 볼륨의 차이

| 구분 | 바인드 마운트 | Docker 볼륨 |
|---|---|---|
| 저장 위치 | 호스트의 지정 폴더 | Docker가 관리하는 영역 |
| 주요 목적 | 호스트와 파일 공유 | 데이터 영구 보관 |
| 실습 사례 | `site` 폴더 연결 | `my-web-data` 볼륨 |


## 7. Git 버전 관리 실습

### 7-1. Git 저장소 생성

프로젝트 폴더에서 Git 저장소를 초기화했습니다.

```powershell
git init
```

### 7-2. Git 사용자 정보 설정

현재 프로젝트에만 적용되도록 사용자 이름과 이메일을 설정했습니다.

```powershell
git config user.name "Kimjeongnim"
git config user.email "kululu81@gmail.com"
```

설정 결과를 확인했습니다.

```powershell
git config --local --list
```

### 7-3. Git 상태 확인

```powershell
git status
```

Git을 통해 프로젝트 파일의 변경 상태를 확인할 수 있음을 확인했습니다.

### 7-4. 실습 결과

- 프로젝트 폴더에 Git 저장소 생성
- 프로젝트별 사용자 정보 설정
- Git 저장소의 파일 변경 상태 확인


## 8. 문제 해결 기록

### 8-1. Git 저장소 초기화 오류

**문제**

Git 명령어 실행 시 다음 오류가 발생했습니다.

`fatal: not a git repository`

**원인**

현재 작업 폴더가 Git 저장소로 초기화되지 않은 상태였습니다.

**해결 방법**

프로젝트 폴더에서 다음 명령어를 실행했습니다.

```powershell
git init
```

Git 저장소가 정상적으로 생성된 후 다시 명령어를 실행하여 해결했습니다.

### 8-2. Docker 컨테이너 실행 명령어 오류

**문제**

컨테이너 재시작 명령어를 입력하는 과정에서 옵션 사이에 공백을 잘못 넣어 오류가 발생했습니다.

**원인**

`docker start -ai` 명령어를 `docker start - ai`로 잘못 입력했습니다.

**해결 방법**

옵션을 붙여서 정확하게 입력했습니다.

```powershell
docker start -ai ubuntu-practice
```

정상적으로 컨테이너에 다시 접속할 수 있었습니다.

### 8-3. README 문서 작성 중 서식 오류

**문제**

PowerShell에서 긴 Markdown 문서를 작성하는 과정에서 코드 블록과 문서 서식이 깨지는 문제가 발생했습니다.

**원인**

여러 단계의 Markdown 코드 블록을 PowerShell 명령어에 함께 입력하면서 서식이 올바르게 처리되지 않았습니다.

**해결 방법**

Visual Studio Code에서 README.md 파일을 직접 편집하고 Markdown 미리보기로 결과를 확인했습니다.

## 9. 최종 실습 결과

이번 프로젝트를 통해 다음 내용을 실습했습니다.

- WSL2 Ubuntu 환경에서 Linux 기본 명령어 사용
- 파일 및 디렉터리 관리와 권한 변경
- Docker 이미지 다운로드 및 컨테이너 실행
- Dockerfile 작성과 Nginx 웹 서버 구축
- 포트 매핑을 통한 로컬 웹 서버 접속
- 바인드 마운트를 이용한 파일 변경 사항 반영
- Docker 볼륨을 이용한 데이터 영구 저장
- Git 저장소 생성 및 프로젝트별 사용자 정보 설정
- README.md 기술 문서 작성
