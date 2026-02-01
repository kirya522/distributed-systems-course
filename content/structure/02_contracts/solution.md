+++
title = 'Решение практического задания'
weight = 3
+++
# Решение практического задания

У [задачи](practice.md#задание) может быть несколько решений, присылайте свои в [чат курса](https://t.me/kirya522_chat)

## Часть 1. HTTP-контракты для домена “Заказы”

- Чтение заказа по ID
- Получение списка заказов пользователя

Используем OpenAPI 3.0

`orders-api.yaml`

```yaml
openapi: 3.0.3

info:
  title: Orders API
  description: Public HTTP API for Orders domain
  version: 1.0.0

servers:
  - url: http://orders-service:8080/api/v1

paths:
  /orders:
    post:
      summary: Create order
      operationId: createOrder
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateOrderRequest'
      responses:
        '201':
          description: Order created
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/Order'
        '400':
          description: Invalid request

  /users/{userId}/orders:
    get:
      summary: Get orders of user
      operationId: getUserOrders
      parameters:
        - name: userId
          in: path
          required: true
          schema:
            type: integer
            format: int64
      responses:
        '200':
          description: List of user orders
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Order'
        '404':
          description: User not found

components:
  schemas:

    CreateOrderRequest:
      type: object
      required:
        - userId
        - items
      properties:
        userId:
          type: integer
          format: int64
        items:
          type: array
          minItems: 1
          items:
            $ref: '#/components/schemas/OrderItem'

    Order:
      type: object
      required:
        - id
        - userId
        - status
        - createdAt
      properties:
        id:
          type: integer
          format: int64
        userId:
          type: integer
          format: int64
        status:
          type: string
          enum:
            - CREATED
            - PAID
            - SHIPPED
            - CANCELLED
        totalPrice:
          type: number
          format: integer
        createdAt:
          type: string
          format: date-time

    OrderItem:
      type: object
      required:
        - productId
        - quantity
        - price
      properties:
        productId:
          type: integer
          format: int64
        quantity:
          type: integer
          minimum: 1
        price:
          type: number
          format: integer
```

Пример кодогенерации [клиентов в golang](https://www.speakeasy.com/docs/sdks/languages/golang/oss-comparison-go)
Как сгенерировать по типам объектов.

Детальное решение на [github](https://github.com/kirya522/distributed-systems-course/tree/main/content/structure/02_contracts/code)

DTO
```
go run github.com/oapi-codegen/oapi-codegen/v2/cmd/oapi-codegen \
  --generate models \
  --package api \
  -o internal/api/models.gen.go \
  api/orders-api.yaml
```

Клиент
```
go run github.com/oapi-codegen/oapi-codegen/v2/cmd/oapi-codegen \
  --generate client \    
  --package api \
  -o internal/api/client.gen.go \
  api/orders-api.yaml
```

ADR, с последствиями
```md
## Контекст
Другим командам нужно интегрироваться с нашими системами

## Решение
Публиковать OpenAPI контракты и генератор для них

## Альтернативы
- Вручную сопровождать контракты
- Динамические схемы и service-discovery

## Последствия
+ Контракты описаны строго
+ Публичные интерфейсы в системе
− Затраты на сопровождение контрактов
− Необходимо переводить все методы на генерацию

```

## Часть 2. Событийный контракт

Событие OrderStatusChanged. Определимся с семантикой контракта.
Событие будет без данных, за данными системы должны обращаться к сервису заказов.
Тут видно проблему - **не хватает метода чтения заказа по ID**.

- `order_id` идентификатор заказа
- `user_id` владелец заказа
- `created_at` момент создания
- `event_id` ключ идемпотентности события

Детальное решение на [github](https://github.com/kirya522/distributed-systems-course/tree/main/content/structure/02_contracts/code)

`orders-events.yaml`

```yaml
asyncapi: 2.6.0
info:
  title: Orders Events API
  version: 1.0.0
  description: >
    Event contracts published by Orders service.

servers:
  kafka:
    url: localhost:9092
    protocol: kafka
    description: Local Kafka broker

channels:
  orders.order-created:
    description: Order lifecycle events
    publish:
      summary: Order was successfully created
      operationId: publishOrderCreated
      message:
        name: OrderCreated
        contentType: application/json
        payload:
          $ref: '#/components/schemas/OrderCreated'

components:
  schemas:
    OrderCreated:
      type: object
      required:
        - event_id
        - order_id
        - user_id
        - total_amount
        - created_at
      properties:
        event_id:
          type: string
          format: uuid
          description: Unique event identifier
        order_id:
          type: integer
          format: int64
        user_id:
          type: integer
          format: int64
        total_amount:
          type: number
          format: double
        created_at:
          type: string
          format: date-time
```

Разберем детали

- event_id (дедупликация, идемпотентность)
- почему не все данные, заказ может поменяться при обработке лучше сходить за актуальным состоянием
- отдельный топик (отделение по семантике, легче масштабировать)

Как сгенерировать
```sh
go install github.com/lerenn/asyncapi-codegen/cmd/asyncapi-codegen@latest
go run github.com/lerenn/asyncapi-codegen/cmd/asyncapi-codegen@latest -i ./events/orders-events.yaml -p events -o ./internal/events/asyncapi.gen.go
```

Генерируется сильно больше всего, можно использовать только схему событий и тд.

## Часть 3. Эволюция контрактов

- Добавить `deliveryType`
- Добавить новый статус `RETURNED`
- Удалить поле `totalPrice`

| Изменение               | Тип            | Безопасно ли                                                                                              | Комментарий                                                                                                 |
| ----------------------- | -------------- | --------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- |
| ✅ добавить deliveryType | новое поле     | зависит от протокола событий (бинарный или нет), по умолчанию расширение моделей не должно быть проблемой | расширяемость API                                                                                           |
| ⚠️ новый статус RETURNED | новое значение | большинство клиентов адекватно реагирует на добавление нового enum, если только набор не валидируется     | нужно проверить всех клиентов, или провести в 2 этапа, сначала добавить статус в значения, но не возвращать |
| ⛔️ удалить поле totalPrice | удаление поля | если поле было обязательным, то клиенты могут сломаться, но важно закладывать бизнес-смысл, требуется скрывать значение, то можно ввести сервисное значение (-1 или 0), сделать поле необязательным и мигрировать клиентов | точно вызовет проблемы |
