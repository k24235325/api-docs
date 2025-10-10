# Загрузка чека для ордера

**Метод:** POST\
**URL:** `/orders/{id}/receipt`

Загрузка изображения/файла чека для существующего ордера. Поддерживаются форматы JPG, JPEG, PNG. Максимальный размер файла - 3MB.

### Аутентификация

Для этого метода требуется аутентификация на основе подписи. Включите заголовки:

* `X-API-Key` - API ключ мерчанта
* `X-Timestamp` - текущая Unix метка времени
* `X-Signature` - HMAC-SHA256 подпись

#### Пример расчета подписи

**X-Timestamp:** "1634567890" (Unix timestamp)\
**Путь запроса:** "/orders/550e8400-e29b-41d4-a716-446655440000/receipt"\
**Строка для подписи:** "1634567890/orders/550e8400-e29b-41d4-a716-446655440000/receipt"\
**X-Signature:** HMAC-SHA256(string\_to\_sign, secretKey) в hex формате

#### Пример генерации подписи (JavaScript)

javascript

```
const timestamp = Math.floor(Date.now() / 1000).toString();
const orderId = '550e8400-e29b-41d4-a716-446655440000';
const path = `/orders/${orderId}/receipt`;
const stringToSign = timestamp + path;
const signature = CryptoJS.HmacSHA256(stringToSign, merchantSecretKey).toString(CryptoJS.enc.Hex);
```

#### Пример генерации подписи (Go)

go

```
import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/hex"
    "fmt"
    "strconv"
    "time"
)

func generateSignature() {
    timestamp := strconv.FormatInt(time.Now().Unix(), 10)
    orderID := "550e8400-e29b-41d4-a716-446655440000"
    path := fmt.Sprintf("/orders/%s/receipt", orderID)
    stringToSign := timestamp + path

    h := hmac.New(sha256.New, []byte(merchantSecretKey))
    h.Write([]byte(stringToSign))
    signature := hex.EncodeToString(h.Sum(nil))

    fmt.Println("X-API-Key:", merchantAPIKey)
    fmt.Println("X-Timestamp:", timestamp)
    fmt.Println("X-Signature:", signature)
}
```

### Параметры запроса

#### Параметры пути

| Параметр | Тип    | Обязательный | Описание    |
| -------- | ------ | ------------ | ----------- |
| id       | string | ✅            | UUID ордера |

#### Данные формы

| Параметр | Тип  | Обязательный | Описание              |
| -------- | ---- | ------------ | --------------------- |
| receipt  | file | ✅            | Файл чека (макс. 3MB) |

#### Заголовки

| Параметр    | Тип    | Обязательный | Описание                   |
| ----------- | ------ | ------------ | -------------------------- |
| X-API-Key   | string | ✅            | API ключ мерчанта          |
| X-Timestamp | string | ✅            | Текущая Unix метка времени |
| X-Signature | string | ✅            | HMAC-SHA256 подпись        |

### Ответы

#### Успешный ответ

**Код:** 200 OK

#### Ошибки

**400 Bad Request**

json

```
{
  "error": "string"
}
```

**401 Unauthorized**

json

```
{
  "error": "string"
}
```

**404 Not Found**

json

```
{
  "error": "string"
}
```

**415 Unsupported Media Type**

json

```
{
  "error": "string"
}
```

**500 Internal Server Error**

json

```
{
  "error": "string"
}
```

### Пример использования

bash

```
curl -X POST "https://api.pandapay24.com/v1/orders/550e8400-e29b-41d4-a716-446655440000/receipt" \
  -H "X-Auth-Token: YOUR_FRONTEND_TOKEN" \
  -H "X-Timestamp: 1634567890" \
  -F "receipt=@/path/to/receipt.jpg"
```

bash

curl -X POST "https://api.pandapay24.com/v1/orders/550e8400-e29b-41d4-a716-446655440000/receipt"\
-H "X-Auth-Token: YOUR\_FRONTEND\_TOKEN"\
-H "X-Timestamp: 1634567890"\
-F "receipt=@/path/to/receipt.jpg"
