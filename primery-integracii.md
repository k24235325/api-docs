# Примеры интеграции

### Способ 2: Использование Tabs (более удобно для переключения)

В GitBook можно использовать блоки с вкладками:

````markdown
# Примеры кода для создания подписи запроса

```tabs
Python:
```python
import hashlib
import hmac
from urllib.parse import urlparse

def calculate_request_signature(url: str, request_json: str, secret: str) -> str:
    parsed_url = urlparse(url)
    signature_string = request_json + parsed_url.path + parsed_url.query
    signature = hmac.new(
        secret.encode('utf-8'), 
        signature_string.encode('utf-8'), 
        hashlib.sha256
    )
    return signature.hexdigest()
````

\`\`\`python # Python код здесь import hashlib import hmac

```
def calculate_request_signature(url: str, request_json: str, secret: str) -> str:
    parsed_url = urlparse(url)
    signature_string = request_json + parsed_url.path + parsed_url.query
    signature = hmac.new(
        secret.encode('utf-8'), 
        signature_string.encode('utf-8'), 
        hashlib.sha256
    )
    return signature.hexdigest()
```
