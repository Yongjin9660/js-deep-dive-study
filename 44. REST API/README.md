# 44. REST API

#### \*\* **REST**

> π‘ RESTλ HTTP/1.0κ³Ό 1.1μ μ€ν μμ±μ μ°Έμ¬νκ³  μνμΉ HTTP μλ² νλ‘μ νΈμ κ³΅λ μ€λ¦½μμΈ λ‘μ΄ νλ©μ 2000λ λΌλ¬Έμμ μ²μ μκ°

<br>

- HTTP λ₯Ό κΈ°λ°μΌλ‘ ν΄λΌμ΄μΈνΈκ° μλ²μ λ¦¬μμ€μ μ κ·Όνλ λ°©μμ κ·μ ν μν€νμ²
- `RESTful` : REST μ κΈ°λ³Έ μμΉμ μ±μ€ν μ§ν¨ μλΉμ€ λμμΈ

<br>

#### \*\* **REST API**

- REST λ₯Ό κΈ°λ°μΌλ‘ μλΉμ€ API λ₯Ό κ΅¬νν κ²

<br>

## 44.1 REST API μ κ΅¬μ±

- REST API λ **μμ**, **νμ**, **νν** μ΄ 3κ°μ§ μμλ‘ κ΅¬μ±
- REST λ `μμ²΄ νν κ΅¬μ‘°` λ‘ κ΅¬μ±λμ΄ REST API λ§μΌλ‘ HTTP μμ²­μ λ΄μ© μ΄ν΄ κ°λ₯

<br>

| κ΅¬μ± μμ            | λ΄μ©                           | νν λ°©λ²        |
| -------------------- | ------------------------------ | ---------------- |
| μμ(Resource)       | μμ                           | URI(μλν¬μΈνΈ)  |
| νμ(Verb)           | μμμ λν νμ               | HTTP μμ²­ λ©μλ |
| νν(Representation) | μμμ λν νμμ κ΅¬μ²΄μ  λ΄μ© | νμ΄λ‘λ         |

<br>

## 44.2 REST API μ€κ³ μμΉ

#### 1) URI λ λ¦¬μμ€λ₯Ό ννν΄μΌ νλ€.

```jsx
// bad
GET / getTodos / 1;
GET / todos / show / 1;

// good
GET / todos / 1;
```

- `URI λ λ¦¬μμ€λ₯Ό νννλ λ° μ€μ μ λμ΄μΌ ν¨.`
  - λ¦¬μμ€λ₯Ό μλ³ν  μ μλ λμ¬λ³΄λ€λ **λͺμ¬**λ₯Ό μ¬μ©ν  κ²
  - λ°λΌμ μ΄λ¦μ get κ°μ νμμ λν νν X

<br>

#### 2) λ¦¬μμ€μ λν νμλ HTTP μμ²­ λ©μλλ‘ νννλ€.

- **HTTP μμ²­ λ©μλ**λ `ν΄λΌμ΄μΈνΈκ° μλ²μκ² μμ²­μ μ’λ₯μ λͺ©μ  (λ¦¬μμ€μ λν νμ) μ μλ¦¬λ λ°©λ²`
- μ£Όλ‘ 5κ°μ§ μμ²­ λ©μλ (GET, POST, PUT, PATCH, DELETE λ±) λ₯Ό μ¬μ©νμ¬ CRUD κ΅¬ν

<br>

| HTTP μμ²­ λ©μλ | μ’λ₯           | λͺ©μ                   | νμ΄λ‘λ |
| ---------------- | -------------- | --------------------- | :------: |
| GET              | index/retrieve | λͺ¨λ /νΉμ  λ¦¬μμ€ μ·¨λ |    X     |
| POST             | create         | λ¦¬μμ€ μμ±           |    O     |
| PUT              | replace        | λ¦¬μμ€μ μ μ²΄ κ΅μ²΄    |    O     |
| PATCH            | modify         | λ¦¬μμ€μ μΌλΆ μμ     |    O     |
| DELETE           | delete         | λͺ¨λ /νΉμ  λ¦¬μμ€ μ­μ  |    X     |

<br>

```jsx
// bad
GET /todos/delete/1

// good
DELETE /todos1
```

- `λ¦¬μμ€μ λν νμλ HTTP μμ²­ λ©μλλ₯Ό ν΅ν΄ νννλ©° URI μ νν X`
  - λ¦¬μμ€λ₯Ό μ·¨λν κ²½μ°μλ GET, λ¦¬μμ€λ₯Ό μ­μ νλ κ²½μ°μλ DELETE λ₯Ό μ¬μ©νμ¬ λ¦¬μμ€μ λν νμλ₯Ό λͺνν ννν κ²
    <br>
