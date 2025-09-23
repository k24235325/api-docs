# Table of contents

* [Api документация](README.md)
  * [Руководство по генерации ключей](readme/rukovodstvo-po-generacii-klyuchei.md)
  * [Примеры интеграции на python](readme/primery-integracii-na-python.md)
  * [Пример интеграции на go](readme/primer-integracii-na-go.md)

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
