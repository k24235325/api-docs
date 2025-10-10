# Получение ордеров по ID заказа мерчанта

### Описание метода

**Метод:** GET\
**URL:** `/orders/merchant/{id}`

Получение списка ордеров, связанных с указанным идентификатором заказа мерчанта.

### Параметры запроса

#### Параметры пути (Path Parameters)

| Параметр | Тип    | Обязательный | Описание                      |
| -------- | ------ | ------------ | ----------------------------- |
| id       | string | ✅            | Идентификатор заказа мерчанта |

### Успешный ответ

**Код:** 200 OK\
**Content-Type:** application/json

json

```
[
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
]
```

### Описание полей ответа

| Поле                              | Тип    | Описание                                          |
| --------------------------------- | ------ | ------------------------------------------------- |
| amount                            | number | Сумма транзакции                                  |
| amount\_usdt                      | string | Сумма в USDT                                      |
| bank\_name\_en                    | string | Название банка на английском                      |
| bank\_name\_ru                    | string | Название банка на русском                         |
| country\_code                     | string | Код страны                                        |
| created\_at                       | string | Дата и время создания ордера                      |
| currency                          | string | Валюта транзакции (по умолчанию: USDT)            |
| customer\_name                    | string | Имя клиента                                       |
| customer\_requisite               | string | Платежные данные клиента                          |
| expires\_at                       | string | Дата и время истечения срока действия ордера      |
| merchant\_order\_id               | string | Внутренний идентификатор заказа мерчанта          |
| requisite\_attributes             | object | Дополнительные атрибуты реквизитов                |
| requisite\_attributes.phoneNumber | string | Номер телефона для платежа                        |
| requisite\_type                   | string | Тип платежного реквизита (SBP, CARD и т.д.)       |
| requisites                        | string | Платежные реквизиты                               |
| status                            | string | Статус ордера (pending, completed, failed и т.д.) |
| url\_callback                     | string | URL для callback-уведомлений                      |
| uuid                              | string | Уникальный идентификатор ордера                   |
| uuid\_callback                    | string | UUID для callback-идентификации                   |

### Возможные ошибки

#### 400 Bad Request

json

```
{
  "error": "string"
}
```

**Причина:** Неверный формат идентификатора

#### 401 Unauthorized

json

```
{
  "error": "string"
}
```

**Причина:** Не авторизованный доступ, неверные учетные данные

#### 404 Not Found

json

```
{
  "error": "string"
}
```

**Причина:** Ордер не найден

#### 500 Internal Server Error

json

```
{
  "error": "string"
}
```

**Причина:** Внутренняя ошибка сервера

### Пример использования

bash

```
curl -X GET "https://api.pandapay24.com/v1/orders/merchant/order_12345" \
  -H "Content-Type: application/json" \
  -H "X-Api-Key: YOUR_API_KEY" \
  -H "X-Signature: YOUR_SIGNATURE" \
  -H "X-Timestamp: 1759744481"
```

### Важные замечания

1. **Статусы ордеров:** Обращайте внимание на поле `status` для отслеживания состояния платежа
2. **Время жизни:** Поле `expires_at` указывает, до какого времени ордер активен
3. **Множественные ордера:** Метод возвращает массив, так как к одному merchant\_order\_id могут быть привязаны несколько ордеров
4. **Callback данные:** Используйте `url_callback` и `uuid_callback` для обработки уведомлений о смене статуса
