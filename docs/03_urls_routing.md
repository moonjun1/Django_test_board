# 3. URL 라우팅: 웹 요청의 길잡이

웹 애플리케이션에서 URL 라우팅은 사용자가 특정 웹 주소(URL)로 접속했을 때, 어떤 코드를 실행하여 어떤 내용을 보여줄지 결정하는 과정입니다. Django에서는 `urls.py` 파일들을 통해 이 라우팅을 관리합니다. 우리 프로젝트에는 두 개의 `urls.py` 파일이 있습니다: 프로젝트의 메인 `urls.py`와 앱의 `urls.py`.

## 3.1. 프로젝트의 메인 `urls.py` (`myboard/myboard/urls.py`)

이 파일은 프로젝트 전체의 URL 요청을 가장 먼저 받아서 처리하는 곳입니다. 마치 큰 건물의 '중앙 안내 데스크'와 같습니다. 사용자가 어떤 주소로 왔을 때, 그 요청을 어떤 앱으로 보낼지 여기서 결정합니다.

```python
# myboard/myboard/urls.py

from django.contrib import admin
from django.urls import path, include # include 함수를 임포트합니다.

urlpatterns = [
    path('admin/', admin.site.urls), # 'admin/' 경로로 오면 Django 관리자 페이지로 연결
    path('board/', include('board.urls')), # 'board/' 경로로 오면 board 앱의 urls.py로 연결
]
```

### 코드 설명

*   `from django.urls import path, include`: `path` 함수는 URL 패턴을 정의하는 데 사용되고, `include` 함수는 다른 `urls.py` 파일을 포함시킬 때 사용됩니다.
*   `urlpatterns = [...]`: Django는 이 리스트에 정의된 순서대로 URL 패턴을 검사합니다.
*   `path('admin/', admin.site.urls)`:
    *   사용자가 `http://127.0.0.1:8000/admin/`과 같이 `/admin/`으로 시작하는 URL로 접속하면, Django의 기본 관리자 페이지(`admin.site.urls`)로 연결하라는 의미입니다.
*   `path('board/', include('board.urls'))`:
    *   사용자가 `http://127.0.0.1:8000/board/`와 같이 `/board/`로 시작하는 URL로 접속하면, 더 이상 이 파일에서 처리하지 않고 `board` 앱 안에 있는 `urls.py` 파일(`board.urls`)로 요청을 넘기라는 의미입니다.
    *   `include()` 함수는 이렇게 URL 라우팅을 여러 파일로 분리하여 관리할 수 있게 해줍니다. 이는 프로젝트가 커질수록 URL 관리를 훨씬 효율적으로 만들어 줍니다.

## 3.2. 앱의 `urls.py` (`board/urls.py`)

이 파일은 특정 앱(`board` 앱) 내에서 사용될 URL 패턴들을 정의합니다. 프로젝트의 메인 `urls.py`에서 요청을 넘겨받은 후, 이 파일에서 해당 앱의 세부적인 URL 라우팅을 처리합니다. 마치 '중앙 안내 데스크'에서 특정 부서(앱)로 요청을 넘기면, 그 부서의 '세부 안내 데스크'에서 요청을 처리하는 것과 같습니다.

```python
# board/urls.py

from django.urls import path
from . import views # 현재 앱의 views.py 파일을 임포트합니다.

app_name = 'board' # 이 앱의 이름을 정의합니다. (템플릿에서 URL을 참조할 때 사용)

urlpatterns = [
    path('', views.post_list, name='post_list'), # 'board/' 뒤에 아무것도 없으면 post_list 뷰로 연결
    path('<int:pk>/', views.post_detail, name='post_detail'), # 'board/숫자/' 형태면 post_detail 뷰로 연결
]
```

### 코드 설명

*   `from . import views`: 현재 `board` 앱 폴더 안에 있는 `views.py` 파일을 임포트합니다. `.`은 현재 디렉토리를 의미합니다.
*   `app_name = 'board'`: 이 앱의 이름을 `board`로 정의합니다. 이 이름은 나중에 템플릿에서 URL을 참조할 때 `{% url 'app_name:url_name' %}` 형식으로 사용되어, URL이 변경되더라도 코드를 수정할 필요 없이 유연하게 관리할 수 있게 해줍니다.
*   `path('', views.post_list, name='post_list')`:
    *   `''`: `myboard/myboard/urls.py`에서 `/board/`로 요청을 넘겨받았으므로, 이 `urls.py` 파일에서는 `/board/` 뒤에 아무것도 붙지 않는 경우(`http://127.0.0.1:8000/board/`)를 의미합니다.
    *   `views.post_list`: 이 URL로 요청이 오면 `views.py` 파일 안에 있는 `post_list` 함수(뷰)를 실행하라는 의미입니다.
    *   `name='post_list'`: 이 URL 패턴에 `post_list`라는 이름을 부여합니다. 이 이름은 템플릿이나 다른 파이썬 코드에서 이 URL을 쉽게 참조할 수 있도록 해줍니다. (예: `{% url 'board:post_list' %}`)
*   `path('<int:pk>/', views.post_detail, name='post_detail')`:
    *   `<int:pk>`: URL 경로에서 정수(integer) 값을 `pk`라는 변수로 추출하라는 의미입니다. 예를 들어, `http://127.0.0.1:8000/board/1/`로 접속하면 `pk` 값은 `1`이 됩니다.
    *   `views.post_detail`: 이 URL로 요청이 오면 `views.py` 파일 안에 있는 `post_detail` 함수(뷰)를 실행하라는 의미입니다. 이때 추출된 `pk` 값은 `post_detail` 함수의 인자로 전달됩니다.
    *   `name='post_detail'`: 이 URL 패턴에 `post_detail`이라는 이름을 부여합니다. (예: `{% url 'board:post_detail' pk=post.pk %}`)

## URL 라우팅의 흐름 요약

1.  사용자가 웹 브라우저에 `http://127.0.0.1:8000/board/1/`와 같은 URL을 입력합니다.
2.  Django는 먼저 프로젝트의 메인 `urls.py`(`myboard/myboard/urls.py`)를 확인합니다.
3.  `path('board/', include('board.urls'))` 패턴과 일치하므로, `/board/` 이후의 부분(`1/`)을 `board` 앱의 `urls.py`(`board/urls.py`)로 넘깁니다.
4.  `board/urls.py`에서 `path('<int:pk>/', views.post_detail, name='post_detail')` 패턴과 `1/`이 일치하는 것을 찾습니다.
5.  `post_detail` 뷰 함수가 실행되고, `pk=1`이라는 인자가 전달됩니다.

이러한 URL 라우팅 시스템을 통해 Django는 사용자의 요청을 적절한 뷰 함수로 연결하여 처리할 수 있게 됩니다.
