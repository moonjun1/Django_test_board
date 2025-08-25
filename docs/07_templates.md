# 7. 템플릿: 사용자에게 보여지는 화면

템플릿은 사용자에게 보여지는 웹 페이지의 구조와 내용을 정의하는 파일입니다. Django는 HTML 파일 안에 파이썬 코드와 유사한 문법을 사용하여 동적인 내용을 삽입할 수 있는 Django Template Language (DTL)를 제공합니다. 우리 프로젝트에서는 `board/templates/board/` 폴더 안에 템플릿 파일들이 위치합니다.

## 템플릿 파일의 위치

Django는 `settings.py`의 `TEMPLATES` 설정(`APP_DIRS: True`) 덕분에 각 앱 폴더 안에 있는 `templates/` 폴더를 자동으로 찾아 템플릿 파일을 로드할 수 있습니다. 관례적으로 `templates/` 폴더 아래에 앱 이름과 동일한 폴더를 하나 더 만들고 그 안에 템플릿 파일을 넣습니다. (예: `board/templates/board/post_list.html`)

## 7.1. `post_list.html`: 게시물 목록 페이지

이 템플릿은 `views.py`의 `post_list` 뷰에서 전달받은 게시물 목록(`posts`)을 화면에 표시합니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>게시물 목록</title>
</head>
<body>
    <h1>게시물 목록</h1>
    <ul>
        {% for post in posts %}
            <li><a href="{% url 'board:post_detail' pk=post.pk %}">{{ post.title }}</a></li>
        {% empty %}
            <li>게시물이 없습니다.</li>
        {% endfor %}
    </ul>
    <a href="/admin/board/post/add/">새 게시물 추가 (관리자 페이지)</a>
</body>
</html>
```

### 코드 설명

*   `{% for post in posts %}` ... `{% endfor %}`:
    *   `posts`는 `post_list` 뷰에서 템플릿으로 전달해 준 게시물 목록입니다.
    *   `for` 루프를 사용하여 `posts` 리스트의 각 항목(`post`)을 반복하면서 HTML 목록(`<li>`)을 생성합니다.
*   `<li><a href="{% url 'board:post_detail' pk=post.pk %}">{{ post.title }}</a></li>`:
    *   `{{ post.title }}`: `post` 객체의 `title` 속성 값을 화면에 출력합니다. `{{ ... }}`는 변수 값을 출력할 때 사용합니다.
    *   `{% url 'board:post_detail' pk=post.pk %}`:
        *   `{% url ... %}` 태그는 Django의 URL 패턴에 정의된 이름을 사용하여 실제 URL을 동적으로 생성합니다.
        *   `'board:post_detail'`: `board` 앱의 `urls.py`에 정의된 `post_detail`이라는 이름의 URL 패턴을 참조합니다. (`app_name = 'board'`와 `name='post_detail'`을 기억하시나요?)
        *   `pk=post.pk`: `post_detail` URL 패턴이 요구하는 `pk` 인자에 현재 `post` 객체의 기본 키(`post.pk`) 값을 전달합니다. 이렇게 하면 각 게시물의 상세 페이지로 이동하는 정확한 URL이 생성됩니다.
*   `{% empty %}`:
    *   `for` 루프 안에 사용되며, `posts` 리스트가 비어 있을 때(`for` 루프가 한 번도 실행되지 않을 때) `<li>게시물이 없습니다.</li>`를 출력합니다.

## 7.2. `post_detail.html`: 게시물 상세 페이지

이 템플릿은 `views.py`의 `post_detail` 뷰에서 전달받은 특정 게시물(`post`)의 상세 내용을 화면에 표시합니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>{{ post.title }}</title>
</head>
<body>
    <h1>{{ post.title }}</h1>
    <p>{{ post.content }}</p>
    <a href="{% url 'board:post_list' %}">목록으로 돌아가기</a>
</body>
</html>
```

### 코드 설명

*   `{{ post.title }}`: `post_detail` 뷰에서 전달받은 `post` 객체의 `title` 속성 값을 출력합니다.
*   `{{ post.content }}`: `post` 객체의 `content` 속성 값을 출력합니다.
*   `<a href="{% url 'board:post_list' %}">목록으로 돌아가기</a>`:
    *   `{% url 'board:post_list' %}`를 사용하여 게시물 목록 페이지로 돌아가는 링크를 생성합니다.

## Django Template Language (DTL) 기본 문법

*   **변수**: `{{ variable }}`
    *   뷰에서 템플릿으로 전달된 파이썬 변수의 값을 출력합니다.
    *   예: `{{ post.title }}`, `{{ user.username }}`
*   **태그**: `{% tag %}`
    *   템플릿에서 특정 로직을 수행합니다. (예: 루프, 조건문, URL 생성 등)
    *   예: `{% for %}`, `{% if %}`, `{% url %}`, `{% csrf_token %}`
*   **주석**: `{# comment #}`
    *   템플릿에 주석을 추가합니다. HTML 주석(`<!-- -->`)과 달리, 이 주석은 최종 HTML 출력에 포함되지 않습니다.

템플릿은 사용자에게 보여지는 웹 페이지의 '얼굴'입니다. 뷰에서 처리된 데이터를 아름답고 의미 있게 표현하는 방법을 배우는 것이 중요합니다.
