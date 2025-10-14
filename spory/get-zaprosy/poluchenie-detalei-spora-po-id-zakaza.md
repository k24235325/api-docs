# Получение деталей спора по ID заказа

**Метод:** GET\
**URL:** `/orders/{id}/disputes`

***

### Описание

Получает детальную информацию о споре по UUID заказа

### Параметры пути

| Параметр | Тип          | Обязательный | Описание    |
| -------- | ------------ | ------------ | ----------- |
| id       | string(uuid) | ✔            | UUID заказа |

### Ответы

#### 200 OK

```json
{
  "amount": 100.5,
  "created_at": "2023-12-01T10:00:00Z",
  "evidence": [
    {
      "created_at": "2023-12-01T10:00:00Z",
      "dispute_id": "123e4567-e89b-12d3-a456-426614174000",
      "key": "evidence_payment_receipt.jpg",
      "uploader": "merchant",
      "url": "https://example.com/evidence_payment_receipt.jpg",
      "uuid": "550e8400-e29b-41d4-a716-446655440000"
    }
  ],
  "initiator": "merchant",
  "logs": [
    {
      "actor_role": "merchant",
      "created_at": "2023-12-01T10:00:00Z",
      "details": "Dispute opened due to amount mismatch",
      "dispute_id": "123e4567-e89b-12d3-a456-426614174000",
      "status": "in_progress",
      "uuid": "550e8400-e29b-41d4-a716-446655440000"
    }
  ],
  "merchant_order_id": "MERCH_ORDER_12345",
  "modified_at": "2023-12-01T12:30:00Z",
  "order_id": "123e4567-e89b-12d3-a456-426614174000",
  "pause_duration_sec": 500,
  "paused_at": "2023-12-01T11:00:00Z",
  "reason": "payment_made",
  "reopened_at": "string",
  "status": "pending",
  "ttl_sec": 86400,
  "uuid": "550e8400-e29b-41d4-a716-446655440000"
}
```

#### 400 Bad Request

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
curl -X GET "https://api.pandapay24.com/v1/orders/123e4567-e89b-12d3-a456-426614174000/disputes" \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: YOUR_API_KEY" \
  -H "X-Signature: YOUR_SIGNATURE" \
  -H "X-Timestamp: 1759744481"
```
