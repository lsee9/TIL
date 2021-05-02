###### 210428_wed

##### ECMAScript6

<hr>



###### 오늘의 목차 :radio:

### 변수와 식별자

- 식별자
- 변수

##### 첫 단추:radio_button: 가 중요하다 

<hr>
<br>

# 1. 변수와 식별자

> 코딩하기위해서는 변수와 식별자가 필수!!!
>
> 가장 기본 내용입니다

## 1.1 식별자

### 정의와 특징

- 식별자(identifier) : **변수를 구분할 수 있는 변수명**
- 반드시 문자, 달러($), 또는 밑줄(_)로 시작 (숫자 NONO)
- 대소문자 구분!
- class명 외에는 모두 **소문자**로 시작
- 예약어 사용 불가 (for, if, case 등등)

### 작성 스타일

- **카멜 케이스** (`camelCase`, lower-camel-case)
  - 변수, 객체, 함수에 사용

```js
//변수
let name
let variableName

//객체
const userInfo = { name: 'jieun', age: 28 }

//함수
function onClick () {}
```

- **파스칼 케이스** (`PascalCase`, upper-camel-case)
  - 클래스, 생성자에 사용

```js
//클래스
class User {
    constructor(options) {
        this.name = options.name
    }
}

//생성자
const good = new User({
    name: 'leejieun'
})
```

- **대문자 스네이크 케이스** (`SNAKE_CASE`)
  - 상수 (constants)에 사용
    - 상수 : 개발자의 의도와 상관없이 **변경될 가능성이 없는 값**

```js
// 상수
const PI = Math.PI
const API_KEY = 'MYKEY'
```

<br>

<br>

## 1.2 변수

### 변수 선언 키워드 - let, const

> 주로 이 2가지를 사용합니다!

- **let**
  - **재할당 할 수 있는** 변수 선언 시 사용
  - 변수 **재선언 불가**
  - 블록 스코프

- **const**
  - **재할당 할 수 없는** 변수 선언 시 사용
  - 변수 **재선언 불가**
  - 블록 스코프

##### 용어 익히기 :jack_o_lantern:

- 선언 (Declaration)
  - <u>변수를 생성</u>하는 행위 또는 시점
- 할당 (Assignment)
  - 선언된 변수에 <u>값을 저장</u>하는 행위 또는 시점
- 초기화 (Initialization)
  - 선언된 병수에 <u>처음으로 값을 저장</u>하는 행위 또는 시점

```js
let flag  //선언
console.log(flag)  // undefined

flag = 1  //할당
console.log(flag)  // 1

let bar = 0  //선언 + 할당
console.log(bar)  // 0
```

<br>

#### 비교

- 재할당

  - **let** : 재할당 `가능`

  ```js
  let number = 10  //선언 및 초기값 할당
  number = 10		 //재할당
  
  console.log(number)  //10
  ```

  - **const** : 재할당 `불가`

  ```js
  const number = 10  //선언 및 초기값 할당
  number = 10		   // 재할당 불가
  
  => Uncaught TypeError: Assignment to constant variable.
  ```

- 재선언

  - **let** : 재선언 불가

  ```js
  let number = 10  //선언 및 초기값 할당
  let number = 50	 // 재선언 불가
  
  => Uncaught SyntaxError: Identifier 'number' has already been declared
  ```

  - **const** : 재선언 `불가`

  ```js
  const number = 10  //선언 및 초기값 할당
  const number = 50  //재선언 불가
  
  => Uncaught SyntaxError: Identifier 'number' has already been declared
  ```

- 블록스코프 (block scope)

  - if, for, function 등의 중괄호 내부를 가리킴
  - 블록 스코프를 가지는 변수는 블록 바깥에서 접근 불가능

  ```js
  let x = 1
  
  if (x === 1) {
      let x = 2
      console.log(x)  //2, 블록 내에서만 유효
  }
  
  console.log(x)  	//1
  ```

<br>

### 변수 선언 키워드 - var

- **var**
  - **재선언** 및 **재할당** 모두 **가능**

    ###### 이게 생각보다 좋은게 아닙니다...!

  - ES6 이전에 변수 선언시 사용되던 키워드

  - **호이스팅 되는 특성**으로 인해 예기치 못한 문제 발생 가능

    :point_right: ES6 이후부터는 var대신 const, let을 사용하는 것을 권장합니다!!

  - 함수 스코프

#### 확인

- 재선언 및 재할당 가능

```js
var number = 10		//선언 및 초기값 할당
var number = 50		//재선언 및 재할당

console.log(number)  //50
```

- 함수 스코프 (function scope)
  - 함수의 중괄호 내부를 가리킴
  - 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가

```js
function func() {
    var x = 5
    console.log(x)  //5
}
console.log(x)

=> Uncaught ReferenceError: x is not defined
```

- 호이스팅 (hoisting) 

  > (무언가를) 끌어올리다

  - 변수를 선언 이전에 참조할 수 있는 현상
  - 변수 선언 이전의 위치에서 접근 시 undefined를 반환

```js
console.log(username) 	//undefined
var username = 'jieun'

console.log(age)  		//Uncaught ReferenceError
let age = 28

console.log(phone)		//Uncaught ReferenceError
const phone = '010-1234-5678'
```

