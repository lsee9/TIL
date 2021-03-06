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
  - 주로 특정 변수의 값에 따라 조건을 분기할 때 활용 (조건이 많아질  경우 if보다 가독성이 나을 수 있음)

<br>

### if statement

- if, else if, else
  - 조건은 **소괄호(condition)**안에 작성
  - 실행할 코드는 **중괄호{}** 안에 작성
  - 블록 스코프 생성

```js
if (condition) {
  //code
} else if (condition) {
  //code
} else {
  //code
}
```

<br>

### switch statement

- switch
  - **표현식(expression)의 결과값**을 이용한 조건문
  - **표현식의 결과값**과 **case문의 오른쪽 값**을 비교
  - break, default : [선택적]으로 사용 가능
  - break문이 없는 경우 break문을 만나거나 default문을 실행할 때 까지 다음 조건문 실행
  - 블록 스코프 생성

```js
switch(expression) {
    case 'first value': {
      //code
      [break]
    }
    case 'second value': {
      //code
      [break]
    }
    [default: {		//모든 경우에 속하지 않는 경우
      //code
    }]
}
```

<br>

<br>

## 1.2 반복문

### 종류

- while
- for
- for...in
  - 주로 **객체(object)의 속성들을 순회**할 때 사용
  - 배열 순회도 가능하나 `인덱스 순으로 순회한다는 보장이 없`으모로 권장하지 않음
- for...of
  - **반복 가능한(iterable) 객체를 순회**하며 값을 꺼낼 때 사용 (Array, Map, Set, String)
    - 반복 가능한 객체 종류 : Array, Map, Set, Srting 등

<br>

### while

- 조건문이 **참(true)인 동안 반복** 시행

- 조건(condition)은 소괄호안에 작성
- 실행할 코드는 중괄호({})안에 작성
- 블록 스코프 생성

```js
while (condition) {
  //code
}
```

#### 예시

```js
let evenNumber = 0
while (evenNumber < 6) {
  console.log(evenNumber)  //0, 2, 4
  evenNumber += 2
}
```

<br>

### for

- 세미콜론(;)으로 구분되는 세 부분을 구성
  - `initialization`
    - 최초 반복문 진입시 1회만 실행되는 부분
  - `condition`
    - 매 반복 시행 전 평가되는 부분
  - `expression`
    - 매 반복 시행 이후 평가되는 부분
- 블록 스코프 생성

```javascript
for (initialiation; condition; expression) {
    //code
}
```

#### 예시

1. initialization 수행

2. 조건확인

3. 코드 블록 실행 후 expression 수행
4. 2, 3 반복

```js
for (let oddNum = 1; oddNum < 5; oddNum += 2) {
    console.log(oddNum)  //1, 3
}
```

<br>

### for... in

- **객체(object)의 속성들을 순회**할 때 사용
  - object는 순서가 필요없이 하나하나의 속성을 표현할 때 쓰는 것!
- 배열도 순회 가능하지만 권장하지 않음
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

```javascript
for (variable in object) {
    //code
}
```

#### 예시

- 객체의 value는 점(.) 또는 대괄호([]) 표기법을 이용하여 key값을 통해 접근 가능!
  - `obj.key`, `obj[key]`
- 변수에 key를 넣어 접근하는 경우!! 대괄호만 사용할 수 있습니다
  - 점(.)으로 접근하면 변수를 key자체로 인식합니다

```javascript
const personInfo = {
    name: 'Jieun-Lee',
    age: 29,
    phone: '010-1234-5678'
}

for (let person in personInfo) {
    console.log(`${person}: ${personInfo[person]}`)
}

// 출력
//name: Jieun-Lee
//age: 29
//phone: 010-1234-5678
```

<br>

### for... of

- **반복 가능한 (iterable) 객체**를 순회하며 값을 꺼낼 때 사용
  - 보통 배열 순회에 사용!
  - python의 for과 유사
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

```javascript
for (vriable of iterables) {
    //code
}
```

#### 예시

```javascript
const students = [
  {name: 'jieun'},
  {name: 'mina'},
  {name: 'inho'},
]

for (const student of students) {
    console.log(`name: ${student.name}`)
}
```

