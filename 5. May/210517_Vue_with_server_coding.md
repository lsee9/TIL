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

- **login**

  - 로그인 요청과 함께 token을 받아오기위해 axios 요청을 보냅니다

    - method: POST (문서에 명시!!)

    - url : `http://127.0.0.1:8000/accounts/login/'`

    - data : Object형태로 전달

      ```python
      {
        username: this.username,
        password: this.password,
      },
      ```

  - `localStorage.setItem('jwt', res.data.token)`
    - 응답을 성공적으로 받아오면, **data.token**에 **jwt token**을 확인할 수 있습니다
    - 이를 통해 로그인 상태를 유지하기 위해서 local Storage에 token을 저장합니다
    - jwt라는 key값으로 token저장 (object 형태)
  - `alert(JSON.stringify(err.respones.data))`
    - 에러 발생시 에러메세지를 띄워줍니다

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    name: 'Login',
    data: function () {
      return {
        username: '',
        password: '',
      }
    },
    methods: {
      login: function () {
        axios({
          method: 'POST',
          url: 'http://127.0.0.1:8000/accounts/login/',
          data: {
            username: this.username,
            password: this.password,
          },
        })
          .then(res => {
            console.log(res)
            localStorage.setItem('jwt', res.data.token)
          })
          .catch(err => {
            // console.log(err)
            alert(JSON.stringify(err.respones.data))
          })
      }
    }
  }
  </script>
  ```

  

### 2.3.1. Login 상태 설정

>jwt가 local storage에 저장되어있다고... 로그인이 유지되냐구요..? 하하.. 그럴리가요 :sweat_smile:
>
>로그인 상태임을 따로 설정해줘야합니다!!!!!
>
>모든 화면에서 login 상태임을 확인할 필요가 있으므로, 가장 상위 컴포넌트인 **App.vue**에 데이터를 선언하도록 합니다

##### account/Login.vue

###### script

- **emit**

  - Login이 이뤄지면, loigin 상태임을 명시하도록 데이터 값을 변경하기 위해 이벤트의 발생을 알립니다

- `this.$router.push({ name: 'TodoList' })`

  - 로그인을 하면 바로 TodoList로 이동합니다

  ```vue
  <script>
  ...
  export default {
    ...
    methods: {
      login: function () {
        axios({
          ...
        })
          .then(res => {
            ...
            // 이벤트를 알리자
            this.$emit('login')
            // 바로 TodoList로 이동하자
            this.$router.push({ name: 'TodoList' })
          })
          .catch(err => {
            ...
          })
      }
    }
  }
  </script>
  ```



##### App.vue

###### script

- **data**

  - login 유무를 저장할 데이터인 `isLogin`을 Boolean값으로 정의합니다 (default false)

  ```vue
  <script>
  export default {
    name: 'App',
    data: function () {
      return {
        isLogin: false,
      }
    },
  }
  </script>
  ```

  

###### template

- **@login**

  - **router-view**에서 `login`이벤트를 감지하면, `isLogin = true`로 값을 변경합니다
  - 하위 컴포넌트가 router-view에 나타나는 형태이므로, 해당 위치에서 이벤트를 인지한다는 사실!!! :cactus:

- **:isLogin="isLogin"**

  - 하위 컴포넌트에서 login상태를 확인할 수 있도록 isLogin을 내려줍니다

- **v-if**

  - 로그인 `안한` 경우
    - Signup, Login 페이지만 확인 가능
  - 로그인 `한` 경우
    - TodoList, CreateTodo 페이지만 확인 가능

  ```vue
  <template>
    <div id="app">
      <div id="nav">
        <span v-if="isLogin">
          <router-link :to="{ name: 'TodoList' }">Todo List</router-link> | 
          <router-link :to="{ name: 'CreateTodo' }">Create Todo</router-link>
        </span>
        <span v-else>
          <router-link :to="{ name: 'Signup' }">Signup</router-link> |
          <router-link :to="{ name: 'Login' }">Login</router-link> 
        </span>
      </div>
      <router-view @login="isLogin=true" :isLogin="isLogin"/>
    </div>
  </template>
  ```

<br>

#### 로그인 해야만 페이지에 접근할 수 있다!! :page_facing_up:

> TodoList와 CreateTodo에서도 Login을 한 경우만 실행!!!
>
> 아니면 Login페이지로 보내줍시다

##### TodoList.vue

- **props**

  - isLogin 데이터를 받아와 type를 지정합니다

- **created**

  - 시작되자마자 로그인되지 않은 경우 Login페이지로 보내줍니다

  ```vue
  <script>
  ...
  export default {
    name: 'TodoList',
    props: {
      isLogin: {
        type: Boolean,
      },
    },
    ...
    created: function () {
      if (!this.isLogin) {
        this.$router.push({ name: 'Login' })
      }
      this.getTodos()
    }
  }
  </script>
  ```

  

##### CreateTodo.vue

- **props**

  - isLogin 데이터를 받아와 type를 지정합니다

- **created**

  - 시작되자마자 로그인되지 않은 경우 Login페이지로 보내줍니다

  ```vue
  <script>
  ...
  export default {
    name: 'CreateTodo',
    props: {
      isLogin: {
        type: Boolean,
      },
    },
    ...
    created () {
      if (!this.isLogin) {
        this.$router.push({ name: 'Login' })
      }
    },
  }
  </script>
  ```

  



#### :thinking: 새로고침해도 로그인을 유지하려면??

> App.vue가 실행됐을 때!
>
> local Storage에 token이 존재한다면 이미 로그인했다고 할 수 있습니다!
>
> 따라서 실행하자마자 token의 유무를 확인해줍시다

##### App.vue

###### script

- **created**

  - App.vue가 실행되자마자, 확인하는 함수를 실행합니다
  - `getItem`으로 `jwt`를 가져오고, 해당 값이 있다면 **isLogin을 true**로 유지합니다

  ```vue
  <script>
  export default {
    ...
    created: function () {
      const token = localStorage.getItem('jwt')
      if (token) {
        this.isLogin = true
      }
    }
  }
  </script>
  ```



#### :loudspeaker: 로그아웃 하고싶어어~!

> 로그인과 반대로! token이 없다면 로그인이 안된것이라고 할 수 있습니다!!!
>
> 따라서 localStorage에서 token을 없애고 isLogin을 false로 바꿔준다면?????
>
> 로그아웃 완료입니다!!!! :champagne:

##### App.vue

###### template

- **@click.native**

  - logout을 위한 router-link에 `click` 이벤트가 발생하면 `logout`을 수행합니다

    ##### :heavy_check_mark: native??? (참고)

    - router-link의 기본은 페이지를 바꾸는 것! (사실 logout은 페이지 변화가 필요없어 적절한 형태는 아니지만... 통일성을 주겠습니다)
    - 원래라면 하위에 연결된 컴포넌트가 있고, 거기서 발생하는 이벤트를 감지합니다
    - 그러나 이처럼 **router-link자체에 이벤트를 발생**시키려면 **native를 붙여줘야**합니다

  ```vue
  <template>
    <div id="app">
      <div id="nav">
        <span v-if="isLogin">
          ...
          <!-- 로그아웃 부분!!!!!! -->
          <router-link @click.native="logout" to="#">Logout</router-link>
        </span>
        <span v-else>
          ...
        </span>
      </div>
      <router-view @login="isLogin=true"/>
    </div>
  </template>
  ```

  

###### script

- **logout**

  - `isLogin`을 `false`로 바꿔줍니다
  - `removeItem`으로 `jwt`를 삭제합니다
  - 로그아웃하면 로그인 페이지로 이동해줍시다

  ```vue
  <script>
  export default {
    ...
    methods: {
      logout () {
        this.isLogin = false
        localStorage.removeItem('jwt')
        this.$router.push({ name: 'Login' })
      },
    },
    ...
  }
  </script>
  ```

  

<br>

<br>

## 2.4. 유저별 Todo

> 유저도 만들어졌겠다
>
> 자신이 작성한 Todo만 볼 수 있게야겠죠??
>
> 또한 글의 수정, 삭제도 동일 유저만 가능해야합니다!!!

### Server

##### todos/views.py

###### :heavy_check_mark: 인증을 위한 둘의 순서!! 매우 중요!!

- `@authentication_classes([JSONWebTokenAuthentication])`

  - JWT 자체를 검증한 인증 여부와 상관없이, JWT가 유효한지 여부만 파악

- `@permission_classes([IsAuthenticated])`

  - 인증되지 않은 상태로 요청 시, '자격 인증 데이터가 제공되지 않았습니다'와 같은 메세지 응답

  ```python
  from rest_framework.decorators import authentication_classes, permission_classes
  from rest_framework.permissions import IsAuthenticated
  from rest_framework_jwt.authentication import JSONWebTokenAuthentication
  
  
  @api_view(['GET', 'POST'])
  @authentication_classes([JSONWebTokenAuthentication])
  @permission_classes([IsAuthenticated])
  def todo_list_create(request):
      #...
      
  @api_view(['PUT', 'DELETE'])
  @authentication_classes([JSONWebTokenAuthentication])
  @permission_classes([IsAuthenticated])
  def todo_update_delete(request, todo_pk):
      #...
  ```

- **todo_list_create**

  - GET : user에 해당하는 todo만 응답으로 보내줘야합니다
    - `todos = request.user.todo`
    - 요청한 user로부터 역참조를 통해 todo를 가져옵니다
  - POST : 저장을 위해 user정보를 전달해줘야합니다!!
    - `serializer.save(user=request.user)`
    - 요청한 유저에대한 정보를 저장합니다

  ```python
  @api_view(['GET', 'POST'])
  @authentication_classes([JSONWebTokenAuthentication])
  @permission_classes([IsAuthenticated])
  def todo_list_create(request):
      if request.method == 'GET':
          # todo = Todo.objects.filter(pk=request.user.pk)도 가능
          ##########
          todos = request.user.todo
          ##########
          serializer = TodoSerializer(todos, many=True)
          return Response(serializer.data)
  
      elif request.method == 'POST':
          serializer = TodoSerializer(data=request.data)
          if serializer.is_valid(raise_exception=True):
              ############
              serializer.save(user=request.user)
              ###########
              return Response(serializer.data, status=status.HTTP_201_CREATED)
  ```

  

### Client

###### :cherries: client는 요청마다 token을 header에 붙여야합니다!

##### TodoList.vue, CreateTodo.vue

###### script

- `JWT ${localStorage.getItem('jwt')}`

  - localStorage에 저장된 token을 가져와 JWT로 만들어줍니다
  - 이를 axios요청 보낼 때마다 header로 붙여줍니다

  ```vue
  <script>
  import axios from 'axios'
  
  export default {
    ...
    methods: {
      getTodos: function () {
        axios({
          method: 'get',
          url: 'http://127.0.0.1:8000/todos/',
          //****************
          headers: {
            Authorization: `JWT ${localStorage.getItem('jwt')}`
          },
          //****************
        })
          ...
      },
      deleteTodo: function (todo) {
        axios({
          method: 'delete',
          url: `http://127.0.0.1:8000/todos/${todo.id}/`,
          //****************
          headers: {
            Authorization: `JWT ${localStorage.getItem('jwt')}`
          },
          //****************
        })
          ...
      },
      updateTodoStatus: function (todo) {
        ...
  
        axios({
          method: 'put',
          url: `http://127.0.0.1:8000/todos/${todo.id}/`,
          data: todoItem,
          //****************
          headers: {
            Authorization: `JWT ${localStorage.getItem('jwt')}`
          },
          //****************
        })
          ...
        },
      },
    ...
  }
  </script>
  ```

  ```vue
  <script>
  import axios from'axios'
  
  export default {
    ...
    methods: {
      createTodo: function () {
        ...
  
        if (todoItem.title) {
          axios({
            method: 'post',
            url: 'http://127.0.0.1:8000/todos/',
            data: todoItem,
            //*********
            headers: {
            Authorization: `JWT ${localStorage.getItem('jwt')}`
            },
            //*********
          })
            ...
          }
      },
    }
  }
  </script>
  ```



<br>

## 2.5. axios 중복 제거하기

> axios에 같은 내용이 너무 많이 쓰입니다...
>
> 좀 줄여볼까요?

### axios creating an instance

- axios instance를 만들어쓸 수 있는 방법!!!!
- import해서 사용하는 것 알아보자

#### baseAxios.js

- src 안에 만들기

- axios에서 중복되는 부분을 적어줍니다

  ```js
  import axios from 'axios'
  
  export default function baseAxios() {
    return axios.create({
      baseURL: 'http://127.0.0.1:8000',
      headers: {
        Authorization: `JWT ${localStorage.getItem('jwt')}`
      },
    })
  }
  ```

  

#### TodoList.vue

- baseAxios를 import해서 수행한 뒤, 뒷부분에 추가적인 내용을 덧붙여줍니다

  ```vue
  <script>
  import baseAxios from '@/baseAxios.js'
  
  export default {
    ...
    methods: {
      getTodos: function () {
        //이렇게 붙여줄 수 있습니다!!!
        baseAxios()({
          url: 'todos/',
          methods: 'GET',
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
      },
    ...
  }
  </script>
  ```

  
