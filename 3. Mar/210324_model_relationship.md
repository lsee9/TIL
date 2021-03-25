###### 210322_mon

# DB(1)

<br>

# 1. Model Relationship 1

> 여러 모델간의 관계에대해 배워보겠습니다

## 1.1 Relationship Fields

> 모델간의 관계를 나타내는 필드

- **Many to one (1 : N)** :heavy_check_mark: Today!
  - ForeignKey()

- Many to Many (M : N)

  - ManyToManyField()

- One to One (1 : 1)

  - OneToOneField()

<br>

<br>

## 1.2 게시글과 댓글의 관계??

#### Article : Comment 

- 1 : N
- 한 게시글에 여러개의 댓글이 달림

#### 서로에 대한 데이터... 누가 갖고있나?

1. Article이 Comment의 정보를 갖는 경우

   - DB의 한 column에 댓글에 대한 정보 저장
   - 여러개인 경우 : '1, 2, 3' (댓글 번호가 문자열로 저장)

     ##### :fire:  문제점
   
     - 문자열을 변환하는 작업 필요!
     - DB의 한 col에 하나의 값을 넣는걸 권장..?(이거 들은거같은데 어디적어놨나 안보이넹)
   
2. Comment가 Article의 정보를 갖는 경우

   - Article의 유일한 값(pk)를 DB의 Column에 저장
   - 하나의 데이터만 작성하여 관계를 표현할 수 있음

##### 추가적인 데이터는 `N`이 소유합니다!!

<br>

<br>

# 2. A many to one relationship

## 2.1 Foreign Key (외래 키)

- RDBMS에서 한 테이블의 필드(`comment의 article 필드`) 중 다른 테이블(`article`)의 행을 식별할 수 있는 키
- 참조하는 테이블에서 참조되는 측의 기본 키(`pk`)를 가짐
- 하나의 테이블이 여러개의 외래키 포함 가능

- 참조하는 테이블의 행 여러개가, 참조되는 테이블의 동일한 행을 참조할 수 있다
  - 서로다른 댓글이, 한 게시글에 달릴 수 있다
- 재귀적 외래키 : 대댓글(나중에 도전해보기)
  - 참조하는 테이블과 참조되는 테이들이 동일한 경우

### 특징

- 참조 무결성
  - 키를 사용하여 부모 테이블의 **유일한 값을 참조**
  - 즉, pk가 아니라도 구현할 수 있지만 유일한 값이어야 한다!!!

> #### 데이터 무결성의 법칙
>
> 1. 개체 무결성
>    - 모든 테이블이 기본 키(primary key)를 가져야한다
> 2. 참조 무결성
>    - 모든 외래 키 값은 두 가지 상태 가운데 하나에만 속함
> 3. 범위 무결성
>    - 정의된 범위에서 관계형 데이터베이스의 모든 열이 선언되어야 한다

<br>

### Django에서의 Foreign Key

#### ForeignKey()

- 1 : N 을 표현하기 위한 model field
- 2개의 필수 위치 인자
  - 참조하는 모델 클래스
  - on_delete

```python
class Comment(models.Model):
    article = models.ForeignKey(Article, on_delete=models.CASCADE)
```

#### on_delete

- 참조하는 객체가 사라졌을 때, ForeignKey를 가진 객체를 어떻게 처리할 지 정의
  - 게시글이 사라진 경우 댓글은 어떻게 할 것인가!
- 데이터 무결성(Database Integrity)을 위해 매우 중요한 설정
- 종루가 다양하지만 나머지는 [공식 문서](https://docs.djangoproject.com/en/3.1/ref/models/fields/#arguments)를 참고합시다!

##### 우리가 사용할 것!!

- **CASCADE** 
  - 부모 객체(참조된 객체)가 삭제된 경우, 이를 참조하는 객체도 삭제

<br>

<br>

## 2.2 댓글모델 구현하기

> 06_django_model_relationship에서 시작합니다
>
> project : crud / app : articles, accounts

#### 기본 시작 과정!!

- 가상환경

```shell
$ python -m venv venv
$ source venv/Scripts/activate
```

- 필요한 프로그램 설치

```shell
$ pip install -r requirements.txt
```

### 모델 작성

> app의 역할이 나뉘어 있습니다.
>
> - accounts : 인증, User
>
> - articles : 게시글
>
> 따라서 `articles`에서 작성해봅시다

- articles/models.py
  - 두번째 모델인 Comment를 작성합니다

```python
class Comment(models.Model):
    #참조하는 모델의 단수형 소문자 = ForeignKey(to=모델명, on_delete=)
    article = models.ForeignKey(Article, on_delete=models.CASCADE)

    #댓글에 필요한 column
    content = models.CharField(max_length=200)
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    #함수 호출시 출력되는 형태 지정
    def __str__(self):
        return self.content
```

- migrations

```shell
$ python manage.py makemigrations
$ python manage.py migrate
```

- DB 확인 : `articles_comment` (app이름_모델명)
  - `article_id` (class변수명_id)으로 table의 field 만들어짐

- articles/admin.py
  - 관리를 위한 모델등록

```python
from django.contrib import admin
from .models import Comment

admin.site.register(Comment)
```

- 관리자 계정 생성

```shell
$ python manage.py createsuperuser
```

<br>

### 댓글 작성 (shell plus)

> django-extensions을 설치한 경우 사용가능!
>
> 즉각적인 출력을 확인할 때 사용합니다

- 실행

```shell
$ python manage.py shell_plus
```

- 댓글 작성

```shell
## 댓글 작성
comment = Comment()  #인스턴스생성
comment.content = '댓글1'
comment.save()  #Error, Not Null : 몇 번 게시들에 달린 글인지 정보 필요

## 게시글 생성
Article.objects.create(title='1번글', content='1번입니다')
article = Article.objects.get(pk=1)

##article_id 작성
comment.article = article  #객체를 통째로 전달
comment.save()  #저장
```

- 정보 확인

```shell
comment				#<Comment: 댓글1>
comment.pk			#1
comment.content		#'댓글1'
comment.article		#<Article: Article object (1)>
comment.article_id	#1
comment.article.pk	#1
comment.article_pk	#AttributeError, 해당 column 존재X
```

<br>

<br>

## 2.3 1:N model manager

> 이전까지는 object라는 모델 매니저를 사용했죠!

### 참조

- Comment(N)가 Article(1) 참조
  - article

```python
Comment.article
```

### 역참조

- Article(1)이 Comment(N)를 참조
  - comment_set
  - `모델이름_set` 형식의 manager

```python
article.comment_set.all()
```

<br>

### 역참조 사용(shell_plus)

> 게시글에 어떤 댓글이 달렸는지 확인하기위해 사용합니다!

- QuerySet의 반환값 존재

```shell
article = Article.objects.get(pk=1)
article  #<Article: Article object (1)>

article.comment_set.all()  #<QuerySet [<Comment: 댓글 1>, <Comment: 댓글2>]>
```

- 변수에 담아 for문같은데 사용 가능

```shell
comments = article.comment_set.all()
comments  #<QuerySet [<Comment: 댓글 1>, <Comment: 댓글2>]>
```

<br>

<br>

## 2.4 ForegnKey's Arguments

### related_name

- 역참조 manager( _set manager) 변경할 이름 설정
- `M:N 관계`에서 사용하는 상황 반드시 존재!! (다음번에 사용하자)

#### 예시

- 바꾸기 전 : article.comment_set.all()
- 바꾼 후 : article.comments.all()
- 한 번 바꾸면 이전의 값은 사용할 수 없습니다

```python
class Comment(model.Model):
    article = models.ForeignKey(
        Article,
        on_delete=models.CASCADE,
        related_name='comments'
    )
```

<br>

<br>

# 3. 댓글 CRD

## 댓글 작성 (Create)

> 댓글 작성하는 form을 detail 페이지에서 보여줍니다
>
> 작성된 게시글을 처리합니다

### 댓글 작성 Form 만들기

- articles/forms.py

  ##### :fire: fields = '\_\_all\_\_' 의 문제점

  - 댓글 작성 시, 어떤 글에 댓글을 작성할 지 선택할 수 있다 (너무 많은 권한)

  #####  :four_leaf_clover: exclude = ('article',) 로 해결
  
  - 전체 field 중 article만 제외

```python
from .models import Comment

class CommentForm(forms.ModelForm):

    class Meta:
        model = Comment
        # fields = '__all__'
        exclude = ('article',)
```

- articles/views.py - detail
  - 보통 게시글 아래에 작성하므로, detail에서 form을 보여주자

```python
from .forms import CommentForm

@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()  #빈 form 전달
    context = {
        'article': article,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail.html', context)
```

- templates/articles/detail.html
  - 아직 create하는 부분은 만들지 않았으므로 action은 비워줍니다
  - comment_form.as_p
    - p태그로 감싸지면서 제출 버튼이 아래로 내려가 빼주었습니다

```html
{% block content %}
  <h2>DETAIL</h2>
  ...
  <hr>
  <h4>Comment</h4>
  <form action="#" method="POST">
    {% csrf_token %}
    {{ comment_form }}
    <input type="submit" value="Submit">
  </form>
{% endblock %}
```

<br>

### 댓글 처리하기

- articles/urls.py
  - 댓글 작성에 필요한 것
    - 댓글 내용(form으로 전달) + 몇번 글의 댓글(url로 article_pk 받음)

```python
urlpatterns = [
   ...
    path('<int:article_pk>/comments/', views.comments_create, name='comments_create'),
]
```

- templates/articles/detail.html
  - form의 action작성

```html
<form action="{% url 'articles:comments_create' article.pk %}" method="POST">
```

- articles/views.py - comments_create

  - POST만 처리

    - GET : detail 에서 form을 보여주는 역할을 수행해줍니다
    - POST : 댓글을 작성해서 처리하는 경우

    ##### :fire:`comment_form.save()`  의 문제점

    - IntegrityError : NOT NULL ~
    - article_id가 빈 상태 입니다 (form에서 article을 제외했기 때문)

    #####  :four_leaf_clover: `comment_form.save(commit=False)` 로 해결

    - DB에 입력하지 않은 상태로 대기한체 인스턴스 반환
    - 추가적인 내용을 작성할 시간을 줍니다

```python
@require_POST        
def comments_create(request, article_pk):
    article = get_object_or_404(Article, pk=article_pk)
    comment_form = CommentForm(request.POST)  #form에 작성한 정보
    if comment_form.is_valid():  #유효성 검사
        comment = comment_form.save(commit=False)
        comment.article = article  #객체 전달
        comment.save()  #저장
        # detail로 재요청
        return redirect('articles:detail', article.pk)
    ## 유효성 검사 통과 못한 경우
    context = {
        'article': article,
        'comment_form': comment_form,
    }
    return render(request, 'articles/detail', context)
```

#### :mag_right: redirect VS render

> 차이를 알고 적절하게 사용합시다!

- redirect
  - 해당 주소로 새롭게 요청을 보내는 것
- render
  - 에러메세지(유효성 검사에 실패한 경우)를 포함한 템플릿을 보냄
  - 현재의 정보를 함께 보내는 느낌이랄까..?(이건 그냥 내 생각...)

<br>

<br>

## 댓글 조회 (Read)

> detail 페이지 댓글 작성 위에 쌓아보겠습니다

- articles/views.py - detail
  - 모든 댓글 정보를 전달합니다

```python
@require_safe
def detail(request, pk):
    article = get_object_or_404(Article, pk=pk)
    comment_form = CommentForm()
    comments = article.comment_set.all()  #모든 댓글 정보 전달
    context = {
        'article': article,
        'comment_form': comment_form,
        'comments': comments,
    }
    return render(request, 'articles/detail.html', context)
```

- templates/articles/detail.html
  - for문을 이용해 댓글을 한 줄씩 출력합니다
  - comment.content로 내용을 출력함을 명시해줍니다

```html
{% block content %}
  ...
  <hr>
  <h4>Comment</h4>
  <ul>
    {% for comment in comments %}
      <li>{{ comment.content }}</li>
    {% endfor %}
  </ul>
  ...
{% endblock %}
```

<br>

<br>

## 댓글 삭제 (Delete)

> 댓글을 삭제하려면?? 몇번 댓글인지 알아야한다!
>
> 댓글을 삭제하면 여전히 detail 페이지... 그럼 몇번 게시글인지도 알아야한다!!

- articles/urls.py
  - 몇번 게시글인지, 몇번 댓글인지 정보가 필요하다
  - url에도 명시적으로 나타나는 것이 좋다!

```python
urlpatterns = [
    ...
    path('<int:article_pk>/comments/<int:comment_pk>/delete/', views.comments_delete, name='comments_delete'),
]
```

- articles/views.py - comments_delete
  - 삭제하는 POST 요청에만 반응합니다
  - 해당 comment를 가져와 삭제합니다

```python
from .models import Comment

@require_POST
def comments_delete(request, article_pk, comment_pk):
    comment = get_object_or_404(Comment, pk=comment_pk)
    comment.delete()
    return redirect('articles:detail', article_pk)
```

- templates/articles/detail.html

```html
{% block content %}
  ...
  <hr>
  <h4>Comment</h4>
  <ul>
    {% for comment in comments %}
      <li>
        {{ comment.content }}
          
        <form action="{% url 'articles:comments_delete' article.pk comment.pk %}" method="POST">
          {% csrf_token %}
          <input type="submit" value="DELETE">
        </form>
          
      </li>
    {% endfor %}
  </ul>
  ...
{% endblock %}

```

<br>

<br>

#### 댓글 수정

> - 수정은 페이지 전환없이 수정할 부분만 활성화되는게 보통입니다
>- 현재까지 배운 것은 모두 페이지 전환이 이뤄집니다
> - 이를 위해서는 JS(Javascript)가 필요합니다!
> - 따라서 수정은 구현하지 않습니다

<br>

<hr>

<br>

# 4. Comment CRD 추가사항

> 기능적으로 추가하면 좋을 부분을 보완해봅시다!

## 4.1 댓글 작성, 삭제 : 로그인한 사람만 가능

#### login_required를 쓸까요??

- 현재 required_POST 사용!!

  ##### :fire: 두개를 같이 쓰면 어색함이 있답니다

  - login_required
    - 로그인 안하고 접근시, next parameter와 함께 login 페이지로!
    - 로그인 성공시, next에 있는 url을 시도(그 전에 시도했던 url)
    - 이때!!! 이는 GET요청입니다
    - 그래서 required_POST에 막혀 **405메세지**를 만납니다(흐름상 막힘)
  - 따라서 GET method를 처리할 수 있는 함수인 경우만 두개를 같이 쓸 수 있습니다

  ##### :four_leaf_clover: 해결방법?

  - 둘 중에 하나를 선택하고, 한가지 동작은 내부에서 처리할 수 있도록 해야겠죠?
  - 남기고싶은걸 선택해서 처리해봅시다

#### require_POST 를 남기고 작성해보자

- articles/views.py - comments_create
  - 로그인 안한 유저는 로그인 페이지로 보내겠습니다

```python
@require_POST        
def comments_create(request, article_pk):
    if request.user.is_authenticated:
        #댓글 작성 처리하는 부분
        return render(request, 'articles/detail', context)
    return redirect(request, 'accounts:login')  #로그인 안한경우, 로그인페이지로 재요청
```

- articles/views.py - comments_delete
  - 위에처럼 동일하게 로그인 페이지로 가도 됩니다
  - 여기서는 detail 페이지를 그대로 보여주겠습니다

```python
@require_POST
def comments_delete(request, article_pk, comment_pk):
    if request.user.is_authenticated:
        comment = get_object_or_404(Comment, pk=comment_pk)
        comment.delete()
    return redirect('articles:detail', article_pk)
```

#### 에러페이지를 보여주고 싶은 경우

> https://www.django-rest-framework.org/api-guide/status-codes/

- http status code
  - `401_UNAUTHORIRED`를 보내면 됩니다
  - 정확한 상태 코드가 없는경우 http 응답을 반환하는 방식을 사용합니다

```python
from django.http import HttpResponse  #http 응답을 반환

@require_POST
def comments_create(request, pk):
    if request.user.is_authenticated:
        #댓글 작성 처리하는 부분
        return render(request, 'articles/detail.html', context)
    return HttpResponse(status=401)  #화면에 401 에러를 표시해줍니다
```

<br>

## 4.2 댓글 개수를 출력하자

#### 개수 출력하는 방법

> 댓글의 개수가 더 많아진다면 첫번째, 두번째 사용하는게 더 낫다고 합니다

- `{{ comment|length }}`
- `{{ article.comment_set.all|length }}`

- `{{ comments.count }}`

#### 댓글이 없다면 없다는 메세지 띄워주기

- templates/articles/detail.html

```html
<hr>
<h4>Comment</h4>
<p>{{ comments|length }}개의 댓글이 있습니다.</p>
<ul>
    {% for comment in comments %}
        <li>
            {{ comment.content }}
            <!-- form부분 -->
        </li>
    {% empty %}
    	<p>아직 댓글이 없습니다...</p>
    {% endfor %}
</ul>
```

