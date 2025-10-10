# Получение статистики по ордерам

### Описание метода

**Метод:** GET\
**URL:** `/orders/stats`

### Параметры запроса

#### Заголовки (Headers)

| Параметр      | Тип    | Обязательный | Описание     |
| ------------- | ------ | ------------ | ------------ |
| Authorization | string | ✅            | Bearer токен |

#### Параметры строки запроса (Query Parameters)

| Параметр      | Тип    | Обязательный | Описание                                   |
| ------------- | ------ | ------------ | ------------------------------------------ |
| country\_code | string | ❌            | Фильтр по коду страны                      |
| req\_type     | string | ❌            | Фильтр по типу запроса                     |
| from\_date    | string | ❌            | Начальная дата фильтрации (RFC3339 формат) |
| to\_date      | string | ❌            | Конечная дата фильтрации (RFC3339 формат)  |

### Успешный ответ

**Код:** 200 OK\
**Content-Type:** application/json

json

```
{
  "amount_conversion": 75,
  "completed_amount": 8000,
  "completed_orders": 80,
  "failed_amount": 500,
  "order_conversion": 80,
  "pending_amount": 2000,
  "pending_orders": 10,
  "total_amount": 10000,
  "total_commission": 100,
  "total_orders": 100
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

**Причина:** Неверные параметры запроса

#### 401 Unauthorized

json

```
{
  "error": "string"
}
```

**Причина:** Отсутствует или недействителен токен авторизации

#### 404 Not Found

json

```
{
  "error": "string"
}
```

**Причина:** Мерчант или трейдер не найден

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
curl -X GET "https://api.pandapay24.com/v1/orders/stats?country_code=RUS&requisite_type=SBP&from_date=2024-01-01T00:00:00Z&to_date=2024-12-31T23:59:59Z" \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer YOUR_ACCESS_TOKEN"
```
