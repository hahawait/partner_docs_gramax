---
order: 2
title: Аутентификация (auth)
---

## Вход в систему

Для входа пользователя используйте эндпоинт:

**POST /auth/login**

В теле запроса передавайте один из идентификаторов (email или phone) и пароль:

```json
{
  "email": "user@example.com",
  "phone": null,
  "password": "your_password"
}
```

**Ответ 200** — возвращает access и refresh токены:

```json
{
  "access_token": "string",
  "refresh_token": "string",
  "token_type": "Bearer"
}
```

**Ошибки:**
- 404 — пользователь не найден
- 409 — некорректный пароль
- 422 — ошибка валидации

---

## Обновление токенов (refresh)

Для обновления access/refresh токенов используйте эндпоинт:

**POST /auth/refresh**

В теле запроса передавайте refresh_token:

```json
{
  "refresh_token": "string"
}
```

**Ответ 200** — возвращает новые access и refresh токены:

```json
{
  "access_token": "string",
  "refresh_token": "string",
  "token_type": "Bearer"
}
```

**Ошибки:**
- 401 — некорректный или истёкший токен
- 404 — пользователь не найден
- 422 — ошибка валидации

---

[openapi:./auth.yaml:false]