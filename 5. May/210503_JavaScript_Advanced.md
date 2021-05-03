###### 210503_mon

##### JavaScript Advanced

<hr>



###### 오늘의 목차 :doughnut:

### JavaScript Advanced

- AJAX
- Asynchronous JavaScript
  - Callback Function
  - Promise
  - Axios

##### :coffee: 어려워... 근데 신기해!!!

<hr>
<br>

# 1. AJAX

### What is AJAX???

> Asynchronous JavaScript And XML (비동기식 JS와 XML)

- **서버와 통신**하기 위해 **XMLHttpRequest** 객체를 활용
  - XMLHttpRequest는 동기식과 비동기식 통신을 모두 지원 (보통 비동기식에 사용하긴 함!)
- 페이지 전체를 reload를 하지 않고서도 수행되는 '**비동기**성'이 특징 :star:
  - 사용자의 `event가 있으면` 전체 페이지가 아닌 `일부분만을 업데이트`
- AJAX의 X가 XML을 의미하긴 하지만, 요즘은 더 가벼운 용량과 JavaScript의 일부라는 장점 때문에 **JSON을 더 많이 사용**

###### 따라서 우리도 JSON을 사용합니다!!

<br>

### AJAX 배경





<br>

### XMLHttpRequest object

- **서버와 상호작용 하기 위해 사용**, 전체 페이지의 새로고침 없이 URL로부터 데이터를 받아올 수 있다!
- 사용자가 하는 것을 방해하지 않으면서 페이지의 일부를 업데이트할 수 있도록 해준다
- 주로 AJAX 프로그래밍에 사용
- XML뿐만 아니라 `모든 종류의 데이터`를 받아오는데 사용 가능!
- 생성자 (class 사용)
  - XMLHttpRequest()

#### 예시

```javascript
const request = new XMLHttpRequest()  //인스턴스 생성
const URL = 'https://~~/~/~/'

request.open('GET', URL)  		//GET으로 URL에 요청 보낼 준비
request.send()  		  		//요청 보냄

const todo = request.response  	//응답 받음
console.log(todo)
```

<br>

<br>

# 2. Asynchronous JS

## 2.1 동기(Synchronous)와 비동기(Asynchronous)

### 동기식

- 순차적, 직렬적 태스크 수행
- 요청을 보낸 후 `응답을 받아야`만 다음 동작이 이뤄짐 (blocking)

그림넣자

### 비동기식

- 병렬적 태스크 수행
- 요청을 보낸 후 `응답을 기다리지 않고` 다음 동작이 이뤄짐 (non-blocking)
  - 즉, 요청을 보내놓고 다음 태스크로 진행

그림넣자

<br>

### :thinking:왜 비동기(Asynchronous)를 사용할까?

- :cherry_blossom:**사용자 경험** :cherry_blossom: 을 증대시키기 위해서!!
  - 예를 들어... 데이터를 구종하고 실행되는 앱에서 데이터가 굉장히 크다고 생각해보자
  - 동기식이라면???
    - 데이터가 모두 로드된 뒤 앱이 실행된다
    - 실행되는 동안 사용자는..? 아무것도 할 수 없다!! :scream:
    - 얼마나 걸릴지 모르는 로딩을 기다려야한다
    - 즉, 앱이 멈춘것:hourglass_flowing_sand: 처럼 보인다 
    - 이렇게 되면 사용자들은 앱을 떠나게 될 것입니다...ㅜ
  - 이처럼 동기식 요청은 코드 실행을 차단하여 화명이 멈추고 응답하지 않는 사용자 경험을 만듭니다
  - 때무넹 많은 웹 API 기능은 현재 `비동기 코드를 사용하여 실행` 됩니다

#### 동기와 비동기 예시



#### 비교





<br>

## 2.2 Threads

### Threads

- 프로그램이 작업을 완료하는데 사용할 수 있는 단일 프로세스
- 각 thread(스레드)는 **한 번에 하나의 작업만 수행**할 수 있다
- 예) Task A => Task B => Task C
  - 다음 작업을 시작하려면 반드시 앞의 작업이 완료되어야 함
  - 컴퓨터 CPU는 여러 코어를 가지고 있기 때문에 한번에 여러가지 일을 처리할 수 있음
  - 여러 코어를 가진다 => 일을 여러개 처리해서 빠르다 (그만큼 비싸다)

### "JS는 single threaded 이다"

- 컴퓨터가 여러 개의 CPU를 가지고 있어도 **main thread**라 불리는 `단일 스레드에서만 작업 수행`
- 즉, 이벤트를 처리하는 **Call Stack**이 하나인 언어라는 의미!
- 이 문제를 해결하기 위해??!?!?! (즉, 비동기식 JS가 되기 위해)
  - 즉시 처리하지 못하는 이벤트들을 **다른 곳 (`Web API`)**으로 보내서 처리(하고 다음 코드 진행)
  - 처리된 이벤트들은 처리된 순서대로 **대기실(`Task queue`)**에 줄을 세워 놓음
  - Call Stack이 비면 **담당자(`Event Loop`)**가 대기 줄에서 가장 오래된 (제일 앞의, queue에 넣은 순) 이벤트를 Call Stack으로 보냄

<br>

### Concurrency model

> single Thread인 것을 보완하기 위해 채택!!

- Event Loop를 기반으로 하는 동시성 모델(Concurrency model) => 비동기 JS
- 구성
  1. Call Stack
  2. Web API (Browser API)
  3. Task Queue (Event Queue, Message Queue)
  4. Event Loop