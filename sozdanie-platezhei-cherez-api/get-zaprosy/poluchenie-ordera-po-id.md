# Получение ордера по ID

### Описание метода

**Метод:** GET\
**URL:** `/orders/{id}`

Получение информации о ордере по его UUID.

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
  "amount": 0,
  "amount_usdt": "string",
  "bank_name_en": "string",
  "bank_name_ru": "string",
  "country_code": "string",
  "created_at": "string",
  "currency": "USDT",
  "customer_name": "string",
  "customer_requisite": "string",
  "expires_at": "string",
  "merchant_order_id": "string",
  "requisite_attributes": {
    "phoneNumber": "+1234567890"
  },
  "requisite_type": "SBP",
  "requisites": "string",
  "status": "pending",
  "url_callback": "string",
  "uuid": "string",
  "uuid_callback": "string"
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
curl -X GET "https://api.pandapay24.com/v1/orders/123e4567-e89b-12d3-a456-426614174000" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```
