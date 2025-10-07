---  
order: 3  
title: Инвайт-ссылки (links)  
---  

## Создание инвайт-ссылки  

**POST /links**  

Создает новую пригласительную ссылку для указанного канала.  

### Авторизация
Требуется Bearer токен.

### Параметры запроса

| Параметр | Обязательный | Тип | Описание |
|-----------|---------------|-----|-----------|
| `external_id` | ✅ | integer | Внешний ID партнёра |

### Тело запроса

```json
{
  "channel_id": 123456789,
  "name": "Новая ссылка для рекламы",
  "creates_join_request": false,
  "expire_date": "2025-12-31T23:59:59Z",
  "member_limit": 100
}
```

**Поля:**
- `channel_id` *(int, обязательное)* — ID канала Telegram
- `name` *(string, обязательное)* — Название ссылки
- `creates_join_request` *(bool, default=false)* — Создавать ли join-запросы
- `expire_date` *(datetime или null)* — Дата истечения действия ссылки
- `member_limit` *(int или null)* — Лимит участников

---

### Ответ 201 — успешное создание

```json
{
  "id": "abc123",
  "name": "Новая ссылка для рекламы",
  "channel_id": 123456789,
  "invite_link": "https://t.me/+abcdef12345",
  "creates_join_request": false,
  "expire_date": "2025-12-31T23:59:59Z",
  "member_limit": 100,
  "created_at": "2025-10-07T12:00:00Z",
  "updated_at": null,
  "is_bot_created": true,
  "creator_id": 456,
  "budget": 0,
  "tags": [
    {
      "id": "uuid-tag-1",
      "name": "Реклама",
      "color": "#FF0000",
      "type": {
        "id": "uuid-type-1",
        "name": "category"
      }
    }
  ]
}
```

---

### Ошибки  

| Код | Описание | Пример |
|------|-----------|--------|
| 400 | Некорректное имя партнёра | `{"detail": "Partner title incorrect or does not exist"}` |
| 401 | Аккаунт не существует | `{"detail": "Account doesn't exist"}` |
| 403 | Нет доступа или недостаточно прав | `{"detail": "У бота нет прав на выполнение данной операции в канале -1009410"}` |
| 404 | Партнёр не найден | `{"detail": "Partner with id=123 not found"}` |
| 409 | Конфликт при добавлении бота | `{"detail": "Бот с helper не добавлен в канал -1009410"}` |
| 422 | Ошибка валидации | `{"detail": [{"loc": ["body", "field"], "msg": "field required", "type": "value_error.missing"}]}` |

---

## Получение всех инвайт-ссылок  

**POST /links/all**

Возвращает список всех инвайт-ссылок для конкретного канала и партнёра.  

### Авторизация  
Требуется Bearer токен.  

### Параметры запроса  

| Параметр | Обязательный | Тип | По умолчанию | Описание |
|-----------|---------------|-----|---------------|-----------|
| `external_id` | ✅ | integer | — | Внешний ID партнёра |
| `channel_id` | ✅ | integer | — | ID канала |
| `page` | ❌ | integer | 1 | Номер страницы |
| `limit` | ❌ | integer | 10 | Количество элементов на странице |

---

### Ответ 200 — успешный запрос

```json
{
  "page": 1,
  "limit": 10,
  "total": 2,
  "items": [
    {
      "id": "link1",
      "name": "Рекламная ссылка #1",
      "channel_id": 12345,
      "invite_link": "https://t.me/+abc",
      "creates_join_request": false,
      "expire_date": null,
      "member_limit": null,
      "created_at": "2025-10-01T10:00:00Z",
      "updated_at": null,
      "is_bot_created": true,
      "creator_id": 789,
      "budget": 0,
      "tags": []
    },
    {
      "id": "link2",
      "name": "Рекламная ссылка #2",
      "channel_id": 12345,
      "invite_link": "https://t.me/+def",
      "creates_join_request": true,
      "expire_date": "2025-12-31T23:59:59Z",
      "member_limit": 50,
      "created_at": "2025-09-30T12:00:00Z",
      "updated_at": "2025-10-01T09:00:00Z",
      "is_bot_created": false,
      "creator_id": null,
      "budget": 1000,
      "tags": []
    }
  ]
}
```

---

### Ошибки  

Такие же, как и у `/links`:
- 400 — некорректный партнёр
- 401 — аккаунт не существует
- 403 — нет прав доступа
- 404 — партнёр не найден
- 409 — конфликт при доступе
- 422 — ошибка валидации

---

[openapi:./links.yaml:false]
