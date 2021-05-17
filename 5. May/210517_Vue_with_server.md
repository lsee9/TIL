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

# 2. CORS :croissant:

> Cross Origin Resource Sharing???
>
> 이것을 알아보기에 앞서... 
>
> CORS와 반대되는 **http가 가진 기본 정책**을 알아봅시다!

## 2.1. Same-origin policy (SOP)

> 동일 출처(origin) 정책

- 특정 출처(origin)에서 불러온 문서나 스크립트가 **다른 출처**에서 가져온 리소스와 **상호작용 하는 것을 제한**하는 **보안** 방식

- 잠재적으로 해로울 수 있는 문서를 분리함으로써 공격받을 수 있는 경로를 줄임

  ###### 나와 다른 경로는 모두 위험성이 있다!

- 정의

  - 두 URL의 `Protocol`, `Post`, `Host`가 **모두 같아야** 동일한 출처라고 할 수 있다

    ###### 하나라도 다르면 다른 출처(교차 출처)

### Origin의 정의

- 두 URL의 `Protocol`, `Post`, `Host`가 모두 같아야 동일한 출처

- URL `http://store.company.com/dir/page.html`의 출처 비교

|                        URL                        | 결과 |                 이유                 |
| :-----------------------------------------------: | :--: | :----------------------------------: |
|    `http://store.company.com/dir2/other.html`     | 성공 |             경로만 다름              |
| `http://store.company.com/dir/inner/another.html` | 성공 |             경로만 다름              |
|      `https://store.company.com/secure.html`      | 실패 |       프로토콜 다름(https://)        |
|    `http://store.company.com:81/dir/etc.html`     | 실패 | 포트 다름<br>(http://는 80이 기본값) |
|     `http://news.company.com/dir/other.html`      | 실패 |  호스트 다름<br>(news.company.com)   |

#### Same origin

<img src="210517_Vue_with_server.assets/image-20210517220146626.png" alt="image-20210517220146626" style="zoom:33%;" />

<br>

## 2.2. Cross-Origin Resource Sharing (CORS)

> 교차 출처 리소스(자원) 공유

- **`추가 Http header를 사용`**하여, 특정 출처에서 실행중인 웹 애플리케이션이 **다른 출처의 자원에 접근 할 수 있는 권한을 부여하도록 `브라우저에 알려주는 체제`**

- 리소스가 자신의 출처 (Domain, Protocal, Port)와 **다를 때 교차 출처 HTTP 요청**을 실행

- 보안 상의 이유로 브라우저는 교차 출처 HTTP 요청을 제한 (SOP)

  예) 

  - XMLHttpResponse

  - 웹 폰트
  - 그 외

- 다른 출처의 리소스를 불러오려면 그 출처에서 `올바른 CORS header를 포함한 응답을 반환` 해야한다
  - 응답 + header
  - **브라우저**가 이를 확인하고 **승인**하는 형태