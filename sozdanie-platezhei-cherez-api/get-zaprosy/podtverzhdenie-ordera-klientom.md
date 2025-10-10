# Подтверждение ордера клиентом

### Описание метода

**Метод:** GET\
**URL:** `/orders/{id}/confirm`

Подтверждение, что клиент отправил платеж по ордеру.

### Параметры запроса

#### Параметры пути (Path Parameters)

| Параметр | Тип    | Обязательный | Описание    |
| -------- | ------ | ------------ | ----------- |
| id       | string | ✅            | UUID ордера |

#### Заголовки (Headers)

| Параметр    | Тип    | Обязательный | Описание                             |
| ----------- | ------ | ------------ | ------------------------------------ |
| X-API-Key   | string | ✅            | API ключ мерчанта                    |
| X-Timestamp | string | ✅            | Текущая метка времени в Unix формате |
| X-Signature | string | ✅            | HMAC-SHA256 подпись                  |

### Успешный ответ

**Код:** 200 OK

### Возможные ошибки

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

bash &#x20;

<pre><code><strong>curl -X GET "https://api.pandapay24.com/v1/orders/123e4567-e89b-12d3-a456-426614174000/confirm" \
</strong>  -H "X-API-Key: YOUR_API_KEY" \
  -H "X-Timestamp: 1759744481" \
  -H "X-Signature: YOUR_SIGNATURE"
</code></pre>
