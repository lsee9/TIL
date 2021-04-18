###### Start_Camp_Day7_210113

# Python_Basic1



## 컴퓨터 프로그래밍 언어란?

> 컴퓨터에게 일을 시키려면 어떻게 해야할까요? 컴퓨터의 언어로 명령을 내려야 합니다! 그러나 이를 사람이 이해하고 사용하기는 어렵습니다. 
>
> 이에 사람이 어느정도 이해할 수 있으면서 컴퓨터에 명령을 내릴 수 있도록 만들어진 것이 프로그래밍 언어입니다.
>
> 즉, 컴퓨터에게 뭔가를 시킬 때 쓰는 말로 컴퓨터와 사람 사이의 매개체 역할로 볼 수 있습니다.



## Python

> 배우기 쉬워 비전공자를 포함한 많은 사람들이 사용하는 언어로, 다양한 기능을 제공하여 프로그램을 빠르게 개발할 수 있습니다.



:raising_hand_man: 그럼 Python의 문법에 대해 알아볼까요?

### 1. 저장

#### 1.1 변수

> 어떤 데이터 여러차례 사용하고 싶다면 어떻게 해야할까요?  박스에 데이터을 넣고 이름을 붙여준다면 필요할 때 찾아서 사용할 수 있을 거에요.
>
> 컴퓨터는 메모리 공간에 데이터를 저장하는데, 필요할 때 원하는 값을 불러올 수 있도록 특정한 이름을 붙인 공간을 생성합니다. 이것을 `변수`라고 합니다.

- 다양한 데이터를 저장할 수 있는 특정 이름을 가진 메모리 공간입니다.

- 변수에 데이터를 저장한다는 것을 `변수에 값을 할당한다` 라고 합니다.

  ```python
  number = 100 #변수 number에 100을 할당한다	
  ```



​	:question: 변수에 무엇을 저장할 수 있을까요?

​		1) 숫자

​			- 존재하는 거의 모든 숫자(숫자 중간에 글자 들어가면 안됨)

​			- 기본적인 연산 가능

​		2) 글자

​			- 존재하는 거의 모든 글자

​			- 따옴표로 둘러싼 글자 or 숫자	

​		3) 참/거짓(Boolean)

​			- True/False(첫 글자는 대문자)

​			- 조건문에 사용

<실습>



#### 1.2 리스트(List)

> 학생 100명의 시험점수 평균을 내려고 합니다. 이때 점수는 어떻게 정리하는게 좋을까요? 물론 score1, score2, ... score100 이렇게 100개의 변수에 점수를 저장할 수 있습니다. 하지만 너무 번거롭고, 메모리만 잔뜩 차지하겠죠?
>
> 우리가 데이터마다 다른 이름의 박스를 사용하는 건 데이터의 특성을 구분하기위해서 이기도 합니다! 학생의 점수는 모두 같은 종류의 데이터 이죠? 이런 경우는 모두 다른 변수를 사용할 필요가 없습니다.
>
> 박스를 여러개 이어붙여 socre라고 이름붙이고, 첫번쨰 칸부터 데이터를 저장한다면 어떨까요? 같은 종류의 데이터를 더 쉽게 관리할 수 있을거에요!
>
> 이러한 형식을 가진 리스트(List)를 배워보겠습니다.

- 비슷한 타입의 여러가지 정보를 저장합니다.

- [] (대괄호)를 사용하여 표현하고, 데이터는 ,(콤마)로 구분합니다.

- index로 각 데이터에 접근 가능하며, 0부터 시작합니다.

  ```python
  dusts = [5, 8, 6, 10, 22]  #List 선언
  dusts[0]  #첫번째 인덱스의 값인 5 반환
  type(dusts)  #<calss 'list'>
  ```



<실습>



#### 1.3 딕셔너리(Dictionary)

> 사전을 떠올려보세요. 이름을 찾으면 해당하는 이름의 개념을 파악할 수 있죠! 이처럼 순서가 아니라 각 데이터에 맞는 이름을 붙여서 데이터를 관리하는 방법을 알아봅시다!
>
> 우리는 이러한 형식을 딕셔너리(Dictionary) 라고 부릅니다.

- {}(중괄호)를 사용해서 표현합니다.
- 각 데이터는 key: value

### 2. 조건(if)



### 3. 반복

#### 3.1 While



#### 3.2 for


