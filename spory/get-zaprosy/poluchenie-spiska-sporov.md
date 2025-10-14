# Получение списка споров

**Метод:** GET\
**URL:** `/disputes`

***

### Описание

Получает постраничный список споров с фильтрацией по статусу и ID мерчанта

### Параметры запроса

#### Query Parameters

| Параметр  | Тип     | Обязательный | Описание                         | Доступные значения                                             |
| --------- | ------- | ------------ | -------------------------------- | -------------------------------------------------------------- |
| page      | integer | ✔            | Номер страницы                   | -                                                              |
| per\_page | integer | ✔            | Количество элементов на странице | -                                                              |
| status    | string  |              | Фильтр по статусу спора          | `in_progress`, `paused`, `resolved_success`, `resolved_cancel` |

### Ответы

#### 200 OK

```json
{
  "disputes": [
    {
      "amount": 100.5,
      "created_at": "2023-12-01T10:00:00Z",
      "initiator": "merchant",
      "merchant_id": "456e7890-e12b-34c5-d678-901234567890",
      "modified_at": "2023-12-01T12:30:00Z",
      "order_id": "123e4567-e89b-12d3-a456-426614174000",
      "pause_duration_sec": 500,
      "paused_at": "2023-12-01T11:00:00Z",
      "reason": "payment_made",
      "reopened_at": "2023-12-01T11:30:00Z",
      "status": "in_progress",
      "ttl_sec": 1800,
      "uuid": "550e8400-e29b-41d4-a716-446655440000"
    }
  ],
  "has_more": true
}
```

#### 400 Bad Request

json

```
{
  "error": "string"
}
```

#### 401 Unauthorized

json

```
{
  "error": "string"
}
```

#### 500 Internal Server Error

json

```
{
  "error": "string"
}
```

### Пример использования

bash

```
curl -X GET "https://api.pandapay24.com/v1/disputes?page=1&per_page=10&status=in_progress" \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: YOUR_API_KEY" \
  -H "X-Signature: YOUR_SIGNATURE" \
  -H "X-Timestamp: 1759744481"
```
