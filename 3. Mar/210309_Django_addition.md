###### 210309_tue

# +Django

> 기본적인 내용과 추가적인 내용을 간략하게 정리해봅시다

### Django를 사용하는 목적

- 어떤 웹페이지를 제공하는 프로그램을 만드는 것(__서버__)

<br>

### 전체적인 틀

> 기본적인 틀은 변하지 않습니다!!

- url, view(어떤 동작을 거쳐서), template(어떻게 보여줄 것인지)
  - 요청을 보냈을 때, 어떤 처리를 거쳐서 어떤 화면을 보여줄 지 정해야함
  - 3개의 순서를 지켜서 작성하는 것이 실수할 확률을 줄여줍니다!
- urls.py
  - 어떤 url로 왔을 때, 어떤 동작을 해야할지 정의
- view.py
  - 어떤 동작을 해야되는지, 함수를 통해 코드 작성
- templates
  - 정보를 어떻게 표현할지 dtl로 작성

<br>

<br>

## url_name

> 각 url에 이름을 다는 것은 __중복__을 피하기위해서 입니다!!
>
> 또한 __유지보수를 쉽게__ 하기위해서입니다!

### 1. app별로 urls.py 작성

- url이 많아질수록 한 페이지에서 관리하기가 어렵습니다
- 그래서 app별로 urls.py를 만들어 사용하고, project의 셋팅 폴더인 urls.py에서는 이들을 한번에 모아줍니다.
- 이때 include를 사용합니다

#### <project_name>/urls.py

```python
from django.contrib import admin
from django.urls import path, include

urlpatterns = [
    path('admin/', admin.site.urls),
    path('<app_name>/', include('<app_name>.urls')),
]
```

#### <app_name>/urls.py

- from . : 현재 위치에서 가져오겠다는 의미입니다

```python
from django.urls import path
from . import views

urlpatterns = [
    path('<url1_name>/', views.url1, name='url1'),
    path('<url2_name>/', views.url2, name='url2'),
    path('<url3_name>/', views.url3, name='url3'),
]
```

<br>

### 2. url_name 활용

- 만약 url을 경로로 작성해뒀다면??
- url의 위치가 바뀔경우, 해당 url을 쓴 위치를 모두 바꿔줘야합니다

#### form 태그에서 url을 직접 사용한 경우

- 1번처럼 url을 전부 app의 urls.py로 위치를 바꿔준다면 url을 아래처럼 변경해줘야합니다

```python
<form action="<app_name>/url1/" method="GET">
    <label for="message">Throw: </label>
    <input type="text" name="message" id="message">
    <input type="submit">
</form>
```

- url_name을 쓴다면 번거롭게 url을 수정할 필요가 없습니다!

```python
<form action="{% url url1 %}" method="GET">
    <label for="message">Throw: </label>
    <input type="text" name="message" id="message">
    <input type="submit">
</form>
```

<br>

<br>

## Template의 상속

> 중복되는 일을 줄이기 위해 상속을 사용합니다!!

- 웹페이지를 만들다보면 비슷하게 작성하는 것들이 생깁니다
- 반복되는 부분은 상속을 사용하고, block부분만 빈칸을 두어 템플릿마다 원하는데로 수정할 수 있도록 합니다
- block는 여러개 만들 수 있으나, 이때 block이름을 중복되지 않도록 설정해야 합니다

```python
##base.html
<!DOCTYPE html>
<html lang="en">
<head>
  ---생략---
</head>
<body>
  <div class="container">
    {% block content %}
    {% endblock %}  #이름을 써도되고 안써도 됨
  ---생략---
</body>
</html>
```

- 상속해서 사용하는 경우 extends를 가장 상단에 적어줘야합니다

```python
{% extends 'base.html' %}

{% block content %}
  ---내용---
{% endblock %}
```

- 이를 위해 임의의 위치에 templates를 추가한 경우, settings.py에 경로를 추가합니다 (보통 project에 추가합니다)

```python
##<project_name>/setting.py
TEMPLATES = [
    {
        ---생략---
        'DIRS': [BASE_DIR / '<project_name>' / 'templates',],
        'APP_DIRS': True,
        ---생략---
    },
]
```

