###### 210511_tue

##### Vue

<hr>




###### 오늘의 주제 :vertical_traffic_light:

### Create My YouTube App

- Component를 나눠 기능별로 화면을 구성하자
- YouTube API로 데이터를!!!
- UX를 고려한 추가적인 스타일링??

##### :car: Vue 배운걸 활용해보자!

<hr>



<br>

# 1. 구성

### 화면 구성

<img src="210511_Create_My_YouTube_App.assets/image-20210512232832591.png" alt="image-20210512232832591" style="zoom:33%;" />

- 제목
- 검색창
- 동영상 재생부분 (영상 디테일)
  - 영상
  - 제목, 설명
  - 구분갈 수 있게 네모칸
- 영상 리스트
  - 왼쪽에 배치
  - 영상 선택하면 커서, 그림자
  - 선택한 경우 네모칸으로 선택표시?? (클래스 없애고 만들고 하면 될 듯?? 이건 생각좀..)

### Component 구성

> 각각 파일을 만들어둔 뒤, 에러가 발생하지 않도록 template에 div로 뭐라도 적어두자!

- **App.vue**
  - 전체 화면
- **SearchBar.vue**
  - 검색창
- **VideoDetail.vue**
  - 영상 재생
- **VideoList.vue**
  - 검색한 영상 리스트
- **VideoListItem.vue**
  - 영상 리스트를 이루는 요소 하나하나

<br>

<br>

# 2. 구현

### 프로젝트 생성

```shell
$ vue create se-youtube
$ code se-youtube
```

:heavy_plus_sign: **절대경로 자동완성** (필요한 경우 추가)

- 프로젝트 최상단에 `jsconfig.json`생성
- 아래 코드 붙여넣기 (`@/`를 `./src/`로 지정한다는 의미)

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

<br>

### component 등록

- **App.vue**
  - SearchBar, VideoDetail, VideoList

```vue
<template>
  <div id="app">
    <h1>My YouTube</h1>
    <!-- 3. 사용 -->
    <search-bar />
    <video-detail />
    <video-list />
  </div>
</template>

<script>
// 1. 불러오기
import SearchBar from './components/SearchBar.vue'
import VideoDetail from './components/VideoDetail.vue'
import VideoList from './components/VideoList.vue'

export default {
  name: 'App',
  // 2. 등록
  components: {
    SearchBar,
    VideoDetail,
    VideoList,
  }
}
</script>
```



- **VideoList**
  - VideoListItem

```vue
<template>
  <div>
    <video-list-item />
  </div>
</template>

<script>
import VideoListItem from './VideoListItem.vue'

export default {
  components: {
    VideoListItem,
  },
}
</script>
```



<br>

## 2.1. Template 구성하기

> 페이지를 만들때는 눈에 보이는 화면 (template) 구성을 먼저 작성하는 걸 추천합니다!!!
>
> 뭐가 보이는지를 알아야 어떤 데이터를 추가할 지 명확히 알 수 있겠죠??

### SearchBar

- 검색창에 해당하는 input을 추가합니다

```vue
<template>
  <div>
    <input type="text">
  </div>
</template>
```



### VideoDetail

- 영상, 그 아래 영상 제목, 제목아래 세부 설명

```vue
<template>
  <div>
    <h2>영상 자리</h2>
    <div>
      <h3>video title</h3>
      <p>video description</p>
    </div>
  </div>
</template>
```



### VideoList

- ul태그 아래 여러개의 ListItem이 존재합니다
- 이를 위해 v-for로 각각을 받아줍니다!
- 동일한 ListItem이 여러개면 구분이 어려우므로, 구분할 수 있도록 **반드시 key**를 잘아줍니다(unique한 값!!!)

```vue
<template>
  <div>
    <ul>
      <video-list-item v-for="video in ['v1', 'v2', 'v3']" :key="video"/>
    </ul>
  </div>
</template>
```



### VideoListItem

- 각각을 li 태그로 구성합니다
- 영상이 썸네일(thumbnail)과 제목을 보여줍니다

```vue
<template>
  <div>
    <li>
      <img src="" alt="video-thumnail" width="100" height="100">
      <span>title</span>
    </li>
  </div>
</template>
```

<br>

<br>

## 2.2. 데이터 가져오기

> 어디에 어떤 데이터가 필요한지 명확히 정해야 적절한 위치에 선언할 수 있습니다!
>
> :fire: **두개 이상의 component에서 사용하는 데이터라면??**:fire:
>
> - 두 component의 :family_woman_girl_girl:**공통 부모**에 선언합니다 
>
>   - 형제라면 공통부모에, 부모-자식이라면 부모의 부모에!!!
>
>   (두 component를 아우르는 곳에 넣기)

### 구성

<img src="210511_Create_My_YouTube_App.assets/image-20210512234143648.png" alt="image-20210512234143648" style="zoom:33%;" />

- SearchBar에서 입력된 searchKeyword로 YouTube API에 데이터 요청
- API로부터 검색 결과로 videos 받음
- videos를 VideoList에 뿌려줌
- 이 중 선택된 selectedVideo를 VideoDetail에 뿌려줌

#### 데이터의 선언 위치는??? :thinking:

- **videos** & **selectedVideo**
  - searchKeyword로부터 생성되어 VideoList에 뿌려진다
  - videos의 일부인 selectedVideo는 VideoDetail에 사용된다
  - 여러 component에서 필요한 데이터 이므로, 공통 부모인 **App**에 선언한다
- **searchKeyword**
  - SearchBar에서 사용되지만, API로 전달되어 videos를 변경하는 역할을 한다
  - App에 선언된 videos는 App에서만 변경할 수 있으므로, App에서 searchKeyword를 활용해 API에 요청을 보낸다
  - 따라서 **App**에 선언된다

<br>

