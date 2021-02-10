# Демка по протофайликам
Чтобы сгенерить тайпинги: `yarn proto`

## Кейс 1:
Ломаем импорт в `proto/api/payments.proto`
```proto
syntax = 'proto3';

package payments;

import "google/protobuf/wrappers.proto";
import "messages/asdasd/payment_order.proto";
                 ^^^^^^

message PaymentPrintableResponse {
  google.protobuf.BytesValue content = 1;
}

```
Теперь при генерации нас ругает за то, что не смог найти модельку, но не поругало, что файлика такого нету:
```
Error: no such Type or Enum 'PaymentOrder' in Type .payments.PaymentPrintableRequest
```


## Кейс 2:
Есть расширение типов от гугла, со всякими доп модельками:
https://googleapis.github.io/gax-php/0.21.0/Google/Type/Money.html
https://github.com/googleapis/googleapis/blob/master/google/type/money.proto

Если раскомментить вот эти 2 строки в `proto/payment_order.proto`
```proto
// import "google/type/money.proto";
// google.type.Money amount = 2;
```

То так же, не будет ругаться, что не может скачать, но будет ругаться, что не может обнаружить тип или энам:
```
Error: no such Type or Enum 'Money' in Type .payments.PaymentOrder
```
