# 4. `models.py`: 데이터베이스 설계

`models.py` 파일은 Django 애플리케이션의 데이터베이스 구조를 정의하는 곳입니다. Django는 ORM(Object-Relational Mapping)이라는 강력한 기능을 제공하여, 파이썬 코드로 데이터베이스 테이블을 정의하고 데이터를 조작할 수 있게 해줍니다. 즉, SQL 쿼리를 직접 작성하지 않고도 데이터베이스와 상호작용할 수 있습니다.

## `board/models.py` 파일 살펴보기

우리 프로젝트의 `board/models.py` 파일에는 `Post`라는 모델이 정의되어 있습니다.

```python
# board/models.py

from django.db import models

class Post(models.Model):
    title = models.CharField(max_length=200)
    content = models.TextField()

    def __str__(self):
        return self.title
```

### 코드 설명

*   `from django.db import models`: Django의 모델 기능을 사용하기 위해 필요한 모듈을 임포트합니다.
*   `class Post(models.Model):`:
    *   `Post`는 우리가 만들 게시물(Post)을 나타내는 모델의 이름입니다. 이 모델은 데이터베이스에서 `board_post`라는 이름의 테이블로 생성됩니다.
    *   `models.Model`을 상속받음으로써 `Post` 클래스는 Django 모델로서의 모든 기능을 갖게 됩니다.
*   `title = models.CharField(max_length=200)`:
    *   `title`은 게시물의 제목을 저장할 필드(컬럼)입니다.
    *   `models.CharField`는 짧은 문자열을 저장할 때 사용되는 필드 타입입니다.
    *   `max_length=200`은 제목의 최대 길이를 200자로 제한한다는 의미입니다. `CharField`에는 `max_length`가 필수입니다.
*   `content = models.TextField()`:
    *   `content`는 게시물의 내용을 저장할 필드입니다.
    *   `models.TextField`는 긴 텍스트를 저장할 때 사용되는 필드 타입입니다.
*   `def __str__(self):`:
    *   이 메서드는 `Post` 객체를 문자열로 표현할 때 어떻게 보여줄지 정의합니다. 예를 들어, 관리자 페이지에서 `Post` 객체를 볼 때 `Post object (1)` 대신 게시물의 `title`이 보이도록 합니다.
    *   `return self.title`: `Post` 객체를 문자열로 변환하면 해당 게시물의 제목을 반환하도록 설정합니다.

## 마이그레이션 (Migrations): 모델 변경 사항을 데이터베이스에 반영하기

`models.py` 파일을 수정하여 모델을 변경(새로운 모델 생성, 필드 추가/수정/삭제 등)했다면, 이 변경 사항을 실제 데이터베이스에 적용하는 과정이 필요합니다. 이 과정을 **마이그레이션**이라고 합니다.

### 마이그레이션 과정

1.  **마이그레이션 파일 생성**: `models.py`의 변경 사항을 감지하여 데이터베이스에 적용할 수 있는 파이썬 코드를 생성합니다.
    ```bash
    python manage.py makemigrations board
    ```
    *   `board`는 변경 사항을 감지할 앱의 이름입니다. 이 명령을 실행하면 `board/migrations/` 폴더 안에 `0001_initial.py`와 같은 새로운 파일이 생성됩니다. 이 파일이 바로 데이터베이스 변경 스크립트입니다.

2.  **마이그레이션 적용**: 생성된 마이그레이션 파일을 실제 데이터베이스에 적용하여 테이블을 생성하거나 변경합니다.
    ```bash
    python manage.py migrate
    ```
    *   이 명령은 프로젝트의 모든 앱에 대한 마이그레이션 파일을 검사하고, 아직 적용되지 않은 마이그레이션을 데이터베이스에 적용합니다. 이 과정을 통해 `Post` 모델에 해당하는 `board_post` 테이블이 `db.sqlite3` 파일 안에 생성됩니다.

### 초보자를 위한 팁

*   `models.py`를 수정할 때마다 `makemigrations`와 `migrate` 명령을 순서대로 실행하는 것을 잊지 마세요. 이 두 명령은 Django 개발에서 데이터베이스와 관련된 가장 기본적인 작업입니다.
*   `migrations` 폴더 안의 파일들은 직접 수정하지 않는 것이 좋습니다. Django가 자동으로 생성하고 관리합니다.

`models.py`는 웹 애플리케이션의 데이터를 어떻게 저장하고 관리할지 정의하는 매우 중요한 부분입니다. 데이터 모델을 잘 설계하는 것이 안정적이고 효율적인 애플리케이션을 만드는 첫걸음입니다.
