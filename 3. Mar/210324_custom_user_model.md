###### 210322_mon

# Custom User Model

> user model을 커스텀해서 사용합시다!!

<br>

# 1. Customizing authentication in Django

> Django에서 커스텀하는 방법!!! 알아봅시다

## 1.1 Substituting a custom User model

> 커스텀 유저 모델로 대체하기!!

### Why Customizing?

- 일부 프로젝트에서는 built-in User model이 제공하는 인증 요구사항이 적합하지 않을 수 있음
  - 기본은 username을 사용해 인증
  - 인증에 이메일을 사용하고 싶은 경우! 만들어줘야함

### How?

- `AUTH_USER_MODEL` 설정 제공
  - 기본 user model을 재정의할 수 있게 함
  - 완전히 새로 만드는게 아닌, 기본 모델을 **상속받아 재정의**(override, 덮어씌움)
- 새 프로젝트를 시작하는 경우 커스텀 유저 모델 설정을 **강력히 권장**