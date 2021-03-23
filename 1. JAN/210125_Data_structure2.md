###### 210125_mon

:joy: 이번에는 순서가 없는 친구들을 좀 알아보겠습니다아ㅏ

제 정신머리도 멘붕이 와서 순서가 없는거같은데여

힘을내봅시다아ㅏ



# 데이터 구조(Data_structure) 2

> 알고리즘에 빈번히 활용되는 순서가 없는(`unordered`) 데이터 구조가 뭘까요?
>
> > 세트(set)
> >
> > 딕셔너리(Dictionary)
>
> 이중엔 특히 딕셔너리가 많이 활용되는것 같습니다!!!
>
> 어쨌든 둘다 잘 알아둬야겠죠?



# 1. 세트(set)

> 변경 가능(`mutable`), 순서가 없는(`unordered`), 순회가능한(`iterable`) 특징을 가지고 있습니다!!
>
> `순서가 없다`는건 내가 `순서를 제어할수없다`라고 받아들이면 더 쉬울것같아요!
>
> [set의 method](https://docs.python.org/ko/3/library/stdtypes.html#set-types-set-frozenset):heavy_check_mark:한번쯤 확인!

## 1.1 추가 및 삭제

> 변경 가능(`mutable`)한 특징이 있기 때문에 추가 및 삭제가 가능합니다!!!
>
> list처럼 원본이 변하는걸 생각할 수 있겠죠??



##### :pushpin:메서드는 이름만 잘 봐도 기능을 유추할 수 있다는 사실!!!



### `.add(elem)`

> elem을 set에 추가합니다.

```python
fruit = {'apple', 'banana'}

fruit.add('lemon')
print(fruit)  #{'banana', 'apple', 'lemon'}
```

- set에서 입력한 순서, 출력되는 순서는 중요하지 않습니다!
- 값이 어디로 추가될지는 알수 없습니다!
  - 이 역시 위치가 중요한게 아니라 `추가되었다`는 사실이 중요하다는 생각이 듭니다.



### `.update(*others)`

> 여러가지 값을 추가합니다. (가변 인자 리스트!! 몇개나 들어올지 알 수 없을때 사용하죠!)
>
> 인자로는 반드시 __iterable 데이터 구조__를 전달해야 합니다.

```python
fruit = {'apple', 'banana'}
fruit.update('lemon')
print(fruit)  #{'n', 'l', 'apple', 'e', 'm', 'banana', 'o'}
```

- iterable 데이터를 전달하면, 그 요소가 하나씩 추가됩니다.

```python
fruit = {'apple', 'banana'}
fruit.update(('lemon',))  #하나의 요소를 가진 tuple 입니다
print(fruit)  #{'banana', 'apple', 'lemon'}
```

- 단어자체를 넣으려면 하나로 묶어줘야합니다.(list, tuple, set, dict 등등)



### `.remove(elem)`

> elem을 set에서 삭제하고, 없으면 KeyError가 발생합니다.
>
> list에선 ValueError가 났었죠!! 꽤 비슷합니다.

```python
fruit = {'apple', 'banana', 'lemon'}

fruit.remove('apple')
print(fruit)  #{'banana', 'lemon'}
```

```python
fruit = {'apple', 'banana', 'lemon'}
fruit.remove('pineapple')  #KeyError
```



### `.discard(elem)`

> elem을 set에서 삭제하고, 없어도 에러가 발생하지 않습니다.

```python
fruit = {'apple', 'banana', 'lemon'}

fruit.discard('apple')
print(fruit)  #{'banana', 'lemon'}
```

```python
fruit = {'apple', 'banana', 'lemon'}

fruit.discard('pineapple')  #통과
print(fruit)  #{'banana', 'apple', 'lemon'}
```



### `.pop()`

> __임의의 원소__를 제거해 반환합니다.
>
> 리스트는 인덱스를 지정했죠?? set은 순서가 없기 때문에 지정할 수 없습니다. 랜덤으로 알아서 제거해줍니다!

```python
fruit = {'apple', 'banana', 'lemon', 'pineapple'}
fruit.pop()
print(fruit)  #{'apple', 'lemon', 'pineapple'}
```





# 2. 딕셔너리(Dictionary)

> 변경 가능하고(`mutable`), 순서가 없고(`unordered`), 순회 가능한(`iterable`) 특징이 있습니다.
>
> `key: vlaue` 페어(pair)의 자료구조를 가지고있어, key를 통해 값에 접근할 수 있습니다.
>
> [dict의 method](https://docs.python.org/ko/3/library/stdtypes.html#mapping-types-dict) :pushpin: 체크체크@@



## 2.1 조회

> 값을 가져와볼까요?

### `.get(key[, default])`

> key를 통해 value를 가져옵니다.
>
> 절대로 __KeyError가 발생하지 않습니다.__ default는 기본적으로 None입니다.

```python
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit['melon']  #KeyError
```

- 존재하지 않는 key를 사용하면 Error가 발생합니다.

```python
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.get('melon')  #None
```

- get을 사용해서 key가 없으면 `None`를 return합니다.
- 다음처럼 return할 값을 지정할 수 있습니다.

```python
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.get('melon', 0)  #0
```





## 2.2 추가 및 삭제

> 변경 가능(`mutable`)한 특징에 따라 값을 추가하고 삭제해봅시다.

### `.pop(key[,default])`

> key가 dict에 있으면 제거하고, 그 값을 반환합니다.
>
> 그렇지 않으면 default를 반환합니다.
>
> default가 없는 상태에서 dict에 없으면 KeyError이 발생합니다.

```python
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.pop('apple')  #'a'반환
print(fruit)  #{'banana': 'b', 'lemon': 'c'}
```

```python
#default가 없는 경우
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.pop('melon')  #KeyError
```

```python
#default가 있는 경우
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.pop('melon', 0)  #0
```



### `.update()`

> 값을 제공하는 key, value로 덮어씁니다.
>
> 말그대로 값을 업데이트 합니다!

```python
fruit = {'apple': 'a', 'banana': 'b', 'lemon': 'c'}
fruit.update({'banana': 'B'})  # == fruit.update(banana = 'B')
print(fruit)  #{'apple': 'a', 'banana': 'B', 'lemon': 'c'}
```





## 2.3 딕셔너리 순회(반복문 활용)

> dict에 `for`문을 실행하면 기본적으로 key가 전달됩니다.

```python
students = {'kim': 98, 'lee': 90, 'park': 100}
for student in students:
    print(student, end =' ')  #kim lee park 
```

- __key__에 접근할 수 있으면 __value__에도 접근할 수 있기때문에 기본적으로 key가 전달됩니다.



#### :thinking: dict의 value는 어떻게 출력할 수 있을까요?

```python
students = {'kim': 98, 'lee': 90, 'park': 100}

print(students.keys())  #dict_keys(['kim', 'lee', 'park']) <class 'dict_keys'>
print(students.vlaues())  #dict_values([98, 90, 100]) <class 'dict_values'>
print(students.items())  #dict_items([('kim', 98), ('lee', 90), ('park', 100)]) <class 'dict_items'>
```

- 타입!! 주의하세요



#### dict에서 `for`활용하는 방법 :pushpin: (꽤 자주써요!)

```python
#0. dictionary순회 (key활용) - 기본
for key in dict:
    print(key)
    print(dict[key])
```

```python
#1. .keys()활용 - 0와 동일해요
for key in dict.keys():
    print(key)
    print(dict[key])
```

```python
#2. .values()활용 - value 직접 사용
for val in dict.values():
    print(val)
```

```python
#3. .items()활용 - key, value사용
for key, val in dict.items():
    print(key, val)
```

- 2, 3도 꽤 많이 쓰이는 방법입니다!! (0, 3을 많이 쓰는듯)



#### enumerate() vs .values()

```python
#enumerate()
students = {'kim': 98, 'lee': 90, 'park': 100}
for key, value in enumerate(students):
    print(key, value, type(key), type(value))

#출력결과
0 kim <class 'int'> <class 'str'>
1 lee <class 'int'> <class 'str'>
2 park <class 'int'> <class 'str'>
```
- 인덱스와 값의 pair를 출력할 때 사용합니다. (dict의 값은 key입니다.)
- 다른 iterable 데이터 타입에도 사용 가능합니다.
```python
#.items()
students = {'kim': 98, 'lee': 90, 'park': 100}
for key, value in students.items():
    print(key, value, type(key), type(value))

#출력결과
kim 98 <class 'str'> <class 'int'>
lee 90 <class 'str'> <class 'int'>
park 100 <class 'str'> <class 'int'>
```

- key와 value의 pair 출력할 때 사용합니다.
- dict에만 있는 메서드 입니다.



### 예) 

> 리스트의 요소의 개수를 value로 dict 생성
>
> > [출력예시]
> >
> > {'great': 2, 'expectations': 1, 'the': 2, 'adventures': 2, 'of': 2, 'sherlock': 1, 'holmes': 1, 'gasby': 1, 'hamlet': 1, 'huckleberry': 1, 'fin': 1}

```python
book_title =  ['great', 'expectations', 'the', 'adventures', 'of', 'sherlock', 'holmes', 'the', 'great', 'gasby', 'hamlet', 'adventures', 'of', 'huckleberry', 'fin']

count = {}
for word in book_title:
    #count[word] += 1  #KeyError, 해당하는 key가 존재하지 않음
    count[word] = count.get(word, 0) + 1
print(count)
```

##### :heavy_check_mark: 핵심 point

- for문
  - word에 dict의 key를 저장
- .get(key[, default])
  - count에서 해당하는 key를 찾지못하면 0을 반환하여 초기화 가능





## 2.4 Dictionary comprehension

> 리스트와 마찬가지로 comprehension을 활용하여 만들 수 있습니다.

### 활용법

- `iterable`에서 `dict`를 생성할 수 있습니다.

```python
{key: value for 요소 in iterable}
dict({key: value for 요소 in iterable})
```

### 예)

```python
#문자 1, 2, 3을 키로 1, 2, 3의 값을 갖는 dict생성
number = {1, 2, 3}
number_dict = {str(n):n for n in number}
print(number_dict)  #{'1': 1, '2': 2, '3': 3}
```





## 2.5 Dictionary comprehension + 조건

> list comprehension과 유사하게, 조건문에 참인 식으로 딕셔너리를 생성합니다.

### 활용법

```python
{key: value for 요소 in iterable if 조건}
dict({key: value if 조건식 else 값 for 요소 in iterable})
```

- 값이 포함되냐 아니냐의 문제면 `if` !
- 조건이 여러개 필요하면 `조건표현식`!

### 예)

```python
#홀수만 표현
number = {1, 2, 3, 4, 5}
number_dict = {str(n):n for n in number if n % 2}
print(number_dict)  #{'1': 1, '3': 3, '5': 5}
```

```python
#짝수는 음수로 표현
number = {1, 2, 3, 4, 5}
number_dict = {str(n):n if n % 2 else -n for n in number}
print(number_dict)  #{'1': 1, '2': -2, '3': 3, '4': -4, '5': 5}
```





##  :pushpin:Dict method 

> __key: value pair__구조로 key를 통해 value에 접근할 수 있다는 점!!
>
> 또한 수정이 가능하다는 점!!

- 조회
  - .get(key[, default])
- 추가 및 삭제
  - .pop(key[, default])
  - .update()
- 딕셔너리 순회 - 반복문 사용
  - .keys()
  - .values()
  - .items()
- Dictonary Comprehension
  - 표현식과 제어문을 통한 리스트 생성







##### 휴... 이것으로 데이터 구조 및 메서드는 마치겠습니다.

##### 이런저런 걱정이 많습니다ㅜㅜ 좀더 빠릿빠릿하게 따라가봅시다ㅏ

