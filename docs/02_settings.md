# 2. `settings.py`: 프로젝트의 심장

`settings.py` 파일은 Django 프로젝트의 모든 설정과 구성을 담고 있는 핵심 파일입니다. 데이터베이스 연결, 설치된 앱 목록, 시간대, 언어 등 프로젝트의 전반적인 동작 방식을 여기서 정의합니다. 이 파일은 프로젝트의 '심장'과 같아서, Django가 어떻게 작동해야 하는지 지시하는 역할을 합니다.

## `settings.py` 파일 살펴보기

우리 프로젝트의 `myboard/myboard/settings.py` 파일을 열어보면 다음과 같은 주요 섹션들을 볼 수 있습니다.

```python
# myboard/myboard/settings.py

# ... (상단 주석 및 Path 임포트)

BASE_DIR = Path(__file__).resolve().parent.parent

# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-^mwg#t!4x*(-hj0jjrj4o9o!3j7^ag1tfl6tffjffj!w^g(bj8'

# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True

ALLOWED_HOSTS = []


# Application definition

INSTALLED_APPS = [
    'board', # 우리가 만든 게시판 앱
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

MIDDLEWARE = [
    # ... (미들웨어 목록)
]

ROOT_URLCONF = 'myboard.urls'

TEMPLATES = [
    # ... (템플릿 설정)
]

WSGI_APPLICATION = 'myboard.wsgi.application'


# Database
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}


# Password validation
AUTH_PASSWORD_VALIDATORS = [
    # ... (비밀번호 유효성 검사기)
]


# Internationalization
LANGUAGE_CODE = 'ko-kr' # 한국어로 변경

TIME_ZONE = 'Asia/Seoul' # 서울 시간대로 변경

USE_I18N = True

USE_TZ = True


# Static files (CSS, JavaScript, Images)
STATIC_URL = 'static/'

# Default primary key field type
DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
```

## 주요 설정 항목 설명

### `BASE_DIR`

*   **역할**: 프로젝트의 루트 디렉토리(가장 상위 폴더)의 경로를 정의합니다. Django가 파일들을 찾을 때 이 경로를 기준으로 합니다.
*   **초보자를 위한 팁**: 이 값은 자동으로 설정되므로 직접 수정할 일은 거의 없습니다. 다른 파일 경로를 지정할 때 `BASE_DIR / '폴더명' / '파일명'` 형태로 사용됩니다.

### `SECRET_KEY`

*   **역할**: Django 프로젝트의 보안을 위한 비밀 키입니다. 세션 관리, 암호화 서명 등 다양한 보안 관련 작업에 사용됩니다.
*   **초보자를 위한 팁**: 이 키는 **절대 외부에 노출되어서는 안 됩니다.** 특히 GitHub 같은 공개 저장소에 올릴 때는 주의해야 합니다. 실제 서비스에서는 환경 변수 등으로 관리하는 것이 일반적입니다.

### `DEBUG`

*   **역할**: 개발 모드인지 운영 모드인지를 설정합니다.
    *   `True`: 개발 모드입니다. 오류 발생 시 자세한 디버그 정보가 웹 페이지에 표시됩니다. 개발 중에는 `True`로 설정합니다.
    *   `False`: 운영 모드입니다. 오류 발생 시 사용자에게 자세한 정보를 보여주지 않고, 일반적인 오류 페이지를 표시합니다. 보안을 위해 실제 서비스에서는 반드시 `False`로 설정해야 합니다.
*   **초보자를 위한 팁**: 개발 중에는 `True`로 두어 오류를 쉽게 파악하고, 배포 전에는 `False`로 바꾸는 것을 잊지 마세요!

### `ALLOWED_HOSTS`

*   **역할**: Django 애플리케이션이 응답할 수 있는 호스트/도메인 이름을 정의합니다. `DEBUG = False`일 때 반드시 설정해야 합니다.
*   **초보자를 위한 팁**: 개발 중에는 비워두거나 `[]`로 두어도 되지만, 실제 서비스에서는 웹사이트의 도메인 이름(예: `['www.myboard.com', 'myboard.com']`)을 여기에 추가해야 합니다.

### `INSTALLED_APPS`

*   **역할**: 이 Django 프로젝트에 설치된 모든 애플리케이션(앱) 목록입니다. Django가 제공하는 기본 앱들과 우리가 만든 커스텀 앱(`board`)이 여기에 포함됩니다.
*   **초보자를 위한 팁**: 새로운 앱을 만들면 반드시 여기에 추가해야 Django가 해당 앱을 인식하고 작동시킬 수 있습니다. 우리가 만든 `board` 앱을 여기에 추가했던 것을 기억하시나요?

### `MIDDLEWARE`

*   **역할**: 요청(Request)과 응답(Response) 사이에서 작동하는 일련의 프레임워크입니다. 보안, 세션, CSRF 보호 등 다양한 기능을 수행합니다.
*   **초보자를 위한 팁**: 미들웨어는 웹 요청이 뷰에 도달하기 전, 그리고 뷰에서 응답이 사용자에게 전달되기 전에 특정 작업을 수행합니다. 지금은 각 미들웨어의 정확한 역할을 모두 알 필요는 없지만, 이런 것이 있다는 정도만 알아두세요.

### `ROOT_URLCONF`

*   **역할**: 프로젝트의 메인 URL 설정 파일의 경로를 지정합니다. 우리 프로젝트에서는 `myboard.urls`로 설정되어 있습니다.
*   **초보자를 위한 팁**: Django가 웹 요청을 받았을 때, 어떤 URL 패턴을 먼저 찾아봐야 할지 알려주는 '시작점'이라고 생각하시면 됩니다.

### `TEMPLATES`

*   **역할**: Django가 HTML 템플릿 파일을 찾고 렌더링하는 방법을 설정합니다.
*   **초보자를 위한 팁**: `APP_DIRS: True` 설정 덕분에 Django는 각 앱 폴더 안에 있는 `templates/` 폴더를 자동으로 찾아 템플릿 파일을 로드할 수 있습니다.

### `DATABASES`

*   **역할**: 프로젝트가 사용할 데이터베이스 연결 정보를 정의합니다. 기본적으로 SQLite가 설정되어 있습니다.
*   **초보자를 위한 팁**: `ENGINE`은 사용할 데이터베이스 종류(SQLite, PostgreSQL, MySQL 등)를, `NAME`은 데이터베이스 파일의 경로를 나타냅니다. 나중에 다른 데이터베이스를 사용하고 싶다면 이 부분을 수정해야 합니다.

### `LANGUAGE_CODE` 및 `TIME_ZONE`

*   **역할**: 프로젝트의 기본 언어와 시간대를 설정합니다. 우리가 관리자 페이지를 한국어로 바꾸기 위해 `ko-kr`과 `Asia/Seoul`로 변경했던 부분입니다.
*   **초보자를 위한 팁**: 이 설정을 통해 Django가 날짜, 시간, 메시지 등을 해당 언어와 시간대에 맞게 표시할 수 있습니다.

### `STATIC_URL`

*   **역할**: 정적 파일(CSS, JavaScript, 이미지 등)에 접근할 때 사용되는 URL 접두사를 정의합니다.
*   **초보자를 위한 팁**: 웹 페이지의 디자인이나 동작을 위한 파일들을 Django가 어떻게 제공할지 설정하는 부분입니다.

`settings.py`는 Django 프로젝트의 모든 것을 제어하는 강력한 파일입니다. 처음에는 모든 설정을 이해하기 어렵겠지만, 프로젝트를 진행하면서 필요한 부분을 찾아보고 수정하는 연습을 통해 익숙해질 수 있습니다.
