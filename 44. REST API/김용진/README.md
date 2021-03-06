# 44. REST API

> π‘ RESTλ HTTP/1.0κ³Ό 1.1μ μ€ν μμ±μ μ°Έμ¬νκ³  μνμΉ HTTP μλ² νλ‘μ νΈμ κ³΅λ μ€λ¦½μμΈ λ‘μ΄ νλ©μ 2000λ λΌλ¬Έμμ μ²μ μκ°

- HTTPμ μ₯μ μ μ΅λν νμ©ν  μ μλ μν€νμ²λ‘μ RESTλ₯Ό μκ°νκ³ 
- μ΄λ HTTP νλ‘ν μ½μ μλμ λ§κ² λμμΈνλλ‘ μ λ

`REST`: HTTPλ₯Ό κΈ°λ°μΌλ‘ ν΄λΌμ΄μΈνΈκ° μλ²μ λ¦¬μμ€μ μ κ·Όνλ λ°©μμ κ·μ ν μν€νμ²
`RESTful`: RESTμ κΈ°λ³Έ μμΉμ μ±μ€ν μ§ν¨ μλΉμ€ λμμΈ
`REST API`: RESTλ₯Ό κΈ°λ°μΌλ‘ μλΉμ€ APIλ₯Ό κ΅¬νν κ²μ μλ―Έ

## 44.1 REST APIμ κ΅¬μ±

> π‘ REST APIλ μμ(Resource), νμ(Verb), νν(Representation)μ 3κ°μ§ μμλ‘ κ΅¬μ±

- μμ²΄ νν κ΅¬μ‘°(self-descriptiveness)λ‘ κ΅¬μ±λμ΄ REST APIλ§μΌλ‘ HTTP μμ²­μ λ΄μ© μ΄ν΄ κ°λ₯

| κ΅¬μ± μμ            | λ΄μ©                           | νν λ°©λ²        |
| -------------------- | ------------------------------ | ---------------- |
| μμ(Resource)       | μμ                           | URI(μλν¬μΈνΈ)  |
| νμ(Verb)           | μμμ λν νμ               | HTTP μμ²­ λ©μλ |
| νν(Representation) | μμμ λν νμμ κ΅¬μ²΄μ  λ΄μ© | νμ΄λ‘λ         |

## 44.2 REST API μ€κ³ μμΉ

> π‘ **URIλ λ¦¬μμ€λ₯Ό νν**νλ λ° μ§μ€νκ³  **νμμ λν μ μλ HTTP μμ²­ λ©μλ**λ₯Ό ν΅ν΄ μ€κ³

**1. URIλ λ¦¬μμ€λ₯Ό νν**

- λ¦¬μμ€λ₯Ό μλ³ν  μ μλ μ΄λ¦μ λμ¬λ³΄λ€λ λͺμ¬λ₯Ό μ¬μ©

```
# bad
GET /getTodos/1
GET /todos/show/1

# good
GET /todos/1
```

**2. λ¦¬μμ€μ λν νμλ HTTP μμ²­ λ©μλλ‘ νν**

- HTTP μμ²­ λ©μλλ ν΄λΌμ΄μΈνΈκ° μλ²μκ² μμ²­μ μ’λ₯μ λͺ©μ (λ¦¬μμ€μ λν νμ)μ μλ¦¬λ λ°©λ²

| HTTP μμ²­ λ©μλ | μ’λ₯           | λͺ©μ                   | νμ΄λ‘λ |
| ---------------- | -------------- | --------------------- | :------: |
| GET              | index/retrieve | λͺ¨λ /νΉμ  λ¦¬μμ€ μ·¨λ |    X     |
| POST             | create         | λ¦¬μμ€ μμ±           |    O     |
| PUT              | replace        | λ¦¬μμ€μ μ μ²΄ κ΅μ²΄    |    O     |
| PATCH            | modify         | λ¦¬μμ€μ μΌλΆ μμ     |    O     |
| DELETE           | delete         | λͺ¨λ /νΉμ  λ¦¬μμ€ μ­μ  |    X     |

- λ¦¬μμ€μ λν νμλ HTTP μμ²­ λ©μλλ₯Ό ν΅ν΄ νν. URIμ νν X

```
# bad
GET /todos/delete/1

# good
DELETE /todos/1
```
