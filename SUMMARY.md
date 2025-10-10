# Table of contents

* [Api документация](README.md)
  * [Руководство по генерации ключей](readme/rukovodstvo-po-generacii-klyuchei.md)
  * [Пример интеграции на python](readme/primer-integracii-na-python.md)
  * [Примеры интеграции на go](readme/primery-integracii-na-go.md)
* [Создание платежей через API](sozdanie-platezhei-cherez-api/README.md)
  * [POST запросы](sozdanie-platezhei-cherez-api/post-zaprosy/README.md)
    * [Создание платежного ордера](sozdanie-platezhei-cherez-api/post-zaprosy/sozdanie-platezhnogo-ordera.md)
    * [Загрузка чека для ордера](sozdanie-platezhei-cherez-api/post-zaprosy/zagruzka-cheka-dlya-ordera.md)
  * [GET запросы](sozdanie-platezhei-cherez-api/get-zaprosy/README.md)
    * [Получение информации о заказе по callback UUID](sozdanie-platezhei-cherez-api/get-zaprosy/poluchenie-informacii-o-zakaze-po-callback-uuid.md)
    * [Получение ордеров по ID заказа мерчанта](sozdanie-platezhei-cherez-api/get-zaprosy/poluchenie-orderov-po-id-zakaza-merchanta.md)
    * [Получение статистики по ордерам](sozdanie-platezhei-cherez-api/get-zaprosy/poluchenie-statistiki-po-orderam.md)
    * [Получение ордера по ID](sozdanie-platezhei-cherez-api/get-zaprosy/poluchenie-ordera-po-id.md)
    * [Отмена ордера](sozdanie-platezhei-cherez-api/get-zaprosy/otmena-ordera.md)
    * [Подтверждение ордера клиентом](sozdanie-platezhei-cherez-api/get-zaprosy/podtverzhdenie-ordera-klientom.md)

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
