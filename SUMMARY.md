# Table of contents

* [Api документация](README.md)
  * [Руководство по генерации ключей](readme/rukovodstvo-po-generacii-klyuchei.md)
  * [Пример интеграции на python](readme/primer-integracii-na-python.md)
  * [Примеры интеграции на go](readme/primery-integracii-na-go.md)

## Reference

* ```yaml
  props:
    models: false
  type: builtin:openapi
  dependencies:
    spec:
      ref:
        kind: openapi
        spec: gitbook-petstore
  ```
