###### 210416_wed

##### APS 응용

<hr>

###### 오늘의 수업 목차 :stuck_out_tongue_winking_eye:

### 완전 검색 & 그리디

- 반복(Iteration)과 재귀(Recursion)
- 완전 검색 기법
- 조합적 문제 - **순열, 조합, 부분집합**
- 탐욕 알고리즘

<hr>
<br>

# 1. 반복(Iteration)과 재귀(Recursion)

- 반복과 재귀는 유사한 작업을 수행

### 반복

- 수행하는 작업이 완료될 때 까지 계속 반복
- 루프 : for, while구조

### 재귀

- 주어진 문제의 해를 구하기 위해 **동일하면서 더 작은**문제의 해를 이용하는 방법
- 하나의 큰 문제를 해결할 수 있는 더 작은 문제로 쪼개고 결과들을 결합한다
- 재귀함수로 구현 (짧은 함수는 복귀시간이 더 걸림!, 그래서 보통 반복보다 오래걸림)

<br>

## 1.1 반복

### 반복구조

- 초기화
  - 반복 명령 실행하기 전에 조건 검사에 사용할 변수 초기값 설정
- 조건 검사 (check control expression) 
  - 반복 여부 확인
- 반복할 명령문 실행 (action)
- 업데이트(loop update)
  - 무한 루프(infinite loop)가 되지 않게 조건이 거짓(false)이 되게 함

### 반복을 이용한 선택 정렬

- 선택 정렬은 구현할 수 있어야 합니다!!
  - 가장 앞에부터 작은 수를 정렬하자

```python
def SelectionSort(arr):
    N = len(arr)
    for i in range(0, N - 1):
        min_idx = i
        for j in range(i + 1, N):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[min_idx], arr[i] = arr[i], arr[min_idx]
        
```

<br>

## 1.2 재귀

### 재귀적 알고리즘

- basis case or rule
  - indection 생성을 위한 seed 역할 (재귀를 종료하는)
- inductive case or rule
  - 새로운 집합의 원소를 생성하기 위해 경합되는 방법 (재귀식 호출)

### 재귀 함수 (recursive function)

- 함수 내부에서 직접 혹은 간접적으로 **자기 자신을 호출**하는 함수
- 반복 구조에 비해 간결하고 이해하기 쉽다
- 함수 호출은 프로그램 메모리 구조에서 스택 사용
  - 따라서 재귀호출은 반복적인 스택 사용을 의미하며 메모리 및 속도에서 성능저하 발생

##### 그렇지만 완전 검색을 수행하는 것에서는 재귀가 의미있습니다!

<br>

### 팩토리얼 재귀 함수의 호출

- 재귀적 정의로 구성된 팩토리얼을 재귀함수로 구현해봅시다

```python
def fact(N):
    if N <= 1:
        return 1  #base case
    else:
        return N * fact(N - 1)  #inductive case
```

메모리 스택의 그림 추가하기



<br>

<br>

## 1.3 반복 또는 재귀?!?!

> 해결할 문제에 따라 적절한 방법을 선택합니다

- 재귀
  - 문제해결을 위한 알고리즘 설계가 간단하고 자연스업다
  - 추상 자료형(list, tree 등)의 알고리즘은 재귀적 구현이 간단하고 자연스러운 경우가 많습니다!!
  - 중위, 후위순회같은것
  - 백트래킹
- (일반적)재귀적 알고리즘의 메모리와 연산 > 반복 알고리즘의 메모리, 연산

##### :cherries: 입력 값 N이 커질수록 재귀 알고리즘은 반복에 비해 효율적일 수 있습니다!!

비교표 작성..?



<br>

<br>

# 2. 완전 검색 기법

> 모든 경우를 확인하는 완전 검색 기법을 알아봅시다!!!
>
> 매우 중요하지만 쉽지 않습니다!!

### Baby-gin Game

- 0 ~ 9사이의 숫자카드에서 임의의 6장을 뽑습니다
  - run : 3장의 카드가 연속적인 번호를 갖는 경우
  - truplet : 3장의 카드가 동일한 번호흫 갖는 경우
- baby-gin : 6장의 카드가 run 과 rtiplet으로만 이뤄진 경우!!!!

##### :baby: Baby-gin을 찾는 프로그램을 작성하자

- 예)
  - 667767 : 2개의 triplet => baby-gin
  - 101123 : 1개의 run => baby-gin X
- 오늘 수업을 듣고 어떻게 풀지 생각해봅시다@@@

<br>

## 2.1 Brute-force (고지식한 방법)

- 문제해결을 위한 간단하고 쉬운 방법!
  - Just-do-it !!!
- 대부분의 문제에 적용 가능
- 상대적으로 빠른 시간에 문제해결(알고리즘 설계)을 할 수 있다
  - 문제 해결 아이디어를 빠르게 찾을 수 있다 (설계하는 시간이 짧다)
- 문제에 포함된 자료의 크기가 작다면 유용하다
- 보통 효율성을 판단하기 위한 척도로 사용된다

### Brute-force 탐색 (sequential search)

- 자료들의 리스트에서 키 값을 찾기 위해 **첫 번째 자료부터 비교**하면서 진행
  - 즉, 처음부터 전체를 다 뒤지는 것!!!
  - 찾는 값이 배열에 있는지 없는지를 확인하고 싶은 경우 사용!!

```python
# len(arr) = n
def sequential_search(arr, k):
    idx = 0
    while arr[idx] != k:
        idx += 1
        
   	return idx if idx< n else -1  #arr의 길이보다 전에 끝나면 값을 찾은 것, 아니면 못찾은 것이므로 -1을 반환한다
```

<br>

## 2.2 완전 검색으로 시작하라

> A형 시험을 준비하는 지금!!! 효율적인 것을 원하지 않습니다!!!!
>
> 일단 답을 구해야 뭐라도 하지 않겠어요?? :sob:

- 