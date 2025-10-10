# Отмена ордера

### Описание метода

**Метод:** GET\
**URL:** `/orders/{id}/cancel`

Отмена активного ордера. Только ордера со статусом 'pending' могут быть отменены.

### Параметры запроса

#### Параметры пути (Path Parameters)

| Параметр | Тип    | Обязательный | Описание    |
| -------- | ------ | ------------ | ----------- |
| id       | string | ✅            | UUID ордера |

### Успешный ответ

**Код:** 200 OK\
**Content-Type:** application/json

json

```
{
  "status": "pending",
  "uuid": "string"
}
```

### Возможные ошибки

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

#### 404 Not Found

json

```
{
  "status": "pending",
  "uuid": "string"
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
curl -X GET "https://api.pandapay24.com/v1/orders/123e4567-e89b-12d3-a456-426614174000/cancel" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```
