###### 210513_thu

##### Vue & Vuex

<hr>



###### 오늘의 주제 :gift:

### Create ToDo

- Vue CLI로 ToDo 페이지를 만들어보자
- Vuex를 적용해서 수정해보자

##### :birthday: Vue로 구성할 수 있는게 일단 중요!!! 나중을 위해 Vuex도 익혀보자

<hr>



<br>

# 1. 구성

### 화면 구성

- 제목
- 입력창
- 입력한 todolist
- 각 todolist
  - 삭제버튼
  - 누르면 완료표시(취소선)

### Component 구성

> 가장 먼저 구조를 나누고, 필요한 component를 구성합시다

- **App.vue**
  - 전체 화면
- **TodoForm**
  - todo 입력하는 form 부분
- **TodoList**
  - 모든 todo 리스트
- **TodoListItem**
  - 각 todo요소

<br>

# 2. 구현

> 우선은 나중을 위해서 vuex도 만들어두고 갑시다!!!

### 프로젝트 생성

```shell
$ vue create my-todo
$ code my-todo/
```

- vuex 추가

```shell
$ vue add vuex
```

- 절대경로 자동완성 
  - 프로젝트 최상단에 `jsconfig.json`생성

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

- 서버 켜기

```shell
$ npm run serve
```

<br>

### component 등록

- **App.vue**
  - TodoForm, TodoList

```vue
<template>
  <div id="app">
    <h2>My Todo List</h2>
    <TodoForm />
    <TodoList />
  </div>
</template>

<script>
import TodoForm from '@/components/TodoForm.vue'
import TodoList from '@/components/TodoList.vue'

export default {
  name: 'App',
  components: {
    TodoForm,
    TodoList,
  },
}
</script>
```



- **TodoList**
  - TodoListItem

```vue
<template>
  <div>
    <TodoListItem />
  </div>
</template>

<script>
import TodoListItem from '@/components/TodoListItem.vue'

export default {
  components: {
    TodoListItem,
  },
}
</script>
```



<br>

##### 우선 vue로 구현해볼까요~ :musical_note:

## 2.1. Template 구성하기

> 눈에 보이는 부분!! 뭐가 필요한지 파악하고 추가해봅시다

### TodoForm

- Form 태그를 사용합니다
- input, button도 추가합니다

```vue
<template>
  <div>
    <form>
      <input type="text">
      <button>+</button>
    </form>
  </div>
</template>
```



### TodoList

- ul태그 아래 TodoListItem이 존재합니다
- 여러번 가져와야하므로 v-for를 사용합니다! (일단은 임으로 여러 값을 넣어봅시다)

```vue
<template>
  <div>
    <ul>
      <TodoListItem 
        v-for="todo in ['td1', 'td2', 'td3']" 
        :key="todo"
      />
    </ul>
  </div>
</template>
```



### TodoListItem

- 각각이 li 태그로 되어있습니다
- Todo가 있고, 그 옆에 취소 버튼을 넣어줍니다

```vue
<template>
  <div>
    <li>
      <span>TodoItem</span>
      <button>-</button>
    </li>
  </div>
</template>
```

<br>

<br>

## 2.2. Data 구성하기

> 무슨 데이터가 필요할까요?? 어떤 데이터를 어디에 정의해볼까요??

### 구성

- **todos**
  - 작성한 todo list에 해당하는 배열!!
  - `TodoForm`에서 작성한 뒤, `TodoList`에 뿌려줘야한다!
  - 따라서 **App**에 선언하자

<br>

#### App.vue

- todos 선언!
- 함수의 반환값으로 지정하자!!
- 초기값도 넣어주자

```vue
<script>
...
export default {
  ...
  data () {
    return {
      todos: [
        {
          id: 1,
          content: '필기하기',
          completed: false,
        },
        {
          id: 2,
          content: '정리하기',
          completed: false,
        },
      ],
    }
  },
}
</script>
```

<br>

### 2.2.1 todos :page_with_curl:



