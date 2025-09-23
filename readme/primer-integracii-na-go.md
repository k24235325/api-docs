# Пример интеграции на go

## Примеры интеграции

Код для создания нового платежного заказа через API Panda Pay.

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

**Что нужно подставить:**

* `apiKey` → ваш API ключ из личного кабинета
* `secret` → ваш секретный ключ
* `1000.4747448348385` → сумма заказа в рублях
* `"SBP"` → тип платежной системы

***

### Сервер для обработки webhook (Python)

Код простого HTTP-сервера для приема и проверки callback-уведомлений.

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

**Что нужно подставить:**

* `apiKey` → ваш API ключ
* `apiSecret` → ваш секретный ключ
* `3000` → порт для webhook-сервера

***

### Создание заказа (альтернативная версия)

Альтернативная версия кода для создания заказа с немного измененной структурой запроса.

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

**Что нужно подставить:**

* `apiKey` → ваш API ключ
* `secret` → ваш секретный ключ
* `"1000.4747448348385"` → сумма заказа (строковый формат)
* `"SBP"` → тип платежной системы
