---
description: >-
  API создания ордера, с примерами разных бизнес-запросов (моно-банк, трансгран
  и тд). Подробное описание каждого типа реквизита и поля
---

# API создания ордера

## Get Payment Order Details

`GET /api/v1/payments/{payment_id}`

This endpoint retrieves detailed information about a specific payment order.

### Request

#### HEADER PARAMETERS

* **Authorization** `string` **REQUIRED**\
  Your Bearer token for authentication.\
  Example: `Bearer your_token_here`

#### PATH PARAMETERS

* **payment\_id** `string` **REQUIRED**\
  Unique identifier of the payment order

***

### Response

Returns detailed payment order information.

**SCHEMA**

```json
{
  "payment_id": "string",
  "status": "pending|completed|failed",
  "amount": 10000,
  "currency": "UAH",
  "sender_account": "string",
  "recipient_account": "string",
  "created_at": "2023-11-21T10:00:00Z",
  "updated_at": "2023-11-21T10:00:00Z"
}
```
