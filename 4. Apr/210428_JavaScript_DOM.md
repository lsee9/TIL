###### 210428_wed

##### JavaScript

<hr>


###### 오늘의 목차 :rainbow:

### JavaScript

- Intro to JavaScript
- DOM
- Event Listener

##### :running: 4일 만에 JS 끝내기!!! 

<hr>
<br>

# 1. JavaScript Intro

### why learn???



## 1.1 history



<br>

<br>

##### 본격 JS 시작!!

# 2. DOM

> JS : 브라우저를 조작하는 언어

#### 브라우저에서 할 수 있는 일

- **DOM 조작**
  - 문서(HTML) 조작 (Document조작)

- **BOM 조작**
  - Browser 조작
  - navigator, screen, location, frames, history, XHR
- **JavaScript Core (EXMAScript)**
  - JS 언어로서 동작하는 것
  - Data Structure(Object, Array), Conditional Expression, Iteration

##### 하나씩 알아보자

<br>

### DOM (Documet Object Model)

> 문서를 조작하는 행위!!!

- 브라우저의 윈도우객체 안에 Document  == 우리가 보는 웹페이지 화면
- 개발자도구 - console에서 명령어를 이용해 브라우저를 수정할 수 있음!

```javascript
window.document.title  //해당 document에서 title을 출력해줌
```



### BOM (Browser Object Model)

> 문서가 아닌 브라우저 자체를 조작하는 것!







<br>

<br>

# 3. DOM 조작

> DOM을 조작해봅시다!!

#### 개념

- Document는 문서 한 장(HTML)에 해당하고, 이를 조작한다

- **DOM 조작 순서** :cherries: :cherries: :cherries: 2가지만 기억!!
  1. 선택 (select)
  2. 변경 (manupulation)

#### Document 위치



#### DOM 관련 객체 상속 구조



<br>

### 잠깐!!! CSS Selector... 기억하니..? :scream:

> selector를 사용해서 선택, 변경 해줘야하기때문에!!! 이를 정리하고 갑시다

- **class** selector
  - `마침표(.)`로 시작!
  - .class_name
- **id** selector
  - `#`문자로 시작, 문서당 한 번만 사용할 수 있다
  - #id_name
- **자손** 결합자
  - selectorA `공백` selectorB
  - selectorA의 후손 요소(level n, 아래에 있는 모든) 중 selectorB와 일치하는 요소 선택!!
- **자식** 결합자
  - selectorA `>` selectorB
  - selectorA의 모든 자식 요소(level 1, 바로 아래) 중 selectorB와 일치하는 요소 택
- **tag**
  - h1, div등 HTML tag

<br>

## 3.1 DOM 선택

### 선택 관련 메서드

> 선택을 어떻게 할까요??
>
> 여러가지가 있지만 체크해둔 `2가지`만 알아도 될 듯합니다!!!
>
> 가장 많이 쓰고, `모든 경우(id, class, tag)에 사용 가능`!! 구체적이고 유연한 선택이 가능합니다

- Document.**querySelector()** :heavy_check_mark:
  - 앞의 문서에서 제공한 선택자(()안에 인자로 전달)와 일치하는 `element 하나 선택`
  - 제공한 CSS selector를 `만족하는 첫번째 element 객체`를 반환 (없다면 null)
- Document.**querySelectorAll()** :heavy_check_mark:
  - 앞의 문서에서 제공한 선택자와 일치하는 `여러 element를 선택`
  - 매칭할 하나 이상의 셀렉터를 포함하는 유효한 CSS selector를 인자(문자열)로 받음(예) 'div > li')
  - 지정된 selector에 일치하는 `NodeList`를 반환
    - 다중 객체를 NodeList라는 이름의 객체로 반환
    - objects.all()해서 QuerySet 받는것과 유사!
- getElementById() => HTML id를 사용해서 선택
- getElementsByTagName() => tag 사용해서 선택
- getElementsByClassName() => class 사용해서 선택

<br>

### 선택 메서드별 반환 타입

- **단일 element**
  - getElementById()
    - id는 하나만 존재하기때문!
  - querySelector()
    - 여러개인 경우 가장 앞의 것 출력

- **HTMLCollection**
  - getElementsByTagName()
  - getElementsByClassName()
- **NodeList**
  - querySelectorAll()

<br>

### HTMLCollection & NodeList

> 여러 element를 반환하는데... type이 다르다구..???

- 둘 다 배열과 같이 각 항목을 접근하기 위한 `인덱스를 제공`(유사 배열)
- **HTMLCollection**
  - name, id, 인덱스 속성으로 각 항목들에 접근 가능
- **NodeList**
  - `인덱스 번호로만` 각 항목들에 접근 가능
  - 단, HTMLCollection과 달리 배열에서 사용하는 for each 함수 및 `다양한 메서드 사용 가능`(좀 더 유연하게 사용 가능)

- 둘 다 **Live Collection**으로 DOM의 변경 사항을 실시간으로 반영!!!
- BUT **querySelectorAll()**에 의해 반환되는 NodeList는 **Static Collection**

###### :see_no_evil: 새로운 개념이 계속.... 그치만 알아야합니다...ㅜ

<br>

### Collection

- **Live Collection**
  - 문서가 바뀔 때 `실시간으로 업데이트`
  - DOM의 변경사항을 실시간으로 collection에 반영
  - 예) HTMLCollection, NodeList
- **Static Collection (non-live)**
  - DOM이 변경되어도 `collection 내용에는 영향 X`
  - querySelectorAll()의 반환 NodeList만 static







