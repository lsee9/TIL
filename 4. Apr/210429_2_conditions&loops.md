###### 210429_thu

##### ECMAScript6

<hr>




###### 오늘의 목차 :musical_note:

### 조건문과 반복문

- 조건문
- 반복문

##### :saxophone: 비슷한 듯 아닌듯...

<hr>
<br>

# 1. 조건문과 반복문

## 1.1 조건문

### 종류와 특징

- **if statement**
  - 조건 표현식의 결과값을 Boolean 타입으로 변환 후 참/거짓을 판단
- **switch statement**
  - 조건 표현식의 결과값이 어느 값(case)에 해당하는지 판별



### if statement

- if, else if, else
  - 조건은 **소괄호(condition)**안에 작성
  - 실행할 코드는 **중괄호{}** 안에 작성
  - 블록 스코프 생성



### switch statement

- switch
  - **표현식(expression)의 결과값**을 이용한 조건문
  - **표현식의 결과값**과 **case문의 오른쪽 값**을 비교
  - break문이 없는 경우 break문을 만나거나 default문을 실행할 때 까지 다음 조건문 실행





## 1.2 반복문

### 종류

- while
- for
- for...in
  - 주로 **객체(object)의 속성들을 순회**할 때 사용
- for...of
  - **반복 가능한(iterable) 객체를 순회**하며 값을 꺼낼 때 사용 (Array, Map, Set, String)