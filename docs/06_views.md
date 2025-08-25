# 6. `views.py`: 웹 요청 처리 로직

`views.py` 파일은 Django 애플리케이션에서 웹 요청을 받아 처리하고, 그에 대한 응답을 생성하는 핵심적인 역할을 합니다. 사용자가 웹 브라우저에서 특정 URL로 접속하면, Django는 해당 URL에 연결된 뷰 함수를 실행하여 필요한 데이터를 처리하고, 최종적으로 사용자에게 보여줄 웹 페이지(HTML)를 생성하여 반환합니다.

## `board/views.py` 파일 살펴보기

우리 프로젝트의 `board/views.py` 파일에는 두 개의 뷰 함수가 정의되어 있습니다: `post_list`와 `post_detail`.

```python
# board/views.py

from django.shortcuts import render, get_object_or_404
from .models import Post # 같은 앱 안에 있는 models.py에서 Post 모델을 임포트합니다.

# 게시물 목록을 보여주는 뷰
def post_list(request):
    # 데이터베이스에서 모든 Post 객체를 가져와서 id의 역순(최신순)으로 정렬합니다.
    posts = Post.objects.all().order_by('-id')
    # 'board/post_list.html' 템플릿을 렌더링하고, 'posts' 변수에 게시물 목록을 담아 전달합니다.
    return render(request, 'board/post_list.html', {'posts': posts})

# 특정 게시물의 상세 내용을 보여주는 뷰
def post_detail(request, pk):
    # 데이터베이스에서 pk(기본 키)에 해당하는 Post 객체를 가져옵니다.
    # 만약 해당 Post가 없으면 404(Not Found) 오류 페이지를 반환합니다.
    post = get_object_or_404(Post, pk=pk)
    # 'board/post_detail.html' 템플릿을 렌더링하고, 'post' 변수에 해당 게시물을 담아 전달합니다.
    return render(request, 'board/post_detail.html', {'post': post})
```

### 코드 설명

*   `from django.shortcuts import render, get_object_or_404`:
    *   `render`: 템플릿 파일을 HTML로 변환하여 웹 응답으로 반환하는 함수입니다. 뷰에서 가장 흔하게 사용됩니다.
    *   `get_object_or_404`: 특정 객체를 데이터베이스에서 가져오거나, 객체가 없으면 `Http404` 예외(404 Not Found 오류)를 발생시키는 편리한 함수입니다.
*   `from .models import Post`: 현재 앱의 `models.py` 파일에서 `Post` 모델을 임포트합니다. 이 모델을 사용하여 데이터베이스에서 게시물 데이터를 가져올 것입니다.

### `post_list(request)` 뷰 함수

*   **역할**: 모든 게시물의 목록을 보여주는 페이지를 처리합니다.
*   `def post_list(request):`: 모든 뷰 함수는 첫 번째 인자로 `request` 객체를 받습니다. 이 `request` 객체 안에는 사용자의 요청에 대한 모든 정보(예: 요청 방식, 사용자 정보, 전달된 데이터 등)가 담겨 있습니다.
*   `posts = Post.objects.all().order_by('-id')`:
    *   `Post.objects`: `Post` 모델의 매니저(Manager)입니다. 데이터베이스와 상호작용하는 데 사용됩니다.
    *   `.all()`: `Post` 모델에 해당하는 데이터베이스 테이블에서 모든 레코드(게시물)를 가져옵니다.
    *   `.order_by('-id')`: 가져온 게시물들을 `id` 필드를 기준으로 내림차순(`-`는 내림차순을 의미) 정렬합니다. 즉, 가장 최근에 작성된 게시물이 목록의 맨 위에 오도록 합니다.
*   `return render(request, 'board/post_list.html', {'posts': posts})`:
    *   `render` 함수를 사용하여 `board/post_list.html` 템플릿을 렌더링합니다.
    *   `{'posts': posts}`는 템플릿으로 전달할 데이터를 딕셔너리 형태로 정의한 것입니다. 템플릿 안에서는 `posts`라는 이름으로 이 게시물 목록에 접근할 수 있습니다.

### `post_detail(request, pk)` 뷰 함수

*   **역할**: 특정 게시물의 상세 내용을 보여주는 페이지를 처리합니다.
*   `def post_detail(request, pk):`: 이 뷰 함수는 `request` 객체 외에 `pk`라는 추가 인자를 받습니다. 이 `pk`는 URL 라우팅(`board/urls.py`의 `<int:pk>`)에서 추출된 게시물의 고유 번호(기본 키)입니다.
*   `post = get_object_or_404(Post, pk=pk)`:
    *   `Post` 모델에서 `pk` 값이 일치하는 하나의 게시물을 데이터베이스에서 가져옵니다.
    *   만약 `pk`에 해당하는 게시물이 데이터베이스에 없으면, `get_object_or_404` 함수는 자동으로 404 Not Found 오류 페이지를 반환하여 사용자에게 해당 페이지를 찾을 수 없음을 알립니다.
*   `return render(request, 'board/post_detail.html', {'post': post})`:
    *   `board/post_detail.html` 템플릿을 렌더링하고, `post`라는 이름으로 해당 게시물 객체를 템플릿에 전달합니다.

### 초보자를 위한 팁

*   뷰 함수는 항상 `request` 객체를 첫 번째 인자로 받습니다.
*   뷰 함수는 일반적으로 `render()` 함수를 사용하여 HTML 템플릿을 반환하거나, `HttpResponse()`를 사용하여 직접 텍스트 응답을 반환합니다.
*   뷰는 `models.py`를 통해 데이터베이스와 상호작용하고, `templates/` 폴더의 템플릿을 사용하여 사용자에게 보여줄 화면을 만듭니다. 즉, 뷰는 모델과 템플릿을 연결하는 다리 역할을 합니다.

`views.py`는 사용자의 요청에 따라 어떤 데이터를 처리하고 어떤 화면을 보여줄지 결정하는 Django 애플리케이션의 핵심 로직이 담겨 있는 곳입니다.
