###### 210517_mon

##### Vue with server

<hr>




###### 오늘의 주제 :shell:

### Vue with server

- Server & Client
- CORS
- Authentication & Authorization
- JWT

##### :sailboat: Server와 Client... 드디어 함께 써보자!!!

<hr>




<br>

# 1. Server & Client

> 서버와 클라이언트!!! 같이 사용해봅시다

### Server

- 클라이언트에게 '정보', '서비스'를 제공하는 컴퓨터 시스템

- 정보 & 서비스

  - Django를 통해 응답한 template
  - DRF를 통해 응답한 JSON

  ###### 이렇게 다양한 것이 대상이 될 수 있다!

### Client

- 서버에게 그 서버가 맞는(서버가 제공하는) **`서비스 요청`**
- 서비스 요청을 위해 필요한 인자를 **`서버가 원하는 방식에 맞게 제공`**
- 서버로부터 반환되는 응답을 **`사용자에게 적절한 방식으로 표현`**하는 기능

###### 이러한 기능을 가진 시스템이 Client입니다!

### 정리

- **Server** : **정보 제공** (Django REST Framework)
  - DB와 통신하며 `데이터를 CRUD`
  - 요청을 보낸 Client에게 이러한 `정보를 응답`

- **Client** : **정보 요청 & 표현**
  - Server에게 정보(데이터) `요청`
  - 응답 받은 정보를 잘 `가공`하여 화면에 보여줌

<br>
<br>

# 2. CORS

