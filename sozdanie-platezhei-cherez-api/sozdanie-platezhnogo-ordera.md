# Создание платежного ордера

### Информация о запросе

**Метод:** `POST`\
**URL:** `/orders`

***

### Параметры запроса

#### Тело запроса (Body)

**Content-Type:** `application/json`

```json
{
  "amount": "string",
  "bank_code": "string",
  "countries": [
    "ANY"
  ],
  "currency": "USDT",
  "customer_name": "string",
  "customer_requisite": "string",
  "idempotency_key": "string",
  "merchant_order_id": "string",
  "payment_systems": [
    "OTHER"
  ],
  "requisite_type": "SBP"
}
```

#### Описание параметров

| Параметр             | Тип    | Обязательный | Описание                              |
| -------------------- | ------ | ------------ | ------------------------------------- |
| `amount`             | string | ✅            | Сумма транзакции                      |
| `requisite_type`     | string | ✅            | Тип платежного реквизита              |
| `idempotency_key`    | string | ✅            | Уникальный ключ идемпотентности       |
| `currency`           | string | ❌            | Код валюты (по умолчанию: USDT)       |
| `countries`          | array  | ❌            | Коды стран для международных платежей |
| `merchant_order_id`  | string | ❌            | Внутренний идентификатор заказа       |
| `customer_name`      | string | ❌            | Имя клиента                           |
| `customer_requisite` | string | ❌            | Платежные данные клиента              |
| `bank_code`          | string | ❌            | Код предпочтительного банка           |
| `payment_systems`    | array  | ❌            | Предпочитаемые платежные системы      |

***

### Успешный ответ

**Код:** `200 OK`

json

```
{
  "amount": "string",
  "amount_usdt": "string",
  "bank_code": "string",
  "bank_name_en": "string",
  "bank_name_ru": "string",
  "country_code": "string",
  "created_at": "string",
  "currency": "USDT",
  "customer_name": "string",
  "customer_requisite": "string",
  "exchange_rate": "string",
  "expires_at": "string",
  "merchant_commission": "string",
  "merchant_id": "string",
  "merchant_order_id": "string",
  "owner_full_name": "string",
  "payment_system": "OTHER",
  "requisite": "string",
  "requisite_attributes": {
    "phoneNumber": "+1234567890"
  },
  "requisite_type": "SBP",
  "status": "pending",
  "uuid": "string",
  "uuid_callback": "string"
}
```

***

### Возможные ошибки

#### `400 Bad Request`

json

```
{
  "error": "string"
}
```

#### `401 Unauthorized`

json

```
{
  "error": "string"
}
```

#### `404 Not Found`

json

```
{
  "error": "string"
}
```

#### `500 Internal Server Error`

json

```
{
  "error": "string"
}
```

***

### Пример использования

bash

```
curl -X POST "https://api.pandapay24.com/v1/orders" \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: YOUR_API_KEY" \
  -H "X-Signature: YOUR_SIGNATURE" \
  -H "X-Timestamp: 1759744481" \
  -d '{
    "amount": "100.50",
    "requisite_type": "SBP",
    "idempotency_key": "123e4567-e89b-12d3-a456-426614174000",
    "currency": "USDT",
    "merchant_order_id": "order_12345",
    "customer_name": "Иван Иванов"
  }'
```
