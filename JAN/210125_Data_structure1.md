###### 210125_mon

# 오늘 배울 내용을 알아볼까요?

:question: 다음의 문장들을 보면 어떤 결과가 나올지 예상이 되나요?

```python
'Hi'.lower()  #hi, 소문자로 바꾼다
[1, 2, 3].remove(1)  #[2, 3], 1을 제거
[1, 2, 3].count(1)  #1, 1을 센다
{'바나나'}.add('수박')  #{'바나나', '수박'}, set에 요소를 더한다
{'b': 'banana'}.get('b')  #banana, 'b'의 값을 불러온다
```

이름으로 대강 유추가 가능하죠?? 오늘은 이런 구조에 대해 배울거랍니다 :wink:

> 위의 문장들은 모두 동일한 구조를 가지고 있습니다.
>
> > 데이터형식.행동()
>
> 데이터 형식에 맞춰 작동할 수 있는 행동이 다양하고, 이를 활용해서 알고리즘을 수월하게 작성할 수 있습니다. 이때 이 행동을 `메서드(method)`라고 한답니다!!



그럼 이제부터 이 구조를 자세히 배워보겠습니다!! 



# 데이터 구조(Data_structure) 1

> 데이터 구조(Data Structure)란 데이터에 편리하게 접근하고, 변경하기 위해서 데이터를 저장하거나 조작하는 방법을 말합니다.
>
> > Program = Datastructure + Algorithm
>
> - 알고리즘에 빈번히 활용되는 순서가 있는(ordered) 데이터 구조
>   - 문자열(String)
>   - 리스트(List)
> - 데이터 구조에 적용 가능한 Built-in Function
>
> 이러한 데이터 구조와 그와 관련된 메서드를 살펴보겠습니다.



본격적으로 수업에 들어가기에 앞서...

## Python이 왜 좋을까요? :thinking:

> 다른 언어보다 진입장벽이 낮습니다!! 왜 일까요?
>
> > [1, 2, 3]에서 1의 개수를 판단한다고 해봅시다.
> >
> > `for문과 if문을 활용`해서 1이 나올때마다 count를 해줘야 겠죠!!
>
> python에서는 더 쉽게?? 가능합니다!
>
> > [1, 2, 3].count(1)
>
> 겨우 한문장... 매우 간단하죠??
>
> 이처럼 python에서는 내장된 함수를 사용해서 빠르고 편하게, 또한 가독성있게 알고리즘을 작성할 수 있습니다!!!
>
> 물론 다양한 상황을 헤쳐나가려면 메서드에만 의존하면 안되는거 아시죠? 직접 구현하는 법도 당연히 익혀야합니다.
>
> 어쨌든 편한 방법임은 사실입니다.
>
> 이렇게 기본적으로 제공되는 메서드 덕분에 다른 언어보다 수월하게 진행할 수 있답니다.

그럼 이제 왜 배워햐하는지 감이 잡히시나요? 좀 더 편하기 위해!! 다양한 메서드를 배워봅시다!



:raised_hand_with_fingers_splayed:참!! 학습하면서 __데이터의 특징을 눈여겨 보세요__.

외우지 않아도 __메서드의 특징을 이해할 수 있을겁니다!__



# 1. 문자열(String)

> character들의 집합으로 다음의 특징을 가집니다.
>
> 변경할 수 없고(`immutable`), 순서가 있고(`ordered`), 순회가능한(`literable`)
>
> [문자열의 method](https://docs.python.org/ko/3/library/stdtypes.html#string-methods) :point_left: 확인해보세요(str.~~)

- 변경불가(immutable)
- 순서 있음(ordered)

```python
a = 'apple'
print(a[0])  #a, 인덱스로 접근가능 = ordered
a[0] = 'A'  #TypeError, 할당자체가 되지 않습니다 
```

- 순회 가능(literable)

```python
for char in a:  #반복문에 전달하여 사용할 수 있습니다
    print(char)
```



## 1.1 조회/탐색

### `.find(x)`

> x의 __첫 번째 위치__를 반환합니다. 없으면 `-1`을 반환합니다.

```python
a = 'apple'

a.find('a')  #0
a.find('p')  #1, p는 두개지만 그 중 첫번째 인덱스를 반환합니다.
a.find('y')  #-1
```



### `.index(x)`

> x의 __첫 번쨰 위치__를 반환합니다. 없으면 __오류__가 발생합니다.

```python
a = 'apple'

a.index('a')  #0
a.index('p')  #1
a.index('y')  #ValueError
```



### .find(x) vs .index(x)

- 둘 다  x의 __첫 번째 위치__를 반환합니다.
- 값이 없으면?
  - `.find(x)` : `-1`반환
  - `.index(x)` :  `Error`

> .index(x)도 try ~ except 구문으로 예외처리를 해줄 수 있지만... 그보다는 이미 제공하는 find(x)를 쓰는게 더 편하겠죠??



## 1.2 값 변경

### `replace(old, new[, count])`

> 바꿀 대상 글자를 __새로운 글자로 바꿔서 반환__합니다.
>
> count를 지정하면 `해당 갯수만큼만 시행`합니다. []로 표현된 건 옵션입니다!

```python
b = 'banana'

b.replace('a', '')  #'bnn', a를 공백으로 치환
b.replace('a', '', 2)  #'bnna', a를 공백으로 2번만 치환
```

###### 원본자체가 바뀌는건 아님!! 헷갈리지 말쟈



### `strip([chars])`

> 특정한 문자들을 지정하면, __양쪽을 제거__하거나 __왼쪽을 제거__하거나(lstrip), __오른쪽을 제거__합니다.(rstrip)
>
> 지정하지 않으면 __공백을 제거__합니다. (이거가 많이 쓰이고 유용!)

```python
hi = '\t  hi! \n'

hi.strip()  #'hi!', 양쪽
hi.lstrip()  #'hi! \n', 왼쪽
hi.rstrip()  #'\t  hi!', 오른쪽
```
- 띄어쓰기, 개행문자(escape, '\r\t\n' 등)도 공백으로 인식합니다.
- 공백제거 메서드의 중요성
```python
'홍길동 ' == '홍길동'  #False, 공백으로인해 같지 않음
```

> 홈페이지에서 회원가입을 하는데 사용자가 공백을 입력하면, 서로 다른 정보로 인식할 위험이 있겠죠? 그래서 이런 경우 일괄적으로 공백을 제거해서 사용합니다!



### `.split()`

> 문자열을 __특정한 단위로 나누어__ 리스트로 반환합니다.
>
> :star:특히 많이 쓰니까 기억해둡시다!

```python
word = 'a,b,c'
word.split(',')  #['a', 'b', 'c'], ','를 구분자로 리스트를 생성합니다.

number = '1 2 3'
number.split()  #['1', '2', '3'], default는 공백을 구분자로 합니다.
```



### `'separator'.join(literable)`

> 특정 __문자열로 만들어__ 반환합니다.
>
> __반복가능한(literable) 컨테이너의 요소__들을 `separator`를 구분자로 `합쳐(join())` 문자열로 반환합니다.

```python
word = 'wow'
'!'.join(word)  #'w!o!w', !를 구분자로 string의 요소 문자열로 반환

words = ['hi', 'hello']
','.join(words)  #'hi,hello', ,(콤마)를 구분자로 list의 요소 문자열로 반환

words = [1, 2]
','.join(words)  #TypeError, 요소가 str인 경우만 가능
```



## 1.3 문자 변형

### `.capitalize()`, `.title()`, `.upper()`

> `.capitalize()` : 앞글자를 대문자로 만들어 반환합니다.
>
> `.title()` : 어포스트로피나 공백 이후를 대문자로 만들어 반환합니다.
>
> `.upper()` : 모두 대문자로 만들어 반환합니다.

```python
a = 'hI! Everyone, I\'m kim'

a.capitalize()  #"Hi! everyone, i'm kim", 첫글자만 대문자
a.title()  #"Hi! Everyone, I'M Kim", 특정 조건에만 대문자
a.upper()  #"HI! EVERYONE, I'M KIM", 전부 대문자
```



### `.lower()`, `.swapcase()`

> `.lower()` : 모두 소문자로 만들어 반환합니다.
>
> `.swapcase()` : 대 <-> 소문자로 변경하여 반환합니다.

```python
a = 'hI! Everyone, I\'m kim'

a.lower()  #"hi! everyone, i'm kim", 전부 소문자
a.swapcase()  #"Hi! eVERYONE, i'M KIM", 대소문자 서로 변경
```





## 1.4 기타 문자열 관련 검증 메소드 : 참/거짓 반환

> .isalpha() , .isdecimal() , .isdigit() , .isnumeric() , .isspace() , .isupper() , .istitle() , .islower()