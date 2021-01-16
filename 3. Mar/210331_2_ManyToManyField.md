###### 210331_wed

##### Model Relationship 3

# ManyToManyField

> django에서 제공하는 필드를 사용해봅시다

- `M : N 관계`를 나타내기 위해 사용하는 필드
- 필수 위치 인자
  - **M : N 관계로 설정할 모델 클래스**

- 필드의 RelatedManager를 사용하여 관련 개체를 추가, 제거, 생성할 수 있다

<br>

## 1. Related manager

- 1 : N or M : N 관련 컨텍스트에 사용되는 매니저

### methods

- 같은 이름의 메서드여도 관계에 따라 다르게 동작합니다
- 1 : N
  - `target` model 객체만 사용 가능
- M : N
  - 관련된 `두 객체(target, source)`에서 모두 사용 가능

- 종류
  - `add()`, create(), `remove()`, clear(), set()

#### target VS source

- `target` : source model이 관계 필드를 통해 참조하는 모델
- `source` : 관계에 대한 필드를 가진 모델

- Article(1) : Comment(N)
  - target : article
  - source : comment

<br>

<br>

## 2. ManyToManyFiels's Arguments

> 관계가 작동하는 방식을 제어하는 것으로, optional합니다

- related_name
- symmetrical
- through
- 나머지는 문서 찾아보세용

### 2.1 related_name

> ForeginKey에서와 동일하게 동작합니다

- target model이 source model(or instance)을 참조할 때 사용하는 manager name을 성정합니다

### 2.2 symmetrical

- 동일한 모델(자기자신)을 참조하는 경우 사용 (**재귀적**)

- default : `symmetric=True`

  - source instance가 target instance를 참조하면, target instance도 source instance를 참조합니다

  - 즉, 한쪽에서 참조하면 레코드가 양쪽에 모두 생깁니다

    예시) 팔로우하면 바로 맞팔됨

- `False` 설정

  - self와의 M : N 관계에서 대칭을 원하지 않는 경우 설정

    예시) A가 B를 팔로우해도 B는 A를 팔로우하지 않습니다

## 2.3 through

- **중개테이블을 직접 지정**하는 경우
  - through옵션을 통해 중개테이블을 나타내는 django 모델을 지정
- 일반적으로 **추가데이터를 다대다 관계와 연결하는 경우**(extra data with a many-tomany relationship)에 사용

<br>

<br>

## 3. Related manager's methods

- `add()`
  - 지정된 객체를 관련 객체 집합에 추가
  - 이미 존재하는 관계에 다시 사용하면 관계가 복제되지 않습니다(같은 예약이 또 생기지 않습니다)
- `remove()`
  - 관련 객체 집합에서 지정된 모델 객체를 제거 (관계가 끊어짐)

<br>

<br>

## 4. 중개 테이블 필드 생성 규칙

1. **source model** 및 **target model**이 `다른 경우`
   - id
   - <source_model>_id
   - <target_model>_id
2. ManyToManyField가 `동일한 모델`을 가리키는 경우
   - id
   - from\_\<model\>\_id
   - to\_\<model\>\_id