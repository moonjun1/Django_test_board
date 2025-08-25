# Simple Django Board

## 프로젝트 설명
<img width="1256" height="848" alt="스크린샷 2025-08-25 161732" src="https://github.com/user-attachments/assets/38169931-2098-408e-b153-95782989749a" />
<img width="1257" height="771" alt="스크린샷 2025-08-25 161723" src="https://github.com/user-attachments/assets/320e1da5-a247-4958-be92-bc80f69a63a4" />

이 프로젝트는 Django 프레임워크를 사용하여 개발된 간단한 게시판 웹 애플리케이션입니다. 기본적인 게시물 생성, 목록 보기, 상세 보기 기능을 제공합니다.

## 주요 기능

-   게시물 생성, 조회, 목록 보기
-   Django 관리자 페이지를 통한 게시물 관리

## 설정 및 실행 방법

### 1. 필수 요구사항

-   Python 3.x
-   pip (Python 패키지 관리자)

### 2. 프로젝트 클론

```bash
git clone https://github.com/moonjun1/Django_test_board.git
cd Django_test_board
```

### 3. 가상 환경 설정 (권장)

```bash
python -m venv venv
# Windows
.\venv\Scripts\activate
# macOS/Linux
source venv/bin/activate
```

### 4. 의존성 설치

```bash
pip install -r requirements.txt
```

### 5. 데이터베이스 마이그레이션

```bash
python manage.py makemigrations board
python manage.py migrate
```

### 6. 슈퍼유저 생성 (관리자 페이지 접속용)

관리자 페이지에 접속하여 게시물을 관리하기 위해 슈퍼유저를 생성합니다. 명령어를 실행한 후 터미널의 지시에 따라 사용자 이름, 이메일, 비밀번호를 입력하세요.

```bash
python manage.py createsuperuser
```

### 7. 애플리케이션 실행

개발 서버를 시작합니다.

```bash
python manage.py runserver
```

서버가 실행되면 웹 브라우저에서 다음 주소로 접속할 수 있습니다:

-   **게시판 목록:** `http://127.0.0.1:8000/board/`
-   **관리자 페이지:** `http://127.0.0.1:8000/admin/` (생성한 슈퍼유저 계정으로 로그인)
