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



### 초기 설정

> 파일을 받아온 뒤에는 반드시 설정을 먼저 해주자!!

#### Server

- 가상환경 설정

```shell
$ python -m venv venv
$ source venv/Scripts/activate
```



- 필요한 프로그램 설치

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

