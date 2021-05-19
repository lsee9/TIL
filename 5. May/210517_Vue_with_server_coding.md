###### 210517_mon

##### Vue with server

<hr>





###### 오늘의 주제 :computer:

### Vue with server coding

- 유저가 없는 Todo
  - CORS Headers
- 유저가 있는 Todo
  - Authentication (JWT)

##### :clipboard: 직접 적용해보자!!

<hr>



<br>

### 초기 상황

> 210518폴더의 06~prac 폴더!!
>
> 제공된 템플릿을 사용하는 만큼... 뭐가 있는지는 파악하고 가쟈!!

- `server`(django)

  - project : **mypjt**

  - app

    - **todos**

      모든 todo 불러오고, 새로 생성하는 부분

      todo를 변경하고 삭제하는 부분

      model, serializer 구현된 상태

    - **accounts**

      회원가입하는 부분

      custom user model 작성됨

      serializer 작성 필요

- `client`

  - **App.vue**

  - views

    - accounts

      **Login.vue**

      **Signup.vue**

    - todos

      **CreateTodo.vue**

      **TodoList.vue**



### 초기 설정 :star:

> 파일을 받아온 뒤에는 반드시 설정을 먼저 해주자!!

#### Server

- 가상환경 설정

```shell
$ python -m venv venv
$ source venv/Scripts/activate
```



- 필요한 프로그램 설치
  - cors에 필요한 라이브러리도 이미 포함된 상태!

```shell
$ pip install -r requirements.txt 
```



- DB작성 (migrate)

```shell
$ python manage.py migrate
```



- 서버 켜기

```shell
$ python manage.py runserver
```





#### Client

- node modeuls 설정

```shell
$ npm i
```

- 서버 켜기

```shell
$ npm run serve
```



###### 그럼 일단... 뭐가 필요한지 파악 한 뒤 하나하나 만들어보자

# 1. 유저가 없는 Todo

> 유저가 없는 경우의 todo작성!

### 요청, 응답형태 파악하기!

- url : `/todos/`

  - **Todo list** 가져오기 (GET)

    - 요청 형태

      따로 보내줄 것은 없다! 달라는 요청을 보낸다

    - 응답 형태 : Array

    ```python
    # 응답형태 예측은 모델 구성과 다름 없다!
    [
      {
    	id,
      	title: string,
    	completed: Boolean,
      }
    ],
    ```

    

  - Todo **create** (POST)

    - 요청 형태 (전달할 데이터)

    ```python
    #id: model에서 자동 생성
    title
    #completed: false를 default로 생성
    ```

    - 응답 형태

    ```python
    {id, title, completed}
    status code  #선택!
    ```

    

- url : `/todos/<int:todo_pk>/`

  - Todo **update** (PUT)

    - 요청 형태

    ```python
    # 변경된 값을 새로 저장
    {id, title, completed}
    ```

    - 응답형태 ?

    ```python
    {id, title, completed}
    status code  #선택!
    ```

    

  - Todo **delete** (DELETE)

    - 요청 형태

      따로 보내줄 것은 없다!, 삭제할 todo의 id로 접근한다

    - 응답형태?

      삭제된 id를 전달? 꼭 필요한 것은 없다

      

###### :sweat_drops: 아니... 좀 헷갈리고 어려운데ㅜㅜㅜ 이건 만들어진거라 이정도지 첨부터 작성할 생각하면 막막...

<br>

<br>

###### :grey_exclamation: 이 부분은 거의 다 작성되어있기 때문에 흐름에 맞춰 살펴봅시다

## 1.1. Todo List 

> Todo를 작성하여 보여주고, 새로운 todo를 작성합시다!
>
> 또한 업데이트와 삭제가 이뤄집니다

### 구성하기

0. 전체적인 그림은 이미 그려져 있습니다
1. **markup 작성**
   - todo가 보여질 부분
   - todo를 불러오는데 사용할 버튼
   - todo작성하는데 필요한 input
   - todo 삭제할 버튼
   
2. **data 파악**
   - 시간에 따라 변할 수 있는 데이터 : `todo list`
   - data에 todo list를 담을 배열인 `todos`를 정의합니다
3. **data**와 **template** 연동
   - todos는 `v-for`을 사용해 하나씩 보여집니다
   - 이때 key는 DB에 저장된 id를 사용합니다

4. **이벤트**, **life cycle hook** (클릭했을 때 \~~\~게 된다)

   - 처음 `페이지에 들어왔을 때`, 전체 `todo list를 가져와야`한다
     - 버튼을 눌러서도 가져올 수 있다
   - todo의 title을 클릭하면 completed 값을 바꿀 수 있고, 이에따라 class가 토글됩니다
- delete 버튼을 누르면 삭제된다

<br>

### 1.1.1. Todo list 가져오기

### Server

#### :cherries: CORS를 위한 설정 :cherries:

###### vue와 django가 데이터를 주고받기 위해서는 필수!!!

- mypjt/**settings.py**

  - INSTALLED_APPS / MIDDLEWARE

  ```python
  INSTALLED_APPS = [
      'todos',
      'accounts',
      'rest_framework',
      # django cors
      'corsheaders',
      ...
  ]
  
  MIDDLEWARE = [
      # django cors middleware setting
      'corsheaders.middleware.CorsMiddleware',
      ...
  ]
  ```

  

  - Origin 허용
    - 일부만 허용할 수도 있지만, 전체를 허용하는 것이 편합니다!

  ```python
  # 1. 특정 Origin만 선택적으로 허용
  # CORS_ALLOWED_ORIGINS = [
  #     "https://example.com",
  #     "https://sub.example.com",
  #     "http://localhost:8080",  #vue
  #     "http://127.0.0.1:9000"
  # ]
  
  # 2. 모든 Origin 허용
  CORS_ALLOW_ALL_ORIGINS = True
  ```

  

##### todos

- **models.py**

  - 전달해야한 데이터 구성 및 타입을 기준으로 작성

  ```python
  from django.db import models
  
  class Todo(models.Model):
      title = models.CharField(max_length=50)
      completed = models.BooleanField(default=False)
  
      def __str__(self):
          return self.title
  ```

- **serializers.py**

  - data 형식을 변경하여 넘기기 위함
  - 모든 필드를 전부 전달합니다

  ```python
  from rest_framework import serializers
  from .models import Todo
  
  class TodoSerializer(serializers.ModelSerializer):
      
      class Meta:
          model = Todo
          fields = ('id', 'title', 'completed',)
  ```

- **urls.py**

  ```python
  from django.urls import path
  from . import views
  
  urlpatterns = [
      path('', views.todo_list_create),
  ]
  ```

- views.py - **todo_list_create**

  - GET요청에 따라 `모든 todo list`를 전달합니다
  - serializer로 변환한 데이터를 전달합니다

  ```python
  @api_view(['GET', 'POST'])
  def todo_list_create(request):
      if request.method == 'GET':
          todos = Todo.objects.all()
          serializer = TodoSerializer(todos, many=True)
          return Response(serializer.data)
  
      elif request.method == 'POST':
          ...
  ```

  ##### :fire: 다른 형태로 데이터를 전달할 순 없을까?

  - dict 형태로 묶어버리면, 나중에 todoCount같은 추가 정보도 붙이기 쉬워집니다
  - 따라서 필요하다면 다음처럼 형태를 바꿀 수 있습니다
  
  ```python
  @api_view(['GET', 'POST'])
  def todo_list_create(request):
      if request.method == 'GET':
          todos = Todo.objects.all()
          serializer = TodoSerializer(todos, many=True)
          context = {
              todos: serializer.data,
          }
          return Response(context)
  ```
  
  

### Client

##### TodoList.vue

###### script

- `getTodos()`

  - 요청 양식에 맞게 axios 요청을 보내고 응답을 받습니다
    - method : 'GET'
    - url : `'http://127.0.0.1:8000/todos/'`

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    name: 'TodoList',
    data: function () {
      return {
        todos: [],
      }
    },
    methods: {
      getTodos: function () {
        axios({
          method: 'get',
          url: 'http://127.0.0.1:8000/todos/'
        })
          .then((res) => {
            console.log(res)
            this.todos = res.data
          })
          .catch((err) => {
            console.log(err)
          })
      },
      ...
  }
  </script>
  ```

  

- `created`

  - TodoList에 들어오면 모든 list를 자동으로 불러옵니다

  ```vue
  <script>
  ...
  export default {
    ...
    created: function () {
      this.getTodos()
    }
  }
  </script>
  ```

###### template

- `v-for`

  - todos의 todo를 하나씩 보여줍니다

- button을 click해도 getTodos가 실행됩니다

  ```vue
  <template>
    <div>
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <span>{{ todo.title }}</span>
          <button class="todo-btn">X</button>
        </li>
      </ul>
      <button @click="getTodos">Get Todos</button>
    </div>
  </template>
  ```

<br><br>

### 1.1.2. Todo Create

> todo를 작성하자!!
>
> TodoList에서 만들어줄 수도 있지만, router를 사용하기위해 나눠주었습니다!

### Server

- **models.py**

  - completed값이 `default false`로 정해져 있기때문에 새로운 값을 작성할 때, 전달할 필요가 없습니다

  ```python
  from django.db import models
  
  class Todo(models.Model):
      title = models.CharField(max_length=50)
      completed = models.BooleanField(default=False)
  
      def __str__(self):
          return self.title
  ```

- **serializers.py**

  - 모든 값이 필드로 지정되어있으므로, `is_valid`에서 모든 값이 존재해야 통과합니다
  - 검증에 사용하지 않고 사용하기만 하려면 `read_only_fields = ('completed', )` 이렇게 쓸 수 있습니다!

  ```python
  from rest_framework import serializers
  from .models import Todo
  
  class TodoSerializer(serializers.ModelSerializer):
      
      class Meta:
          model = Todo
          fields = ('id', 'title', 'completed',)
  ```


- views.py - **todo_list_create**

  - POST 요청에 따라 받아온 데이터(`request.data`)에 TodoSerializer를 적용해 가공합니다
  - 검증에 통과하면 DB에 저장한 뒤, 응답으로 data를 전달합니다
  - status는 선택!! 사용하면 더 단단하게 만들 수 있습니다

  ```python
  from rest_framework import status
  from rest_framework.response import Response
  from rest_framework.decorators import api_view
  
  from .serializers import TodoSerializer
  
  @api_view(['GET', 'POST'])
  def todo_list_create(request):
      if request.method == 'GET':
          ...
      elif request.method == 'POST':
          serializer = TodoSerializer(data=request.data)
          if serializer.is_valid(raise_exception=True):
              serializer.save()
              return Response(serializer.data, status=status.HTTP_201_CREATED)
  ```

  

### client

##### CreateTodo.vue

###### script

- `createTodo`

  - data에 정의된 **title**을 이용해 **새로운 todoItem**을 생성한 뒤, axios 요청을 보냅니다
    - method : POST
    - url : `'http://127.0.0.1:8000/todos/'`
    - data : todoItem

  ##### :thinking: 새로운 todo를 입력한 뒤, TodoList로 이동할 수는 없을까??

  - `this.$router.push({ name: 'TodoList' })`
  - 응답을 받은 뒤, TodoList로 바로 연결합니다!! 
  - name대신 url을 작성해도 OK

  ```vue
  <script>
  import axios from'axios'
  
  export default {
    name: 'CreateTodo',
    data: function () {
      return {
        title: '',
      }
    },
    methods: {
      createTodo: function () {
        const todoItem = {
          title: this.title,
        }
  
        if (todoItem.title) {
          axios({
            method: 'post',
            url: 'http://127.0.0.1:8000/todos/',
            data: todoItem
          })
            .then((res) => {
              console.log(res)
              this.$router.push({ name: 'TodoList' })
            })
            .catch((err) => {
              console.log(err)
            })
          }
      },
    }
  }
  </script>
  ```

  

###### template

- input태그에 `keyup.enter`가 발생하거나, button을 `click`하면 createTodo가 실행됩니다

- input태그에 입력된 값을 input에서도 사용하고, data에도 저장하기 위해 `v-model`로 양방향 바인딩 해줍니다!

  ```vue
  <template>
    <div>
      <input type="text" v-model.trim="title" @keyup.enter="createTodo">
      <button @click="createTodo">+</button>
    </div>
  </template>
  ```

<br>

###### client의 역할!!!!! 헷갈리지 말기

> DB, Data에 맞게 요청 보내기
>
> 요청이 성공적으로 처리되었을 때 화면 바꾸는 것!!! (자동으로 되지 않는다!!!!)

<br>

### 1.1.3. Todo Delete

> 필요없는 todo를 삭제합시다

### Client

##### TodoList.vue

###### template

- li태그로 표현되는 todo에 붙은 X button을 `click`하면, `deleteTodo`가 실행됩니다

- 이때 삭제되는 todo가 뭔지 알 수 있도록 함수의 인자로 전달합니다

  ```vue
  <template>
    <div>
      <ul>
        <li v-for="todo in todos" :key="todo.id">
          <span>{{ todo.title }}</span>
          <!-- 삭제 버튼!! -->
          <button @click="deleteTodo(todo)" class="todo-btn">X</button>
        </li>
      </ul>
      <button @click="getTodos">Get Todos</button>
    </div>
  </template>
  ```

  

###### script 

- `deleteTodo`

  - axios 요청을 보내 DB에서 해당 todo를 삭제합니다 (전달받은 todo를 통해 url에 접근)
    - method : delete
    - url : `http://127.0.0.1:8000/todos/${todo.id}/`

  ##### :heavy_check_mark: todo가 삭제되면, 기존 TodoList에서도 없어져야하지 않을까??

  - `this.getTodos()`를 실행하여, 저장되어있는 전체 todo list를 받아옵니다!

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    ...
    methods: {
      ...
      deleteTodo: function (todo) {
        axios({
          method: 'delete',
          url: `http://127.0.0.1:8000/todos/${todo.id}/`
        })
          .then((res) => {
            console.log(res)
            this.getTodos()
          })
          .catch((err) => {
            console.log(err)
          })
      },
    },
    ...
    }
  }
  </script>
  ```

  

### Server

- **urls.py**

  - todo_pk를 url에 받아주어 구분합니다

  ```python
  from django.urls import path
  from . import views
  
  urlpatterns = [
      ...
      path('<int:todo_pk>/', views.todo_update_delete),
  ]
  ```



- views.py - **todo_update_delete**
  - DELETE로 요청이 들어온 경우, DB에서 삭제한 뒤 삭제된 id를 응답합니다

  ```python
  from django.shortcuts import get_object_or_404
  from rest_framework import status
  from rest_framework.response import Response
  from rest_framework.decorators import api_view
  
  @api_view(['PUT', 'DELETE'])
  def todo_update_delete(request, todo_pk):
      todo = get_object_or_404(Todo, pk=todo_pk)
      if request.method == 'PUT':
          #...
      elif request.method == 'DELETE':
          todo.delete()
          return Response({ 'id': todo_pk })
  ```

  

<br>

### 1.1.4. Todo Update

> todo를 완료하면 취소선을 그어봅시다!!

### Client

##### TodoList.vue

###### template

- class `.completed`

  - text-decoration으로 취소선이 그어지도록 하는 클래스!
  - `todo.completed`에 따라 class 적용 여부를 결정합니다(true인 경우 취소선)

- todo를 `click`하면 `updateTodoStatus`를 실행하여 기존의 completed를 변경합니다

  - 변경된 todo가 무엇인지 알 수 있도록 인자로 넘겨줍니다

  ```vue
  <template>
    <div>
      <ul>
        <li v-for="todo in todos" :key="todo.id">
            <!-- 업데이트 부분 -->
          <span @click="updateTodoStatus(todo)" :class="{ completed: todo.completed }">{{ todo.title }}</span>
          <button @click="deleteTodo(todo)" class="todo-btn">X</button>
        </li>
      </ul>
      <button @click="getTodos">Get Todos</button>
    </div>
  </template>
  ```

###### script

- `updateTodoStatus`
  - 인자로 받은 todo를 그대로 복사한 뒤, completed만 반대로 바꿔준 **todoItem**을 생성하여 axios 요청을 보냅니다
    - method : PUT
    - url : `http://127.0.0.1:8000/todos/${todo.id}/`
    - data : todoItem

  ##### :anger: 응답을 받았는데... 왜 todo에 표시는 안되지??

  - 요청을 보내서 DB에 저장해줬을 뿐! 화면에 보여주기위해서는 client에서 변화가 필요합니다
  - delete처럼 getTodo를 다시 하는 것도 하나의 방법입니다
  - 전체 중 단 하나의 **todo의 completed만 변경**되면 되므로, 해당 값만 바꿔줍니다

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    ...
    methods: {
      ...
      updateTodoStatus: function (todo) {
        const todoItem = {
          ...todo,
          completed: !todo.completed
        }
  
        axios({
          method: 'put',
          url: `http://127.0.0.1:8000/todos/${todo.id}/`,
          data: todoItem,
        })
          .then((res) => {
            console.log(res)
            todo.completed = !todo.completed
          })
        },
      },
    ...
  }
  </script>
  ```

  

<br>

<br>

# 2. 유저가 있는 Todo

> Authentication (인증) 이 필요합니다!!!!
>
> 인증된 유저만 글을 작성하고, 자신의 것만 불러와서 볼 수 있어야겠죠???

### 필요한 구성 Check!!!

- **회원가입**한 뒤, **로그인**해야 todo 작성이 가능하다
- 누가 작성한 todo인지 구분해야한다

##### :star: 따라서, 기존 데이터에 User 정보가 필요하다!!!

<br>

#### 인증방식 - Token(JWT) :heavy_check_mark:

- 회원가입 후 로그인 정보 전달
- 비밀번호 암호화 및 토큰 생성
- 토큰 발급 후 client에게 전달
- local storage에 토큰 저장
- request header에 token을 붙여서 요청을 보낸다!!!
- decoding한 결과와 token이 일치하면 성공적으로 응답을 받을 수 있다

<br>

<br>

## 2.1. Model 구성하기

> User 사용을 위해서는 server에 model이 존재해야겠죠???
>
> 가장 먼저 server에 필요한 model을 정의하고, 적절한 serializer를 구성해봅시다!!!

### Server

###### 이미 Custon User model이 형성되어있으므로, 바로 시작합시다!

##### todos/`models.py` - `Todo`

- **User (1) : Todo (N) **

  - **ForeginKey**를 사용해 둘의 관계를 작성합니다!!!
  - model에서 user를 가져올 때는 **AUTH_USER_MODEL**을 사용합니다
  - related_name은 **todos**로 설정하겠습니다
    - 설정하지 않으면, 역참조시 todo_set을 사용합니다.

  ```python
  from django.db import models
  from django.conf import settings
  
  class Todo(models.Model):
      # user와 todo의 model관계 작성
      user = models.ForeignKey(
              settings.AUTH_USER_MODEL, 
              on_delete=models.CASCADE,
              related_name='todos'
          )
      title = models.CharField(max_length=50)
      completed = models.BooleanField(default=False)
  
      def __str__(self):
          return self.title
  ```

  

- **migrate**

  - 기존 DB를 지우고, 새로 migrate를 진행합시다! (지우는게 편해요...)

  ```shell
  $ python manage.py makemigrations
  $ python manage.py migrate
  ```

<br>

##### accounts/serializers.py - `UserSerializer`

###### signup에 사용될 부분입니다!

- **write_only=True**

  - password 데이터는 serializing해서 검증 및 암호화해야하지만, **응답으로 넘겨주진 않습니다**

- **get_user_model()**

  - model.py가 아닌 곳에서 user는 get_user_model을 사용합니다

  ```python
  from rest_framework import serializers
  from django.contrib.auth import get_user_model
  
  User = get_user_model()
  
  class UserSerializer(serializers.ModelSerializer):
  
      password = serializers.CharField(write_only=True)
      
      class Meta:
          model = User
          fields = ('username', 'password',)
  ```

  

<br>

## 2.2. Signup

> 회원가입을 만들어봅시다!
>
> 회원가입에는 앞으로 이 방식을 적용하면 됩니다@@@

### Server

##### account

- **urls.py**

  - `signup/`으로 경로를 지정해줍니다

  ```python
  from django.urls import path
  from . import views
  
  
  urlpatterns = [
      path('signup/', views.signup),
  ]
  ```

  

- **views.py - `signup`**
  - password를 확인 후 일치하면 serializing 합니다 (passwordConfirmation)
    - 일치하지 않으면 error에 대한 응답을 줍니다!
    - 따라서 이런 경우는 **status code** 주는 것이 유용합니다
  - **set_password** 통해 django의 hashing algorithm을 수행한 뒤 **password를 암호화**합니다
  - 암호화된 상태로 user 정보를 저장합니다.
  - write_only로 설정했기때문에, response에는 password가 포함되지 않습니다

  ```python
  from rest_framework import status
  from rest_framework.decorators import api_view
  from rest_framework.response import Response
  from .serializers import UserSerializer
  
  @api_view(['POST'])
  def signup(request):
  	#1-1. Client에서 온 데이터를 받아서
      password = request.data.get('password')
      password_confirmation = request.data.get('passwordConfirmation')
  		
  	#1-2. 패스워드 일치 여부 체크
      if password != password_confirmation:
          return Response({'error': '비밀번호가 일치하지 않습니다.'}, status=status.HTTP_400_BAD_REQUEST)
  		
  	#2. UserSerializer를 통해 데이터 직렬화
      serializer = UserSerializer(data=request.data)
      
  	#3. validation 작업 진행 -> password도 같이 직렬화 진행
      if serializer.is_valid(raise_exception=True):
          user = serializer.save()
          #4. 비밀번호 해싱 후 
          user.set_password(request.data.get('password'))
          user.save()
      # password는 직렬화 과정에는 포함 되지만 → 표현(response)할 때는 나타나지 않는다.
      return Response(serializer.data, status=status.HTTP_201_CREATED)
  ```

  

### Client

##### accounts/Signup.vue

###### template

- **submit**

  - form 태그에서 `submit`이벤트가 발생하면 `signup`을 수행합니다
    - 이때, 기존 form의 동작(화면 새로고침)을 없애기위해 prevent를 사용합니다 

- **v-model**

  - input 태그의 값이 사용됨과 동시에 data에 저장되어야 하므로 **양방향바인딩**을 수행합니다

  ```vue
  <template>
    <div>
      <h1>Signup</h1>
      <form @submit.prevent="signup">
        <div>
          <label for="username">사용자 이름: </label>
          <input type="text" id="username" v-model="username">
        </div>
        <div>
          <label for="password">비밀번호: </label>
          <input type="password" id="password" v-model="password">
        </div>
        <div>
          <label for="passwordConfirmation">비밀번호 확인: </label>
          <input type="password" id="passwordConfirmation" v-model="passwordConfirmation">
        </div>
        <button>회원가입</button>
      </form>
    </div>
  </template>
  ```

  

###### script

- **data**

  - 총 3개의 데이터가 필요합니다
  - `username`, `password`, `passwordConfirmation`
  - `''` 또는 `null`로 지정

- **signup**

  - 검증 및 저장을 위해 axios 요청을 보냅니다

    - method : POST

    - url: `http://127.0.0.1:8000/accounts/signup/`

    - data : Object 형태로 전달

      ```python
      {
        username: this.username,
        password: this.password,
        passwordConfirmation: this.passwordConfirmation,
      },
      ```

  - `this.$router.push({ name: 'Login' })`

    - 응답을 성공적으로 받아오면, 로그인 화면으로 전환됩니다
    - name대신 url을 직접 사용할 수도 있습니다

  ###### :recycle: 이건 추가적인 사항!!

  - `alert(JSON.stringify(err.response.data))`
    - 에러가 발생하면 해당 **에러 메세지를 띄워**줍니다
    - `console.dir(err)`로 확인한 결과 에러 메세지는 response.data에 Object형태로 존재합니다
    - `JSON.stringify`를 사용하여 Object의 내용이 보이도록 합니다(안쓰면 Object라고 보임)

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    name: 'Signup',
    data: function () {
      return {
        username: '',
        password: '',
        passwordConfirmation: '',
      }
    },
    methods: {
      signup: function () {
        axios({
          method: 'POST',
          url: 'http://127.0.0.1:8000/accounts/signup/',
          data: {
            username: this.username,
            password: this.password,
            passwordConfirmation: this.passwordConfirmation,
          },
        })
          .then(()=>{
            this.$router.push({ name: 'Login' })
          })
          .catch(err=>{
            // console.dir(err)  // 에러객체 전체 확인
            alert(JSON.stringify(err.response.data))
          })
      }
    }
  }
  </script>
  ```

  

<br>

<br>

## 2.3. Login

> 이제 로그인화면을 구성하고, 직접 로그인을 해봅시다!!!

### Server

###### 라이브러리에서 제공하는 로그인을 사용하겠습니다!!!

[공식문서 확인](https://jpadilla.github.io/django-rest-framework-jwt/) :point_left:

- **rest_framework** 설치

  - token 방식 인증 할 수 있는 라이브러리 입니다

  ```shell
  $ pip install djangorestframework-jwt
  $ pip freeze > requirements.txt 
  ```

##### account

- **urls.py**

  - 문서를 참고하여 url을 작성합니다

  ```python
  from rest_framework_jwt.views import obtain_jwt_token
  #...
  
  urlpatterns = [
      #...
      path('login/', obtain_jwt_token),
  ]
  ```

##### mypjt

- **settings.py**

  - `JWT_EXPIRATION_DELTA`를 사용해 token의 유효기간을 설정합니다
  - 기본은 seconds=300 (5분) 입니다
  - 유효기간은 짧은 것이 좋지만, 현재는 개발하는 동안 로그인을 유지하기 위해 5시간으로 늘려줍니다

  ```python
  import datetime
  
  JWT_AUTH = {
      'JWT_EXPIRATION_DELTA': datetime.timedelta(hours=5),
  }
  ```

###### :heavy_heart_exclamation: 서버에서 필요한 로그인 기능은 라이브러리로 완수!!

<br>

### Client

##### accounts/Login.vue

###### template

- **submit**

  - form에 `submit`이벤트가 발생하면 `login`을 수행합니다.
  - prevent를 사용해 기존 form의 동작을 무시합니다

- **v-model**

  - input 데이터의 **양방향 바인딩**을 위해 사용합니다

  ```vue
  <template>
    <div>
      <h1>Login</h1>
      <form @submit.prevent="login">
        <div>
          <label for="username">사용자 이름: </label>
          <input type="text" id="username" v-model="username">
        </div>
        <div>
          <label for="password">비밀번호: </label>
          <input type="password" id="password" v-model="password">
        </div>
        <button>로그인</button>
      </form>
    </div>
  </template>
  ```

  

###### script

