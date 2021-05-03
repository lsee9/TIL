###### 210429_thu

##### ECMAScript6

<hr>



###### 오늘의 목차 :mushroom:

### 함수

- 함수활용법
- 선언식 vs 표현식
- Arrow Function

##### :evergreen_tree: 종류 여러가지인거...신기해!!

<hr>
<br>

# 1. 함수 (Functions)

## 1.1 함수 활용법

### 함수 in JavaScript

- 참조 타입 중 하나, <u>function 타입</u>에 속함
- 주로 함수를 정의하는 방법 2가지
  - 함수 선언식 (function declaration)
  - 함수 표현식(function expression)
- JavaScript에서의 함수는 **일급 객체(First-class citizens)**에 해당
  - 일급 객체의 조건 3가지
    - `변수에 할당` 가능
    - 함수의 `매개변수로 전달` 가능
    - 함수의 `반환 값으로 사용` 가능
  - 예시) django => path('index/', views.index)에서 views.index가 함수 

<br>

### 함수 선언식 (function statement, declaration)

- 함수의 **이름과 함께 정의**하는 방식
- 3가지 부분으로 구성
  - `함수의 이름(name)`
  - `매개변수 (args)`
  - `몸통 (중괄호 내부)`

```javascript
function name(args) {
  //code
}
```

