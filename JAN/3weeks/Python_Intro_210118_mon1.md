###### Jan_3weeks\_Day1_210118(mon)



# Python_Intro

> python을 본격적으로 배우기에 앞서 기초적인 부분을 먼저 정리해봅시다!



:smiley: 학습을 시작하기에 앞서, python의 개발환경을 알아볼까요?

## Jupyter notebook

> 2주간의 python 기초 수업에서 우리는 jupyter notebook을 사용할 것입니다! 사용하기에 앞에 이에대해 알아볼까요?

### 1. 특징

- REPL(Read Eval Print Loop)
  - 파이썬 대화형 개발 환경인 REPL중 하나입니다.
- __셀 단위의 코드실행__ :star:
  - 한 줄 한 줄 결과를 확인하면서 개발할 수 있습니다.
- MarkDown 문법을 통한 풍부한 문서화 가능



### 2. 설치 및 실행

- 설치

  - Git Bash 실행 후 아래 명령어를 입력하면 설치가 진행됩니다.

    ```bash
    pip install notebook #pip는 python의 패키지 관리자 입니다.
    ```

- 실행

  1. 저장을 원하는 폴더를 열어 [마우스 오른쪽 버튼] - [Git Bash Here]

     ```bash
     jupyter notebook #명령어를 입력하면 실행됩니다.
     ```

  2. [New] - [Python3] 를 누르면 새로운 .py 파일이 생성됩니다.



### 3. 자주쓰는 단축키

- Ctrl + Enter : 셀 실행하여 결과확인
- a (above) / b (below) : 위/아래에 셀 생성
- m (makrdown) : 마크다운 문법 사용하기
- h (help) : 여러가지 단축키를 확인하세요!

단축키는 `command mode(명령모드/파란 셀)`에서 수행해야 합니다! 

edit mode(수정모드/ 초록셀)에서 ESC를 누르면 명령모드, Enter를 누르면 다시 수정모드



### 4. 폰트 설정하기

> 0, O/ l, I 글자가 구분되지 않으시나요? 글자를 잘못 읽는 실수를 없애기 위해 `D2coding`폰트를 사용하겠습니다.

- [D2coding](https://github.com/naver/d2codingfont/releases/tag/VER1.3.2) 다운받기
- Chrome - [설정] - [모양] - [글꼴맞춤설정] - [고정폭 글꼴]에서 폰트를 수정합니다.



#### [PEP-8](https://www.python.org/dev/peps/pep-0008/)

> python 코드가 어떻게 작성되야 하는지에대한 가이드 입니다! 이를 숙지하여 누구나 보기 쉽게 코드를 작성하는 스타일을 익히도록 합시다!!





## 기초 문법(Syntax)

> python을 사용하면서 필요한 기초 문법을 알아봅시다.

### 1. 주석(Comment)

> 코드와 함께 실행되지 않는 부분으로, 코드에 대한 간략한 설명을 작성할 때 활용합니다.

- 한 줄 주석 : `#`로 표현

- 여러줄 주석

  - 한 줄씩 `#` 사용 (보통의 경우에 사용합니다.)

  - `'''` or `"""` (여러줄 문자열, multiline string)사용

    (multiline은 주로 함수/클래스를 설명(docstring)하기 위해 활용됩니다.)

  ```python
  # 한줄 주석입니다.
  '''
  멀티라인입니다.
  여러줄을 주석처리할 때 용이합니다.
  '''
  ```



### 2. 코드라인

- `1줄에 1문장(statment)`가 원칙

- 문장(statment): 파이썬이 실행 가능(executable)한 최소한의 코드 단위

- 문장 끝에 `;`을 작성하지 않습니다. (작성해도 에러는 없음)

- 한 줄에 여러 문장을 작성할 경우, `;`을 작성하여 구분합니다.

  ```python
  x + y  #한문장, 문장끝에 ;쓰지 않음
  print('hello');print('python') #한줄에 표현할 경우 ;로 구분합니다.
  ```

- 줄을 여러 줄 작성할 경우 `\`(backslash)를 사용합니다. 

  ``` python
  print('Hello\
        Python')
  
  #PEP-8 가이드에 따라 여러줄 문자열은 아래와 같이 쓰는 것이 관계(convention)입니다.
  print('''Hello
        Python''')
  ```

  

- `[] {} ()`는 \없이도 표현 가능합니다.

  ```python
  lunch = [
      'sandwitch', 'noodles', 'pizza'
  ]
  #PEF-8 가이드에 따라 여러줄의 생성자의 닫히는 괄호(소, 중, 대)는 
  #(1)첫번째 문자(요소)위치에 오거나 
  #(2) 마지막 줄에서 생성자가 시작되는 첫번째 열에 옵니다.
  #이 중 (1)방식을 사용할 것입니다.
  ```






## 변수(Variable)

> 우리가 보통 물건을 담는 박스를 떠올려 보세요. 박스에 이름을 붙이고 물건을 넣어 정리해둔다면 어떨까요? 필요할 때마다 이름이 적힌 박스를 찾아 물건을 사용할 수 있을거에요!!
>
> 프로그래밍을 할 때로 이와 비슷한 방식을 사용합니다. 사용할 데이터의 특성에 맞는 이름을 붙이고, 데이터를 넣어서 사용합니다. 이때 이름을 붙인 박스는 __변수(variable)__이고, 변수에 값을 넣는 것을 __변수에 값을 할당한다__고 합니다.

### 1. 변수(Variable)

- 할당연산자(Assignment Operator) `=` 을 통해 변수에 데이터가 할당됩니다.

- `type()`을 활용해 해당 데이터 타입을 확인합니다. (항상 확인하는 습관을 들입시다!)

- `id()`를 활용해 해당 값의 메모리 주소를 확인합니다.

  ```python
  sum = 60 + 10  # = 을 기준으로 오른쪽 값을 왼쪽 변수에 할당합니다.
  print(sum, type(sum)) #타입을 함께 확인해야 데이터를 정확히 파악할 수 있습니다.
  
  print(id(sum))  #컴퓨터의 해당 특정 공간에 들어있다는 의미입니다.
  				#(컴퓨터 주소라고 생각하면 됩니다.)
  ```

- 같은 값을 동시에 할당 가능합니다.

- 다른 값을 동시에 할당 가능합니다.

  ```python
  x = y = 20  #같은 값을 동시에 할당
  x, y = 10, 20 #다른 값을 동시에 할당
  
  x, y, z = 10, 20  #변수의 개수가 더 많을 때 오류, 값을 늘리거나 변수를 지워주세요
  x, y = 10  #변수의 개수가 더 적을때 오류, x = y = 10으로 수정 필요
  ```

- __swap__** : 위의 성질을 활용해 쉽게 값을 바꿀 수 있습니다.

  ```python
  x = 10
  y = 20
  x, y = y, x  #이처렁 값을 서로 바꿀 수 있습니다.
  
  #일반적인 언어 - 임시변수 사용
  temp = x
  x = y
  y = temp
  ```

#### 1.1 식별자(Identifiers)

> 식별자는 변수, 함수 모듈, 클래스 등을 식별하는데 사용되는 이름(name)입니다. 이름짓는 법을 알아볼까요?

- 식별자의 이름은 영문알파벳(대,소문자), 밑줄(_), 숫자로 구성됩니다.

- 첫 글자에 숫자는 올 수 없습니다.

- 대소문자(case)를 구분합니다.

- 길이 제한이 없습니다.

- 아래의 `키워드`는 사용할 수 없습니다.

  ```python
  False, None, True, and, as, assert, async, await, break, class, continue, def, del, 
  elif, else, except, finally, for, from, global, if, import, in, is, 
  lambda, nonlocal, not, or, pass, raise, return, try, while, with, yield
  ```

  ```python
  import keyword
  print(keyword.kwlist)  #키워드를 확인할 수 있습니다.
  ```

- 내장함수나 모듈 등의 이름으로도 만들면 안됩니다.

  (사용은 가능하나, 더이상 기존의 동작을 수행하지 못합니다.)

  ```python
  print = 'hi'  #print는 'hi'라는 값으로 인식됩니다.
  print('hello')  #이전의 기능을 수행할 수 없습니다.
  del print  #원래의 기능을 수행하도록 생성한 print를 지워주세요.
  ```






## 데이터 타입(Data Type)

> 지금까지 박스에 대해 알아봤습니다. 그렇다면 박스에 무엇을 담을 수 있을지 궁금하지 않나요? 
>
> 이제부터 박스에 어떤 값이 들어갈 수 있는지 알아보겠습니다!

### 1. 숫자(Number) 타입

#### 1.1 `int` (정수, integer)

- 모든 정수는 `int`로 표현합니다.

- python 3.x버전에서는 `long`타입은 없고 모두 `int` 타입으로 표기됩니다.

  - 임의정밀도산술 방식을 택하여 __매우 큰 수도 정수도 표현 가능__합니다.
  - 다른 언어와 달리 type의 표현 가능한 범위를 가지지 않습니다.

- 2진수 : `0b` / 8진수 : `0o` / 16진수 : `0x` 로도 표현 가능합니다.

  ```python
  a = 100
  print(a, type(a))  #100 <class 'int'>
  b = 100 ** 100  
  print(b, type(b))  #매우 큰수도 'int'로 표현 가능합니다.
  ```

  ``` python
  decimal_number = 10  #10진수
  binary_number = 0b10  #2진수
  octal_number = 0o10  #8진수
  hexadecimal_number = 0x10  #16진수
  ```

  ##### 파이썬에서 표현할 수 있는 가장 큰 수

  - python에서 가장 큰 숫자를 활용하기 위해 sys모듈을 불러온다.

    ```python
    import sys
    max_int = sys.maxsize  #2**63-1 => 64비트에서 부호비트를 뺀 63개의 최대치
    super_max = sys.maxsize * sys.maxsize
    ```

  - python은 기존 C 계열 프로그래밍 언어와 달리 정수형에서 오버플로우가 없습니다.

    > __오버플로우(overflow)__
    >
    > 데이터 타입별로 사용할 수 있는 `메모리의 크기가 제한`되어 있습니다.
    >
    > 이때 표현할 수 있는 수의 범위를 넘어가는 연산을 하면 기대했던 값이 출력되지 않는 현상, 즉 `메모리가 차고 넘쳐 흐르는 현상`인 오버플로우가 발생합니다.

  - 이는 임의 정밀도 산술(arbitrary-precision arithmetic)을 사용하기 때문입니다.

    > __임의 정밀도 산술(arbitrary-precision arithmetic)__
    >
    > 사용할 수 있는 메모리양이 정해진 기존 방식과 달리, 현재 `남아있는 만큼의 가용 메모리를 모두 수 표현에 끌어다 쓸 수 있는` 형태입니다.
    >
    > 특정 값을 나타내는데 4바이트가 부족하다면 5바이트, 더 부족하면 6바이트까지 사용할 수 있게 유동적으로 운용됩니다.



#### 1.2 `float` (부동소수점, 실수, floating point number)

- 실수는 `float`로 표현됩니다.

  > 실수를 컴퓨터가 표현하는 과정에서 부동소수점을 사용하며, 항상 같은 값으로 일치되지 않습니다. 이를 `Floating point rounding error`라 합니다.
  >
  > 이는 컴퓨터가 2진수(비트)를 통해 숫자를 표현하는과정에서 생기는 오류이며, 대부분의 경우는 중요하지 않으나 __값이 같은지 비교하는 과정에서 문제가 발생__할 수 있습니다.

- 컴퓨터식 지수 표현 방식

  -  e (or E)를 사용할 수 있습니다.

  ```python
  c = 3.5
  print(c, type(c))  #3.5 <class 'float'>
  ```

  ```python
  pi = 314e-2  #3.14의 컴퓨터식 지수표현
  ```

- 실수의 연산

  > 실수의 경우 실제로 값을 처리하기 위해서는 조심할 필요가 있습니다.
  >
  > 앞서 언급했던 것처럼 실수의 연산에서는 rounding error가 발생할 수 있습니다. 이를 처리할 일은 크게 없지만, 발생하는 상황과 해결법을 알아봅시다.

  - 덧셈

    ```python
    3.5 + 3.2  #6.7, 덧셈은 크게 문제되지 않습니다.
    ```

  - 뺄셈

    ```python
    3.5 - 3.12  #0.3799999999999, 우리의 예상과는 다른 수가 나옵니다.
    3.5 - 3.12 == 0.38  #False, 우리는 True라는 값을 얻길 원합니다.
    ```

    - round()를 사용해 표현할 수는 있으나 완벽한 방법은 아닙니다.

    ```python
    round(3.5 - 3,12, 2)  #반올림하여 소수점 둘째자리까지 표현
    					  #0~4(내림), 5(짝수: 내림/홀수: 올림)
    ```

    - 3가지 방법을 사용해 `rounding error을 해결`하고 원하는 결과를 얻을 수 있습니다.

    ```python
    #1. 기본적인 처리방법
    #절댓값으로 표현한 뒤, 아주 작은 수보다 차이가 작은지 확인하는 방법입니다.
    a = 3.5 - 3.12
    b = 0.38
    abs(a - b) <= 1e-10
    ```

    ```python
    #2. sys 모듈(내장 모듈)을 통한 처리방법
    #epsilon은 부동소수점연산에서 반올림을 함으로써 발생하는 오차를 상환합니다.
    import sys
    abs(a - b) <= sys.float_info.epsilon  
    ```

    ```python
    #3. math 모듈(내장모듈)을 통한 처리방법(3.5부터 가능)
    import math
    math.isclose(a, b)
    ```

    

#### 1.3 `complex` (복소수, complex number)

- 각각 실수로 표현되는 실수부와 허수부를 가집니다.

- 허수부를 `j`로 표현합니다.

  (많이 사용되지는 않으나 표현법은 알아두도록 합시다!)

  ```python
  a = 3 - 4j
  print(a, type(a))  #(3-4j) <class 'complex'>
  
  complex('1+2j')  #문자열을 복소수로 변환
  complex('1 + 2j')  #ValueError, 문자열 변환시 +, -연산자 주위에 공백을 포함해서는 안됩니다.
  ```






### 2. 문자(String) 타입

#### 2.1 기본 활용법

- 문자열은 Single quotes(`'`)나 Double quotes(`"`)을 활용해 표현 가능합니다.

  - 작은따옴표: '"큰" 따옴표를 담을 수 있습니다.'
  - 큰따옴표: "'작은' 따옴표를 담을 수 있습니다."
  - 삼중 따옴표: '''세 개의 작은따옴표''' or """세 개의 큰 따옴표"""

  > 단, 문자열을 묶을 때 동일한 문장부호를 활용해야하며, `PEP-8`에서는 __하나의 문장부호를 선택__하여 유지하도록 하고 있습니다.(Pick a rule and Stick to it)
  >
  > 우리는 작은따옴표(`'`)를 기본으로 사용하겠습니다.

  ```python
  #다음처럼 값은 동일하게 출력되므로 항상 type에 대한 확인이 필요합니다.
  num1 = '1'
  num2 = 1
  print(num1, type(num1))  #1 <class 'str'>
  print(num2, type(num2))  #1 <class 'int'>
  ```

- 사용자에게 받은 입력은 기본적으로 str입니다.

  ```python
  num = input()  #셀에 [*]표시와 함께 입력창이 뜹니다.
  print(num, type(name))  #1 <class 'str'>
  ```

- 따옴표 사용

  > 큰따옴표(`"`)는 문자열 사이에 따옴표를 쓰고싶다 하는 경우만 사용하겠습니다.
  >
  > 문자열 안에 문장부호(`'`, `"`)가 사용될 경우 이스케이프 문자(`\`)를 활용 가능합니다.

  ```python
  print('철수가 말했다. '안녕?'')  #같은 종류의 따옴표를 중첩해쓰면 원하지 않은 따옴표끼리 짝이 됩니다.
  print('철수가 말했다. "안녕?"')  #다른 문장부호 사용을 통한 해결
  print('철수가 말했다. \'안녕?\'')  #이스케이프 문자를 활용한 해결
  ```

- 여러줄에 걸친 문장은 `PEP-8`에 따라 `"""`을 사용합니다.

  ```python
  print("""
  개행문자 없이
  여러 줄을 그대로 출력 가능합니다.
  """)
  ```

- `+`연산자로 문자열을 이어붙이고, `*`연산자로 반복시킬 수 있습니다.

  ```python
  'hi' * 3  #'hihihi', 3번 반복
  'hi' + 'sung'  #'hisung'
  
  #변수화 해서도 동일한 연산 가능합니다.
  a = 'hi'
  b = ', sung'
  a + b  #'hi, sung'
  ```

  

#### 2.2 이스케이프 시퀀스(Escape Sequence)

> 문자열에서 특수문자 혹은 조작을 하기위해 사용되는 것으로 `\`를 활용하여 이를 구분합니다.

| 예약문자 | 내용(의미) | 예약문자 |   내용(의미)    |
| :------: | :--------: | :------: | :-------------: |
|    \n    |  줄 바꿈   |    \\    |        \        |
|    \t    |     탭     |    \'    | 단일인용부호(') |
|    \r    | 캐리지리턴 |    "     | 이중인용부호(") |
|    \0    |  널(Null)  |          |                 |

```python
print('줄바꿈\\n\n그리고 탭\\t\t출력')  #줄바꿈\n
									 #그리고 탭\t	출력
```

- print()의 기본 개행은 \n, `end`옵션으로 변경가능

  ```python
  print('기본은 줄바꿈\\n')
  print('탭도', end = '\t')
  print('가능')
  print('붙어라', end = '')
  print('얍')
  print('이스케이프 말고도 가능', end = '!')
  ```

  기본은 줄바꿈\n
  탭도	가능
  붙어라얍
  이스케이프 말고도 가능!

  

#### 2.3 String Interpolation

> 문자열 안에 변수 넣는 방법!!

- %-formatting

  - %d : 정수
  - %f : 실수
  - %s: 문자열

- *str.format()

  - 넣어져 있는 값 그대로 보여주기 위함, 고민없이 사용 가능합니다.

- *f-strings (3.6이후 버전부터 가능)

  - 문장이 길어져도 가독성이 좋습니다.(가장 편한방법)
  - 여러줄 문자열도 가능합니다.

  ```python
  name = 'Kim'
  score = 4.5
  print('Hello, %s 내 성적은 %d(%f)야!' %(name, score, score))
  print('Hello, {0} 내 성적은 {1}야!'.format(name, score))
  print(f'Hello, {name} 내 성적은 {score}야!')
  ```

  ​	Hello, Kim 내 성적은 4(4.500000)야!
  ​	Hello, Kim 내 성적은 4.5야!
  ​	Hello, Kim 내 성적은 4.5야!

:hand: f-strings의 활용

- 형식을 지정할 수 있습니다.

  ``` python
  #형식지정자를 사용하여 날짜를 표시해봅시다.
  import datetime
  today = datetime.datetime.now()
  print(today)
  print(f'오늘은 {today:%y}년 {today:%m}월 {today:%d}일 {today:%A}')
  ```

  ​	2021-01-19 16:56:36.703550
  ​	오늘은 21년 01월 19일 Tuesday

- 연산과 출력형식 지정도 가능합니다.

  ```python
  pi = 3.141592
  print(f'원주율은 {pi:.4}! 반지름이 2일때 원의 넓이는 {pi * 2 * 2}')  #.4는 소수점아래 4자리에서 반올림
  ```

  ​	원주율은 3.142! 반지름이 2일때 원의 넓이는 12.566368



### 3. 참/거짓 (Boolean) 타입

> 컴퓨터의 언어는 0과 1로 이뤄져 있다는 것 알고계시죠? 그래서 우리가 배우는 python은 이를 번역해주는 역할을 합니다.
>
> 어쨌든! 그렇기 때문에 Boolean은 가장 근본이 되는 데이터타입이라 할 수 있습니다.

- `True`, `False`로 이뤄져있습니다. (대문자에 주의!!)

- 비교/논리 연산 수행 등에서 사용됩니다.

- 다음은 `False`로 변환됩니다.

  - 0, 0.0, (), [], {}, '', None 

  ```python
  bool(0)  #False
  bool('0')  #True
  bool(0.1)  #True
  bool([])  #False, 일반적으로 비어있는 건 전부 False
  bool(None)  #False
  ```

#### 3.1 None 타입

> 값이 없음을 표현하기위해 `None`타입이 존재합니다! 초기화를 하거나 비워두고 싶을 때 사용하면 됩니다.

```python
a = None
print(a, type(a))  #None <class 'NoneType'>
```



### 4. 형변환(Type conversion, Typecasting)

> 데이터타입은 서로 변환할 수 있습니다.

#### 4.1 암시적 형변환(Implicit Type COnversion)

> 사용자가 의도하지 않아도, 파이썬 내부에서 __자동으로 변환__하는 경우입니다.

- 두가지 상황에서만 가능합니다.

  - Bool
  - Number (int, float, complex)

  ```python
  True + 5  #6, True를 int 1로 변환
  ```

  ```python
  int_num = 3
  float_num = 5.0
  complex_num = 3 + 5j
  
  result1 = int_num + float_num
  result2 = int_num + complex_num
  print(result1, type(result1))  #8.0 <class 'float'>
  print(result2, type(result2))  #6+5j <class 'complex'>
  ```

  - 다른 타입의 연산 결과 타입은 넓은 수체계로 변환합니다.

    ![수체계](https://lh3.googleusercontent.com/proxy/JPG2G3eNrBL9g8KJqmBqBSfvU_tajn8XVW1xX9jXjt58VFOqjRjgw9pc2g5osw7NR__w08NY78TtTBwbfDda1aLEXdRxSeldM5S_Y9WVSVzfJt4B)

#### 4.2 명시적 형변환(Explicit Type Conversion)

> 사용자가 __명시적으로 형변환__을 해야하는 상황입니다.

- 위의 상황을 제외한 모든 경우에 해당합니다.

  - string -> integer : 형식에 맞는 숫자만 가능
  - integer -> string : 모두 가능

- 암시적 형변환이 되는 모든 경우도 명시적 형변환이 가능합니다.

  - `int()` : string, float를 int로 변환

  - `float()` : string, int를 float로 변환

  - `str()`: int, float, list, tuple, dictionary를 문자열로 변환

    (list(), tuple()는 다음 챕터에!!)

  ```python
  
  ```

  

## 연산자(Operator)

### 1. 산술 연산자



### 2. 비교 연산자



### 3. 논리 연산자





#### 3.1 단축평가***



### 4. 복합 연산자



### 5. 기타 주요 연산자

#### 5.1 Concatenation



#### 5.2 Containment Test



#### 5.3 Identity



#### 5.4 Indexing/Slicing



### 6. 연산자 우선순위





#### 6.1 [참고] 표현식 (Expression) & 문장 (Statement)

##### 6.1.1 표현식(Expression)



##### 6.1.2 문장(Statement)



##### 6.1.3 문장과 표현식의 관계







