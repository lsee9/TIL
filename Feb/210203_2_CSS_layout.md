###### 210203_wed



이제부터 배울 부분이 __오늘의 핵심__ 입니다!!!

집중집중!! :dizzy_face:



# CSS layout

- CSS page layout techniques
  - DIsplay
  - Position
  - Float
  - `Flexbox`
  - Grid



## 2. CSS Flexible Box Layout

> Flexbox의 실제 이름은 좀 더 길져..?
>
> 여기서는 헷갈리지 않도록 핵심을 꼭 외우고 있어야합니다!!!

- 요고 간 공간 배분과 정렬 기능을 위한 강력한 __1차원(단방향) 레이아웃__
- 상, 하, 좌, 우 중 한방향으로만 정렬 합니다!

### :star: 두가지 핵심 - 요소, 축

- 요소
  - Flex Container (부모 요소)
  - Flex Item (자식 요소)
- 축
  - main axis (메인축)
  - cross axis (교차축)

#### 그림을 기억하세요!

<img src="210203_2_CSS_layout.assets/image-20210204014901765.png" alt="image-20210204014901765" style="zoom:70%;" />

- Container를 정의하면 Flexbox가 만들어지며, 그 안의 요소는 item이 되어 정렬됩니다.

  #### :heavy_check_mark: 즉, Container를 통해 내부 요소의 순서나 축을 조절합니다!!!

- 기본 default값
  - 그림의 main, cross 축 방향
  - row 정렬(left 부터 쌓여감, main 기준 정렬)