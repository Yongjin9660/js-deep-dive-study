# 44. REST API

> 💡 REST는 HTTP/1.0과 1.1의 스펙 작성에 참여했고 아파치 HTTP 서버 프로젝트의 공동 설립자인 로이 필딩의 2000년 논문에서 처음 소개

- HTTP의 장점을 최대한 활용할 수 있는 아키텍처로서 REST를 소개했고
- 이는 HTTP 프로토콜을 의도에 맞게 디자인하도록 유도

`REST`: HTTP를 기반으로 클라이언트가 서버의 리소스에 접근하는 방식을 규정한 아키텍처
`RESTful`: REST의 기본 원칙을 성실히 지킨 서비스 디자인
`REST API`: REST를 기반으로 서비스 API를 구현한 것을 의미

## 44.1 REST API의 구성

> 💡 REST API는 자원(Resource), 행위(Verb), 표현(Representation)의 3가지 요소로 구성

- 자체 표현 구조(self-descriptiveness)로 구성되어 REST API만으로 HTTP 요청의 내용 이해 가능

| 구성 요소            | 내용                           | 표현 방법        |
| -------------------- | ------------------------------ | ---------------- |
| 자원(Resource)       | 자원                           | URI(엔드포인트)  |
| 행위(Verb)           | 자원에 대한 행위               | HTTP 요청 메서드 |
| 표현(Representation) | 자원에 대한 행위의 구체적 내용 | 페이로드         |

## 44.2 REST API 설계 원칙

> 💡 **URI는 리소스를 표현**하는 데 집중하고 **행위에 대한 정의는 HTTP 요청 메서드**를 통해 설계

**1. URI는 리소스를 표현**

- 리소스를 식별할 수 있는 이름은 동사보다는 명사를 사용

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

**2. 리소스에 대한 행위는 HTTP 요청 메서드로 표현**

- HTTP 요청 메서드는 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 방법

| HTTP 요청 메서드 | 종류           | 목적                  | 페이로드 |
| ---------------- | -------------- | --------------------- | :------: |
| GET              | index/retrieve | 모든/특정 리소스 취득 |    X     |
| POST             | create         | 리소스 생성           |    O     |
| PUT              | replace        | 리소스의 전체 교체    |    O     |
| PATCH            | modify         | 리소스의 일부 수정    |    O     |
| DELETE           | delete         | 모든/특정 리소스 삭제 |    X     |

- 리소스에 대한 행위는 HTTP 요청 메서드를 통해 표현. URI에 표현 X

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
