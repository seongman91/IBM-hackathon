# Windows·macOS 개발환경 구성

## 1. 실습 목표

본 실습에서는 이후 교육과정에서 공통으로 사용할 Python, VS Code, Docker Desktop 및 watsonx.ai API 실행환경을 구성한다.

## 2. 사전 준비

### 공통 준비사항

- 인터넷 연결
- Visual Studio Code
- Python 3.11
- Docker Desktop
- IBM watsonx.ai 프로젝트 접근 권한
- 교육 담당자가 제공한 API Key, URL, Project ID, Model ID

### 운영체제별 참고사항

| 구분 | Windows | macOS |
|---|---|---|
| 터미널 | PowerShell | Terminal(zsh) |
| 가상환경 생성 전 Python | `python` 또는 `py` | `python3` |
| Docker 기반 기술 | WSL 2 | macOS Virtualization Framework |
| 설치파일 | Windows용 | Intel 또는 Apple Silicon용 |

Docker Desktop은 관리자 권한과 시스템 재부팅이 필요할 수 있으므로 교육 시작 전에 설치를 완료하는 것을 권장한다.

## 3. Starter Project 준비

배포받은 `starter` 폴더를 개인 작업 폴더로 복사한 후 폴더명을 `watsonx-hackathon`으로 변경한다.

```text
watsonx-hackathon/
├─ .env.example
├─ .gitignore
├─ requirements.txt
├─ check_environment.py
├─ check_watsonx.py
├─ src/
└─ tests/
```

## 4. Windows 환경설정

### 4.1 Python 설치 확인

PowerShell에서 다음 명령을 실행한다.

```powershell
python --version
python -m pip --version
```

`python` 명령을 인식하지 못하는 경우 다음 명령을 확인한다.

```powershell
py --version
```

### 4.2 프로젝트 폴더 이동

실제 프로젝트 경로에 맞게 이동한다.

```powershell
cd C:\work\watsonx-hackathon
```

### 4.3 가상환경 생성 및 활성화

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
```

`python` 대신 `py`를 사용하는 환경에서는 다음 명령을 사용할 수 있다.

```powershell
py -3.11 -m venv .venv
.venv\Scripts\Activate.ps1
```

가상환경이 정상적으로 활성화되면 Terminal 앞부분에 `(.venv)`가 표시된다.

```text
(.venv) PS C:\work\watsonx-hackathon>
```

### 4.4 PowerShell 실행 정책 오류

다음과 유사한 오류가 발생할 수 있다.

```text
running scripts is disabled on this system
```

개인 PC에서는 다음 명령으로 현재 사용자 범위의 실행 정책을 설정할 수 있다.

```powershell
Set-ExecutionPolicy -Scope CurrentUser RemoteSigned
```

학교 또는 조직에서 관리하는 PC는 정책을 임의로 변경하지 않고 관리자에게 문의한다.

### 4.5 VS Code 실행

```powershell
code .
```

`code` 명령을 사용할 수 없는 경우 VS Code를 실행하고 `File > Open Folder`에서 프로젝트 폴더를 선택한다.

### 4.6 Docker Desktop과 WSL 2 확인

```powershell
wsl --version
docker --version
docker compose version
```

Docker Desktop을 실행한 상태에서 다음 명령을 실행한다.

```powershell
docker run --rm hello-world
```

다음 문구가 표시되면 Docker 실행환경이 준비된 상태이다.

```text
Hello from Docker!
```

## 5. macOS 환경설정

### 5.1 프로세서 유형 확인

화면 왼쪽 상단의 Apple 메뉴에서 `이 Mac에 관하여`를 선택한다.

- Intel 프로세서: Intel용 Docker Desktop 사용
- Apple M 계열: Apple Silicon용 Docker Desktop 사용

Terminal에서는 다음 명령으로 확인할 수 있다.

```bash
uname -m
```

주요 결과:

```text
x86_64  # Intel
arm64   # Apple Silicon
```

### 5.2 Python 설치 확인

```bash
python3 --version
python3 -m pip --version
```

Python 3.11이 설치되지 않은 경우 Python 공식 설치파일 또는 Homebrew를 이용하여 설치한다.

Homebrew 사용 예시:

```bash
brew install python@3.11
```

### 5.3 프로젝트 폴더 이동

```bash
cd ~/work/watsonx-hackathon
```

### 5.4 가상환경 생성 및 활성화

```bash
python3 -m venv .venv
source .venv/bin/activate
```

가상환경이 정상적으로 활성화되면 Terminal 앞부분에 `(.venv)`가 표시된다.

```text
(.venv) user@mac watsonx-hackathon %
```

### 5.5 VS Code 실행

```bash
code .
```

`code` 명령을 사용할 수 없는 경우 VS Code를 실행하고 `File > Open Folder`에서 프로젝트 폴더를 선택한다.

### 5.6 Docker Desktop 확인

```bash
docker --version
docker compose version
docker run --rm hello-world
```

다음 문구가 표시되면 Docker 실행환경이 준비된 상태이다.

```text
Hello from Docker!
```

## 6. VS Code 공통 설정

### 6.1 확장 프로그램 설치

다음 확장 프로그램을 설치한다.

- Python
- Pylance
- Docker

### 6.2 Python Interpreter 선택

1. `Ctrl + Shift + P` 또는 `Cmd + Shift + P`를 실행한다.
2. `Python: Select Interpreter`를 선택한다.
3. 프로젝트의 `.venv` Interpreter를 선택한다.
4. VS Code Terminal을 새로 실행한다.

Interpreter 경로 예시:

```text
Windows: .venv\Scripts\python.exe
macOS:   .venv/bin/python
```

## 7. Python 라이브러리 설치

가상환경이 활성화된 상태에서는 Windows와 macOS 모두 다음 명령으로 통일한다.

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

설치 상태를 확인한다.

```bash
python -c "import ibm_watsonx_ai; print('watsonx SDK OK')"
```

## 8. watsonx.ai 인증정보 설정

### 8.1 `.env` 파일 생성

Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

macOS Terminal:

```bash
cp .env.example .env
```

### 8.2 인증정보 입력

`.env` 파일에 교육 담당자가 제공한 값을 입력한다.

```dotenv
WATSONX_APIKEY=YOUR_API_KEY
WATSONX_URL=YOUR_WATSONX_URL
WATSONX_PROJECT_ID=YOUR_PROJECT_ID
WATSONX_MODEL_ID=YOUR_MODEL_ID
```

실제 인증정보를 `.env.example`에 입력하지 않는다.

## 9. 환경 점검

```bash
python check_environment.py
```

예상 결과:

```text
=== Python ===
PASS Version: 3.11.x
PASS Virtual environment: .../.venv/...

=== Packages ===
PASS dotenv
PASS ibm_watsonx_ai

=== Environment variables ===
PASS WATSONX_APIKEY
PASS WATSONX_URL
PASS WATSONX_PROJECT_ID
PASS WATSONX_MODEL_ID

Environment check: PASS
```

인증정보의 실제 값은 출력되지 않는다.

## 10. watsonx.ai 모델 연결 확인

```bash
python check_watsonx.py
```

정상적으로 연결되면 watsonx.ai가 생성한 응답 문자열이 출력된다.

## 11. Git 보안 설정 확인

`.gitignore`에 `.env`가 포함되어 있는지 확인한다.

```gitignore
.venv/
.env
__pycache__/
*.pyc
.vscode/
.DS_Store
```

Git 저장소인 경우 다음 명령으로 `.env`가 추적되지 않는지 확인한다.

```bash
git status
```

## 12. 최종 점검 순서

```text
Python 실행
→ 가상환경 활성화
→ VS Code Interpreter 선택
→ 라이브러리 Import
→ Docker 실행
→ 환경변수 확인
→ watsonx.ai 모델 호출
```

## 13. 참고 문서

- [Python Windows 사용 안내](https://docs.python.org/3/using/windows.html)
- [Python macOS 사용 안내](https://docs.python.org/3/using/mac.html)
- [VS Code Python 환경](https://code.visualstudio.com/docs/python/environments)
- [Docker Desktop Windows 설치](https://docs.docker.com/desktop/setup/install/windows-install/)
- [Docker Desktop macOS 설치](https://docs.docker.com/desktop/setup/install/mac-install/)
- [IBM watsonx.ai Python 모델 호출](https://www.ibm.com/docs/en/watsonx/saas?topic=generation-inferencing-foundation-model-python)

