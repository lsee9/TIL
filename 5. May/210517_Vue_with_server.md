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
>
>  [CORD MDN](https://developer.mozilla.org/ko/docs/Web/HTTP/CORS) :point_left:한번쯤은 읽기 

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

<br>

#### Cross-Origin Resource Sharing Policy (CORS Policy)

###### <=> SOP

- 교차 출처 리소스 (자원) **공유 정책**
- 다른 출처(origin)에서 온 리소스를 공유하는 것에 대한 정책

<br>

#### 교차 출처 접근 허용하기

- CORS 사용해 교차 출처 접근 허용
- CORS는 HTTP의 일부로, **어떤 호스트**에서 자신의 컨텐츠를 불러갈 수 있는지 **`서버에 지정`**할 수 있는 방법
- 응답 + CORS header => 브라우저(클라이언트) 에게 보냄

<br>

### 2.2.1. Why CORS?

> 왜 써야만 할까??/

1. 브라우저 & 웹 애플리케이션 보호

   - 악의적인 사이트(=같은 출처 아닌) 에서 데이터를 가져오지 않도록 사전 차단
   - 응답으로 받은 자원에 대한 **최소한의 검증**
   - 서버는 정상적으로 응답하지만 **브라우저에서 차단**

2. Server의 자원 관리

   - 누가 해당 리소스에 접근 할 수 있는지 관리 가능 

     (서버가 header를 붙이므로, 이를 파악하고 관리할 수 있다)

<br>

### 2.2.2. How CORS?

> 어떻게 써야하는거니...

- CORS 표준에 의해 추가된 **HTTP Header**를 통해 이를 통제

- CORS HTTP 응답 헤더 예시 

  ###### 목적과 상황에 따라 다양! 대표적인 4가지를 보자

  - Access-Control-Allow-Origin ​ ​ ​ ​ ​ ​ ​ :heavy_check_mark: 출처에 대한 것 사용해보자
  - Access-Control-Allow-Credentials
  - Access-Control-Allow-Headers
  - Access-Control-Allow-Methods

<br>

#### Access-Control-Allow-Origin 응답 헤더

- 이 **응답이 주어진 출처**(origin)으로 부터 **요청 코드와 공유될 수 있는 지**를 나태낸다

- 예
  - `Access-Control-Allow-Origin: *`
  - 브라우저 리소스에 접근하는 임의의 origin으로부터 **요청을 허용한다고 알리는 응답에 포함**
  - `*` : 모든 도메인에서 접근할 수 있음을 의미
  - 특정 origin 하나를 명시 할 수 있다@@

<br>

### 2.2.3. CORS 시나리오 예시

> 우리의 시나리오!
>
> 이러이러한 흐름에 따라 CORS를 사용해봅시다

1. Vue.js에서 A 서버로 요청
2. A 서버는 Access-Control-Allow-Origin에 **특정한 origin을 포함시켜 응답**
   - 서버는 CORS Policy와 직접적인 연관이 없다!! 그저 요청에 응답할 뿐
3. **브라우저**는 응답에 Access-Control-Allow-Origin을 확인 후 **허용 여부**를 결정한다!
4. 프레워크 별로 이를 지원하는 라이브러리가 존재
   - django는 **`django-cors-headers`** 라이브러리를 통해 응답 헤더 및 추가 설정 가능!

#### 왜 CORS 처리 해야하나??

- Vue.js에서 요청을 하게되는데, django과 서로 출처가 다르다!
  - Vue.js : port 8080
  - django : port 8000
- 서로 다른 출처로, SOP에 의해 차단된다
- 이를 해결하기위해서 django에서 `응답 + CORS Header 형태`로 보내야 한다
- 그러면 브라우저에서 이를 검토한 뒤, 응답을 전달한다

