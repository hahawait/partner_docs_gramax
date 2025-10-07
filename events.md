---  
order: 5
title: События (events)
---  

## Получение событий подписок

**GET /events/sub**

### Авторизация  
Требуется Bearer токен. 

### 🧩 Параметры запроса

| Параметр | Тип | Обязательный | Описание | Пример |
|-----------|-----|---------------|-----------|---------|
| `offset` | integer | ❌ | Смещение для пагинации (по умолчанию 0) | 0 |
| `limit` | integer | ❌ | Кол-во записей для выборки (по умолчанию 100) | 100 |
| `channels_id` | array(integer) / null | ❌ | ID каналов для фильтрации | [123, 456] |
| `date_gte` | string (date-time) | ✅ | Начало диапазона дат | "2025-03-18 08:00:10" |
| `date_lte` | string (date-time) | ❌ | Конец диапазона дат | "2025-03-18 08:30:10" |

### ✅ Успешный ответ — 200 OK
```json
{
  "items": [
    {
      "oid": "uuid",
      "channel_id": 123,
      "user_id": 456,
      "event_status": "sub",
      "invite_link": "https://t.me/joinchat/abcd",
      "is_premium": 1,
      "is_username": 0,
      "created_at": "2025-03-18T08:15:00Z"
    }
  ],
  "pagination": {
    "offset": 0,
    "limit": 100,
    "total": 1234
  }
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

## Получение событий отписок

**GET /events/unsub**

Получение списка событий *отписок* за указанный период.

### Авторизация  
Требуется Bearer токен. 

### 🧩 Параметры запроса

| Параметр | Тип | Обязательный | Описание | Пример |
|-----------|-----|---------------|-----------|---------|
| `offset` | integer | ❌ | Смещение для пагинации (по умолчанию 0) | 0 |
| `limit` | integer | ❌ | Кол-во записей для выборки (по умолчанию 100) | 100 |
| `channels_id` | array(integer) / null | ❌ | ID каналов для фильтрации | [789, 101] |
| `date_gte` | string (date-time) | ✅ | Начало диапазона дат | "2025-03-18 08:00:10" |
| `date_lte` | string (date-time) | ❌ | Конец диапазона дат | "2025-03-18 08:30:10" |

### ✅ Успешный ответ — 200 OK
```json
{
  "items": [
    {
      "oid": "uuid",
      "channel_id": 999,
      "user_id": 123,
      "event_status": "unsub",
      "invite_link": null,
      "is_premium": 0,
      "is_username": 1,
      "created_at": "2025-03-18T08:10:00Z"
    }
  ],
  "pagination": {
    "offset": 0,
    "limit": 100,
    "total": 456
  }
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

## 📘 Схемы

### **EventOut**
| Поле | Тип | Описание |
|-------|-----|-----------|
| `oid` | string | Уникальный идентификатор события |
| `channel_id` | integer | ID канала |
| `user_id` | integer | ID пользователя |
| `event_status` | string | Статус события (`sub` / `unsub`) |
| `invite_link` | string / null | Ссылка-приглашение |
| `is_premium` | integer | Признак премиум-пользователя |
| `is_username` | integer | Признак наличия username |
| `created_at` | string (date-time) | Время создания события |

### **PaginationOut**
| Поле | Тип | Описание |
|-------|-----|-----------|
| `offset` | integer | Смещение |
| `limit` | integer | Количество элементов на страницу |
| `total` | integer | Общее количество элементов |

### **HTTPValidationError**
| Поле | Тип | Описание |
|-------|-----|-----------|
| `detail` | array[ValidationError] | Подробности ошибки |

### **ValidationError**
| Поле | Тип | Описание |
|-------|-----|-----------|
| `loc` | array[string/integer] | Местоположение ошибки |
| `msg` | string | Сообщение об ошибке |
| `type` | string | Тип ошибки |

[openapi:./events.yaml:false]
