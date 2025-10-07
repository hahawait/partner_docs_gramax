---  
order: 4  
title: Каналы (channels)  
---  

## Получение списка каналов  

**GET /channels**  

Возвращает список всех каналов, зарегистрированных у партнёра.  

### Авторизация  
Требуется Bearer токен.  

---

### Ответ 200 — успешный запрос

```json
[
  {
    "channel_id": 123456789,
    "event_type": "add",
    "created_at": "2025-10-07T12:00:00Z"
  },
  {
    "channel_id": 987654321,
    "event_type": "remove",
    "created_at": "2025-09-15T08:30:00Z"
  }
]
```

**Поля:**  
- `channel_id` *(int)* — ID канала  
- `event_type` *(enum: add | remove)* — Тип события (добавление или удаление)  
- `created_at` *(datetime)* — Дата создания записи  

---

## Добавление каналов  

**POST /channels**  

Добавляет один или несколько каналов для отслеживания.  

### Авторизация  
Требуется Bearer токен.  

### Тело запроса  

Массив объектов:  

```json
[
  {
    "channel_id": 123456789,
    "event_type": "add"
  },
  {
    "channel_id": 987654321,
    "event_type": "remove"
  }
]
```

**Поля:**  
- `channel_id` *(int, обязательное)* — ID канала  
- `event_type` *(enum: add | remove, обязательное)* — Тип события  

---

### Ответ 200 — успешное добавление  

```json
{
  "successfully_added_channels": [123456789],
  "not_added_channels": [987654321]
}
```

**Поля:**  
- `successfully_added_channels` *(list[int])* — Список успешно добавленных каналов  
- `not_added_channels` *(list[int])* — Каналы, которые не удалось добавить  

---

### Ошибки  

| Код | Описание | Пример |
|------|-----------|--------|
| 422 | Ошибка валидации | `{"detail": [{"loc": ["body", 0, "channel_id"], "msg": "field required", "type": "value_error.missing"}]}` |

---

[openapi:./channels.yaml:false]
