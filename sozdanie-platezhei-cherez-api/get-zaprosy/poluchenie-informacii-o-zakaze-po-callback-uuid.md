# Получение информации о заказе по callback UUID

## Информация о запросе

**Метод:** GET\
**URL:** `/orders/callback/{callback_uuid}`

***

### Описание

Возвращает информацию о заказе с использованием callback UUID. Этот эндпоинт может быть использован для получения деталей заказа в процессе оплаты.

Callback UUID - это уникальный идентификатор, связанный с каждым заказом, который может быть использован для получения информации о заказе без необходимости аутентификации. Это полезно для вебхук-колбэков и подтверждений оплаты от внешних сервисов.

### Параметры пути

| Параметр        | Тип          | Обязательный | Описание                           |
| --------------- | ------------ | ------------ | ---------------------------------- |
| `callback_uuid` | string(uuid) | ✅            | Callback UUID, связанный с заказом |

### Ответы

#### 200 - Информация о заказе успешно получена

json

```
{
  "amount_rub": 1000,
  "confirmed_by_customer": true,
  "created_at": "2024-02-07T15:04:05Z",
  "currency": "RUB",
  "customer_name": "John Doe",
  "customer_requisite": "40817810155191164626",
  "dark_qr_code_url": "https://example.com/dark_qr.png",
  "light_qr_code_url": "https://example.com/light_qr.png",
  "payment_redirect_url": "https://merchant.com/redirect",
  "receipt": "https://receipt.url",
  "requisite_info": {
    "attributes": {
      "phoneNumber": "+1234567890"
    },
    "bank_code": "AkbarsBank",
    "bank_name": "string",
    "owner_full_name": "string",
    "requisites": "1234123412341234",
    "type": "SBP"
  },
  "status": "pending",
  "ttl_sec": 0,
  "url_callback": "string",
  "uuid": "string",
  "uuid_callback": "string"
}
```

**Описание полей ответа:**

| Поле                                    | Тип               | Описание                           |
| --------------------------------------- | ----------------- | ---------------------------------- |
| `amount_rub`                            | integer           | Сумма заказа в рублях              |
| `confirmed_by_customer`                 | boolean           | Подтвержден ли заказ клиентом      |
| `created_at`                            | string(date-time) | Дата и время создания заказа       |
| `currency`                              | string            | Валюта заказа                      |
| `customer_name`                         | string            | Имя клиента                        |
| `customer_requisite`                    | string            | Реквизиты клиента                  |
| `dark_qr_code_url`                      | string            | URL темного QR-кода                |
| `light_qr_code_url`                     | string            | URL светлого QR-кода               |
| `payment_redirect_url`                  | string            | URL для редиректа платежа          |
| `receipt`                               | string            | URL чека                           |
| `requisite_info`                        | object            | Информация о реквизитах            |
| `requisite_info.attributes`             | object            | Дополнительные атрибуты реквизитов |
| `requisite_info.attributes.phoneNumber` | string            | Номер телефона                     |
| `requisite_info.bank_code`              | string            | Код банка                          |
| `requisite_info.bank_name`              | string            | Название банка                     |
| `requisite_info.owner_full_name`        | string            | Полное имя владельца               |
| `requisite_info.requisites`             | string            | Реквизиты                          |
| `requisite_info.type`                   | string            | Тип реквизитов                     |
| `status`                                | string            | Статус заказа                      |
| `ttl_sec`                               | integer           | Время жизни в секундах             |
| `url_callback`                          | string            | URL для callback                   |
| `uuid`                                  | string            | UUID заказа                        |
| `uuid_callback`                         | string            | Callback UUID заказа               |

***

## Возможные ошибки

**400 Bad Request**

json

```
{
  "error": "Invalid callback UUID format"
}
```

**404 Not Found**

json

```
{
  "error": "Order not found"
}
```

**500 Internal Server Error**

json

```
{
  "error": "Internal server error"
}
```

### Пример использования

bash

```
curl -X GET "https://api.pandapay24.com/orders/callback/123e4567-e89b-12d3-a456-426614174000" \
  -H "X-Api-Key: YOUR_API_KEY" \
  -H "X-Signature: YOUR_SIGNATURE" \
  -H "X-Timestamp: 1759744481"
```

### Примечания

* Для использования этого эндпоинта не требуется аутентификация
* Callback UUID предоставляется при создании заказа
* Эндпоинт особенно полезен для интеграции с платежными системами и вебхуками
