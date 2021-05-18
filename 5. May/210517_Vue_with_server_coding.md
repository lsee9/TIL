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

## 1.1. TodoList

> Todo가 직접적으로 보이는 부분입니다
>
> 또한 업데이트와 삭제가 이뤄집니다

### 구성하기

0. 전체적인 그림은 이미 그려져 있습니다
1. **markup 작성**
   - todo가 보여질 부분
   - todo를 불러오는데 사용할 버튼

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

##### CORS를 위한 설정 :cherries:

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

### Server

- **models.py**

