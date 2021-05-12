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

- 제목
- 검색창
- 동영상 재생부분 (영상 디테일)
  - 영상
  - 제목, 설명
  - 구분갈 수 있게 네모칸
- 영상 리스트
  - 왼쪽에 배치
  - 영상 선택하면 커서, 그림자
  - 선택한 경우 네모칸으로 선택표시?? (클랫 없애고 만들고 하면 될 듯)

### Component 구성

- App.vue
- SearchBar.vue
- VideoDetail.vue
- VideoList.vue
- VideoListItem.vue

<br>

<br>

# 2. 구현

### 기본 설정

- 프로젝트 생성

```shell
$ vue create se-youtube
$ code se-youtube
```

