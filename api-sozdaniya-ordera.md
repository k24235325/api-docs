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

## Тестирование API

### Способ 1: Используйте Postman

[![Run in Postman](https://run.pstmn.io/button.svg)](https://your-postman-collection-link)

### Способ 2: Используйте наш Sandbox

Перейдите на: https://sandbox.yourdomain.com\
Для тестирования используйте тестовый ключ: `test_api_key_12345`

### Способ 3: Пример кода

```atom
// Скопируйте этот код для тестирования
const response = await fetch('https://api.yourdomain.com/endpoint', {
  headers: {
    'x-api-key': 'your_api_key_here'
  }
});
```

## Тестирование API

📋 **Используйте наш интерактивный Sandbox:**

[👉 Перейти к интерактивному тестеру API](https://sandbox.yourdomain.com)

Там вы сможете:

* Вводить свой API ключ
* Отправлять реальные запросы
* Видеть ответы в реальном времени

API TesterSEND API REQUEST

```
имормромро
```

```
<script>
    async function sendRequest() {
        const apiKey = document.getElementById('apiKey').value;
        const response = await fetch('https://api.yourdomain.com/endpoint', {
            headers: { 'x-api-key': apiKey }
        });
        const data = await response.json();
        document.getElementById('response').textContent = JSON.stringify(data, null, 2);
    }
</script>
```
