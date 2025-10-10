---
order: 2
title: Аутентификация (auth)
---

## Регистрация нового партнера

**POST /auth/registration**

Создает нового партнера и возвращает его данные.

### Тело запроса
```json
{
  "title": "string",
  "account_id": 123
}
```

**Поля:**
- `title` *(string, обязательное)* — Название партнера
- `account_id` *(int, обязательное)* — ID создателя партнера


### ✅ Успешный ответ — 201 OK
```json
{
  "id": "uuid",
  "client_secret": "string",
  "title": "string",
  "account_id": 123
}
```

### ⚠️ Ошибка — 422 Validation Error
```json
{
  "detail": [
    {
      "loc": ["query", "date_gte"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

---

## Создание партнерского токена

**POST /auth/token**

### Тело запроса
```json
{
  "id": "uuid",
  "client_secret": "uuid"
}
```

**Поля:**
- `id` *(UUID, обязательное)* — ID партнера
- `client_secret` *(UUID, обязательное)* — Secret партнера


### ✅ Успешный ответ — 200 OK

Возвращает *access* токен, срок жизни - 1 месяц.

```json
{
  "access_token": "string"
}
```
### Ошибки  

| Код | Описание | Пример |
|------|-----------|--------|
| 404 | Партнёр не найден | `{"detail": "Partner with id=123 not found"}` |
| 409 | Некорректный ключ аутентификации | `{"detail": "Некорректный ключ аутентификации"}` |
| 422 | Ошибка валидации | `{"detail": [{"loc": ["body", "field"], "msg": "field required", "type": "value_error.missing"}]}` |

---

## Получение ссылки для аутентификации партнера

**GET /auth/auth-link**  

### Авторизация  
Требуется Bearer токен.  

### 🧩 Параметры запроса
- `external_id`: integer (обязательный)

### ✅ Успешный ответ — 200 OK
Вовзращает ссылку.

### ⚠️ Ошибка — 422 Validation Error
```json
{
  "detail": [
    {
      "loc": ["query", "date_gte"],
      "msg": "field required",
      "type": "value_error.missing"
    }
  ]
}
```

---

## Подключение пользователя партнера

**POST /auth/connect**  

### Авторизация  
Требуется Bearer токен.  

### 🧩 Параметры запроса
- `external_id`: integer (обязательный)  
- `account_id`: integer (обязательный)

### ✅ Успешный ответ — 201 OK
```json
{
  "message": "string"
}
```

### Ошибки

| Код | Описание | Пример |
|------|-----------|--------|
| 400 | Некорректные данные | `{"detail": "Partner title incorrect or does not exist"}` |
| 404 | Партнёр не найден | `{"detail": "Partner with id=123 not found"}` |
| 409 | Пользователь уже подключен | `{"detail": "Account already exists"}` |
| 422 | Ошибка валидации | `{"detail": [{"loc": ["body", "field"], "msg": "field required", "type": "value_error.missing"}]}` |

---

## Проверка существования интеграции партнера

**GET /auth/check-partner**  

### Авторизация  
Требуется Bearer токен.  

### 🧩 Параметры запроса
- `external_id`: integer (обязательный)

### ✅ Успешный ответ — 200 OK
```json
{
  "is_integration_exist": true
}
```

### Ошибки

| Код | Описание | Пример |
|------|-----------|--------|
| 400 | Некорректные данные | `{"detail": "Partner title incorrect or does not exist"}` |
| 404 | Партнёр не найден | `{"detail": "Partner with id=123 not found"}` |
| 422 | Ошибка валидации | `{"detail": [{"loc": ["body", "field"], "msg": "field required", "type": "value_error.missing"}]}` |

[openapi:./partner_auth.yaml:false]