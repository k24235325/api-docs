# Примеры интеграции на go

### Быстрая навигация

* [Создание заказа ](primery-integracii-na-go.md#sozdanie-zakaza)
* [Сервер для обработки webhook ](primery-integracii-na-go.md#server-dlya-obrabotki-webhook)
* [Создание заказа (альтернативная версия)](primery-integracii-na-go.md#sozdanie-zakaza-alternativnaya-versiya)

### Создание заказа&#x20;

```go
package main

import (
	"bytes"
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"encoding/json"
	"fmt"
	"github.com/google/uuid"
	"net/http"
	"strconv"
	"time"
)

func main() {
	// Your API credentials
	apiKey := "apiKey"
	secret := "secret"

	// Request payload
	payload := map[string]interface{}{
		"amount_rub":      1000.47474483483843894,
		"requisite_type": "SBP",
		"idempotency_key": uuid.New().String(),
	}

	// Convert payload to JSON
	jsonPayload, err := json.Marshal(payload)
	if err != nil {
		panic(err)
	}

	// Generate signature
	signature, timestamp, err := GenerateSignature(secret, string(jsonPayload))
	if err != nil {
		panic(err)
	}

	// Create request
	req, err := http.NewRequest("POST", "https://api.pandapay24.com/orders", bytes.NewBuffer(jsonPayload))
	if err != nil {
		panic(err)
	}

	// Add headers
	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("X-API-Key", apiKey)
	req.Header.Set("X-Signature", signature)
	req.Header.Set("X-Timestamp", timestamp)

	// Send request
	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}

	fmt.Printf("Response: %s\n", resp.Status)
}


// GenerateSignature creates an HMAC signature for API requests
// Parameters:
//   - secret: API key secret
//   - payload: JSON request body as string
//
// Returns:
//   - signature: HMAC signature
//   - timestamp: Unix timestamp used in signature
//   - error: if any error occurs during signature generation
func GenerateSignature(secret string, payload string) (signature string, timestamp string, err error) {
	// Generate current UTC timestamp
	ts := time.Now().UTC().Unix()
	timestamp = strconv.FormatInt(ts, 10)

	// Combine timestamp and payload
	// If payload is empty, you only need to sign the timestamp
	stringToSign := timestamp + payload

	// Create HMAC SHA256 hash
	h := hmac.New(sha256.New, []byte(secret))
	h.Write([]byte(stringToSign))
	signature = hex.EncodeToString(h.Sum(nil))

	return signature, timestamp, nil
}
```

[↑ К быстрой навигации](primery-integracii-na-go.md#bystraya-navigaciya)

### Что нужно заполнить:

&#x20;**Авторизация:**

* `apiKey` → ваш API-ключ из личного кабинета
* `secret` → ваш секретный ключ

**Данные заказа:**

* `1000.4747448348385` → сумма в рублях
* `"SBP"` → тип реквизита

### Как использовать:

1. Замените `apiKey` и `secret` на реальные ключи
2. Укажите сумму и тип реквизита из списка выше
3. Запустите скрипт

***

### Сервер для обработки webhook&#x20;

```go
package main

import (
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"fmt"
	"io"
	"log"
	"net/http"
	"time"
)

const (
	// Your API credentials
	apiKey    = "apiKey"
	apiSecret = "apiSecret"
	port      = 3000
)

type OrderCallback struct {
	OrderUUID          string  `json:"order_uuid"`
	Status             string  `json:"status"`
	AmountUSDT         float64 `json:"amount_usdt"`
	AmountRUB          float64 `json:"amount_rub"`
	MerchantCommission float64 `json:"merchant_commission"`
}

func main() {
	http.HandleFunc("/webhook", handleCallback)

	fmt.Printf("Test merchant API server starting on port %d...\n", port)
	fmt.Printf("Webhook URL: http://localhost:%d/webhook\n", port)

	if err := http.ListenAndServe(fmt.Sprintf(":%d", port), nil); err != nil {
		log.Fatal(err)
	}
}

func handleCallback(w http.ResponseWriter, r *http.Request) {
	// Check HTTP method
	if r.Method != http.MethodPost {
		http.Error(w, "Method not allowed", http.StatusMethodNotAllowed)
		fmt.Printf("Error: Received %s request, expected POST\n", r.Method)
		return
	}

	fmt.Println("\n=== Received Callback ===")
	fmt.Printf("Time: %s\n", time.Now().Format(time.RFC3339))
	fmt.Printf("Method: %s\n", r.Method)

	// Read the request body
	body, err := io.ReadAll(r.Body)
	if err != nil {
		log.Printf("Error reading body: %v", err)
		http.Error(w, "Error reading body", http.StatusBadRequest)
		return
	}

	// Log headers
	fmt.Println("\nHeaders:")
	fmt.Printf("Content-Type: %s\n", r.Header.Get("Content-Type"))
	fmt.Printf("X-API-Key: %s\n", r.Header.Get("X-API-Key"))
	fmt.Printf("X-Signature: %s\n", r.Header.Get("X-Signature"))
	fmt.Printf("X-Timestamp: %s\n", r.Header.Get("X-Timestamp"))

	// Check Content-Type
	if r.Header.Get("Content-Type") != "application/json" {
		log.Printf("Invalid Content-Type: %s", r.Header.Get("Content-Type"))
		http.Error(w, "Content-Type must be application/json", http.StatusUnsupportedMediaType)
		return
	}

	// Verify API key
	if r.Header.Get("X-API-Key") != apiKey {
		log.Printf("Invalid API key received: %s", r.Header.Get("X-API-Key"))
		http.Error(w, "Invalid API key", http.StatusUnauthorized)
		return
	}

	// Verify signature
	if !verifySignature(r.Header.Get("X-Timestamp"), body, r.Header.Get("X-Signature")) {
		log.Printf("Invalid signature received: %s", r.Header.Get("X-Signature"))
		http.Error(w, "Invalid signature", http.StatusUnauthorized)
		return
	}

	log.Printf("Callback JSON: %v\n", string(body))
}

func verifySignature(timestamp string, body []byte, signature string) bool {
	// Create string to sign (timestamp + body)
	stringToSign := timestamp + string(body)

	// Generate HMAC signature
	h := hmac.New(sha256.New, []byte(apiSecret))
	h.Write([]byte(stringToSign))
	expectedSignature := hex.EncodeToString(h.Sum(nil))

	// Compare signatures
	return signature == expectedSignature
}
```

[↑ К быстрой навигации](primery-integracii-na-go.md#bystraya-navigaciya)

### Авторизация:

* `apiKey` → ваш API-ключ из личного кабинета
* `apiSecret` → ваш секретный ключ

### Как использовать:

1. Замените `apiKey` и `apiSecret` на реальные ключи
2. Запустите скрипт
3. Сервер начнет принимать webhook-уведомления на `http://localhost:3000/webhook`

***

### Создание заказа (альтернативная версия)

```go
package main

import (
	"bytes"
	"crypto/hmac"
	"crypto/sha256"
	"encoding/hex"
	"encoding/json"
	"fmt"
	"github.com/google/uuid"
	"net/http"
	"strconv"
	"time"
)

func main() {
	// Your API credentials
	apiKey := "apiKey"
	secret := "secret"

	// Request payload
	payload := map[string]interface{}{
		"amount":          "1000.47474483483843894",
		"requisite_type":  "SBP",
		"idempotency_key": uuid.New().String(),
	}

	// Convert payload to JSON
	jsonPayload, err := json.Marshal(payload)
	if err != nil {
		panic(err)
	}

	// Generate signature
	signature, timestamp, err := GenerateSignature(secret, string(jsonPayload))
	if err != nil {
		panic(err)
	}

	// Create request
	req, err := http.NewRequest("POST", "https://api.pandapay24.com/v1/orders", bytes.NewBuffer(jsonPayload))
	if err != nil {
		panic(err)
	}

	// Add headers
	req.Header.Set("Content-Type", "application/json")
	req.Header.Set("X-API-Key", apiKey)
	req.Header.Set("X-Signature", signature)
	req.Header.Set("X-Timestamp", timestamp)

	// Send request
	client := &http.Client{}
	resp, err := client.Do(req)
	if err != nil {
		panic(err)
	}

	fmt.Printf("Response: %s\n", resp.Status)
}

// GenerateSignature creates an HMAC signature for API requests
// Parameters:
//   - secret: API key secret
//   - payload: JSON request body as string
//
// Returns:
//   - signature: HMAC signature
//   - timestamp: Unix timestamp used in signature
//   - error: if any error occurs during signature generation
func GenerateSignature(secret string, payload string) (signature string, timestamp string, err error) {
	// Generate current UTC timestamp
	ts := time.Now().UTC().Unix()
	timestamp = strconv.FormatInt(ts, 10)

	// Combine timestamp and payloads
	// If payload is empty, you only need to sign the timestamp
	stringToSign := timestamp + payload

	// Create HMAC SHA256 hash
	h := hmac.New(sha256.New, []byte(secret))
	h.Write([]byte(stringToSign))
	signature = hex.EncodeToString(h.Sum(nil))

	return signature, timestamp, nil
}
```

[↑ К быстрой навигации](primery-integracii-na-go.md#bystraya-navigaciya)

### Что нужно заполнить:

**Авторизация:**

* `apiKey` → ваш API-ключ из личного кабинета
* `secret` → ваш секретный ключ

**Данные заказа:**

* `1000.4747448348385` → сумма в рублях
* `"SBP"` → тип реквизита

### Как использовать:

1. Замените `apiKey` и `secret` на реальные ключи
2. Укажите сумму и тип реквизита из списка выше
3. Запустите скрипт

###
