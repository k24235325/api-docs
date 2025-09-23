# Пример интеграции на python

### Быстрая навигация

* [Создание заказа ](primer-integracii-na-python.md#sozdanie-zakaza-python)
* [Сервер для обработки webhook ](primer-integracii-na-python.md#server-dlya-obrabotki-webhook-python)
* [Создание заказа (альтернативная версия)](primer-integracii-na-python.md#sozdanie-zakaza-alternativnaya-versiya)

***

### Создание заказа  <a href="#sozdanie-zakaza-python" id="sozdanie-zakaza-python"></a>

```python
import os
import json
import time
import uuid
import hmac
import hashlib
import urllib.request
import urllib.error


def generate_signature(secret: str, payload: str) -> tuple[str, str]:
    ts = str(int(time.time()))
    string_to_sign = ts + payload
    signature = hmac.new(secret.encode(), string_to_sign.encode(), hashlib.sha256).hexdigest()
    return signature, ts


def main() -> None:
    api_key = os.getenv("API_KEY", "apiKey")
    secret = os.getenv("SECRET_KEY", "secret")
    api_host = os.getenv("API_HOST", "https://api.pandapay24.com")

    payload_dict = {
        "amount_rub": 1000.4747448348385,
        "requisite_type": "SBP",
        "idempotency_key": str(uuid.uuid4()),
    }

    json_payload = json.dumps(payload_dict, separators=(",", ":"))
    signature, timestamp = generate_signature(secret, json_payload)

    req = urllib.request.Request(
        url=f"{api_host.rstrip('/')}/orders",
        data=json_payload.encode(),
        headers={
            "Content-Type": "application/json",
            "X-API-Key": api_key,
            "X-Signature": signature,
            "X-Timestamp": timestamp,
        },
        method="POST",
    )

    try:
        with urllib.request.urlopen(req, timeout=30) as resp:
            body = resp.read().decode("utf-8", errors="replace")
            print(f"Response: {resp.status} {resp.reason}")
            if body:
                print(body)
    except urllib.error.HTTPError as e:
        body = e.read().decode("utf-8", errors="replace")
        print(f"Response: {e.code} {e.reason}")
        if body:
            print(body)
    except urllib.error.URLError as e:
        print(f"Request failed: {e.reason}")


if __name__ == "__main__":
    main()
```

↑[ К быстрой навигации](primer-integracii-na-python.md#bystraya-navigaciya)

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



### Сервер для обработки webhook  <a href="#server-dlya-obrabotki-webhook-python" id="server-dlya-obrabotki-webhook-python"></a>

```python
import hmac
import hashlib
import time
from http.server import BaseHTTPRequestHandler, HTTPServer


api_key = "apiKey"
api_secret = "apiSecret"
port = 3000


def verify_signature(timestamp: str, body: bytes, signature: str) -> bool:
    string_to_sign = timestamp + body.decode("utf-8")
    expected = hmac.new(api_secret.encode(), string_to_sign.encode(), hashlib.sha256).hexdigest()
    return hmac.compare_digest(signature, expected)


class Handler(BaseHTTPRequestHandler):
    def do_POST(self):
        if self.path != "/webhook":
            self.send_response(404)
            self.end_headers()
            return

        print("\n=== Received Callback ===")
        print(f"Time: {time.strftime('%Y-%m-%dT%H:%M:%SZ', time.gmtime())}")
        print(f"Method: {self.command}")

        length = int(self.headers.get("Content-Length", 0))
        body = self.rfile.read(length) if length > 0 else b""

        print("\nHeaders:")
        print(f"Content-Type: {self.headers.get('Content-Type')}")
        print(f"X-API-Key: {self.headers.get('X-API-Key')}")
        print(f"X-Signature: {self.headers.get('X-Signature')}")
        print(f"X-Timestamp: {self.headers.get('X-Timestamp')}")

        if self.headers.get("Content-Type") != "application/json":
            self.send_response(415)
            self.end_headers()
            self.wfile.write(b"Content-Type must be application/json")
            return

        if self.headers.get("X-API-Key") != api_key:
            self.send_response(401)
            self.end_headers()
            self.wfile.write(b"Invalid API key")
            return

        timestamp = self.headers.get("X-Timestamp", "")
        signature = self.headers.get("X-Signature", "")
        if not verify_signature(timestamp, body, signature):
            self.send_response(401)
            self.end_headers()
            self.wfile.write(b"Invalid signature")
            return

        print(f"Callback JSON: {body.decode('utf-8')}")
        self.send_response(200)
        self.end_headers()

    def log_message(self, format, *args):
        return


def run():
    server = HTTPServer(("", port), Handler)
    print(f"Test merchant API server starting on port {port}...")
    print(f"Webhook URL: http://localhost:{port}/webhook")
    server.serve_forever()


if __name__ == "__main__":
    run()
```

↑ [К быстрой навигации](primer-integracii-na-python.md#bystraya-navigaciya)

***

### Авторизация:

* `apiKey` → ваш API-ключ из личного кабинета
* `apiSecret` → ваш секретный ключ

### Как использовать:

1. Замените `apiKey` и `apiSecret` на реальные ключи
2. Запустите скрипт
3. Сервер начнет принимать webhook-уведомления на `http://localhost:3000/webhook`

### Создание заказа (альтернативная версия) <a href="#sozdanie-zakaza-alternativnaya-versiya" id="sozdanie-zakaza-alternativnaya-versiya"></a>

```python
import os
import json
import time
import uuid
import hmac
import hashlib
import urllib.request
import urllib.error


def generate_signature(secret: str, payload: str) -> tuple[str, str]:
    ts = str(int(time.time()))
    string_to_sign = ts + payload
    signature = hmac.new(secret.encode(), string_to_sign.encode(), hashlib.sha256).hexdigest()
    return signature, ts


def main() -> None:
    api_key = os.getenv("API_KEY", "apiKey")
    secret = os.getenv("SECRET_KEY", "secret")
    api_host = os.getenv("API_HOST", "https://api.pandapay24.com")

    payload_dict = {
        "amount": "1000.4747448348385",
        "requisite_type": "SBP",
        "idempotency_key": str(uuid.uuid4()),
    }

    json_payload = json.dumps(payload_dict, separators=(",", ":"))
    signature, timestamp = generate_signature(secret, json_payload)

    req = urllib.request.Request(
        url=f"{api_host.rstrip('/')}/v1/orders",
        data=json_payload.encode(),
        headers={
            "Content-Type": "application/json",
            "X-API-Key": api_key,
            "X-Signature": signature,
            "X-Timestamp": timestamp,
        },
        method="POST",
    )

    try:
        with urllib.request.urlopen(req, timeout=30) as resp:
            body = resp.read().decode("utf-8", errors="replace")
            print(f"Response: {resp.status} {resp.reason}")
            if body:
                print(body)
    except urllib.error.HTTPError as e:
        body = e.read().decode("utf-8", errors="replace")
        print(f"Response: {e.code} {e.reason}")
        if body:
            print(body)
    except urllib.error.URLError as e:
        print(f"Request failed: {e.reason}")


if __name__ == "__main__":
    main()
```

[↑ К быстрой навигации](primer-integracii-na-python.md#bystraya-navigaciya)

###

