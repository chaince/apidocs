# Restful APIs for Orders

## Authorization

### Bearer
See [Authentication](./authentication.md)

### Authorization
Put the `Bearer` string `eyJhbG...` into http header `Authorization: Bearer eyJhbG...`

## Endpoints

[Submit Order](#submit-order)

[Active Orders](#active-orders)

[Today Orders](#today-orders)

[Cancel Orders](#cancel-orders)

[Get Orders](#get-orders)

## Submit order
```
POST /orders/{code}
```
Submit new order of a pair

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
POST /orders/ceteos
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`direction` | string | yes | `"ask"` or `"bid"` | trading direction
`type` | string | yes | only `"limit"` for now | `"market"` coming soon
`price` | decimal | yes | `(0, 100000)` | price
`quantity` | decimal | yes | `(0, 100000000)` | quantity
`client_order_id` | string | no | max length: `32`; `space` `,` `;` `'` `"` not allowed | client defined order id

*Demo*:

```json
{
	"direction": "bid",
	"type": "limit",
	"price": 0.1,
	"quantity": 1,
	"client_order_id": "my-order-1"
}
```

**Response:**
```json
{
  "id": 1,
  "direction": "bid",
  "type": "limit",
  "state": "queued",
  "price": 0.1,
  "quantity": 1.0,
  "created_at": 1543224496502,
  "client_order_id": "my-order-1",
}
```

Key | Description
------------ | ------------
`id` | native order id
`state` | always be `queued` in this response
`created_at` | when order created, unix timestamp with milliseconds

**Ratelimit:**
60 requests / 60 seconds, per pair

## Active orders
```
GET /orders/{code}/active
```
All active orders of a pair

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /orders/ceteos/active
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`limit` | integer | no | `1..1000`, default: `100` | max orders
`direction` | string | no | `"bid"` or `"ask"` | orders in this direction

**Response:**
```json
[
  {
    "id": 1,
    "direction": "bid",
    "type": "limit",
    "state": "active",
    "price": 0.1,
    "quantity": 8.0,
    "volume": 2.0,
    "created_at": 1543224496502,
    "updated_at": 1543224962319,
    "client_order_id": "my-order-1",
  },
  {
    "id": 2,
    "direction": "ask",
    "type": "limit",
    "state": "active",
    "price": 0.2,
    "quantity": 10.0,
    "volume": 0.0,
    "created_at": 1543224536882,
    "updated_at": 1543224817490,
    "client_order_id": "my-order-2",
  }
]
```

Key | Description
------------ | ------------
`id` | native order id
`state` | always be `active` in this response
`quantity` | quantity remaining
`volume` | volume executed
`created_at` | when order created, unix timestamp with milliseconds
`updated_at` | when last updated, unix timestamp with milliseconds

**Ratelimit:**
5 requests / 60 seconds, per pair

## Today orders
```
GET /orders/{code}/today
```
Orders in the past 24 hours

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /orders/ceteos/today
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`limit` | integer | no | `1..1000`, default: `100` | max orders
`direction` | string | no | `"bid"` or `"ask"` | orders in this direction

**Response:**
```json
[
  {
    "id": 1,
    "direction": "bid",
    "type": "limit",
    "state": "filled",
    "price": 0.1,
    "quantity": 0.0,
    "volume": 10.0,
    "created_at": 1543224496502,
    "updated_at": 1543224962319,
    "client_order_id": "my-order-1",
  },
  {
    "id": 2,
    "direction": "ask",
    "type": "limit",
    "state": "cancelled",
    "price": 0.2,
    "quantity": 10.0,
    "volume": 0.0,
    "created_at": 1543224536882,
    "updated_at": 1543224817490,
    "client_order_id": null,
  }
]
```

Key | Description
------------ | ------------
`id` | native order id
`state` | one of `queued` `active` `filled` `cancelled`
`quantity` | quantity remaining
`volume` | volume executed
`created_at` | when order created, unix timestamp with milliseconds
`updated_at` | when last updated, unix timestamp with milliseconds

**Ratelimit:**
5 requests / 60 seconds, per pair

## Cancel orders
```
PUT /orders/{code}/cancel
```
Cancel orders by order id or client order id

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
PUT /orders/ceteos/cancel
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`order_id` | integer | one_of `1/5` | single order id | i.e. `16`
`order_ids` | list[integer] | one_of `2/5` | list of order id | i.e. `[12, 13]`
`client_order_id` | string | one_of `3/5` | single client order id | i.e. `"my-order-1"`
`client_order_ids` | list[string] | one_of `4/5` | list of client order id | i.e. `["my-order-1", "my-order-2"]`
`direction` | string | one_of `5/5` | `"bid"` or `"ask"` | all orders in this direction

**`one_of` means one and only one parameter of the five above is required**

*Demo*

```json
{
  "order_id": 16
}
```
```json
{
  "order_ids": [12, 13]
}
```
```json
{
  "client_order_id": "my-order-1"
}
```
```json
{
  "client_order_ids": ["my-order-1", "my-order-2"]
}
```
```json
{
  "direction": "bids"
}
```

**Response:**
```json
{
  "cancelled_order_ids": {
    "asks": [1,2,3],
    "bids": [8]
  }
}
```

Key | Description
------------ | ------------
`asks` | order ids actually cancelled on asks side
`bids` | order ids actually cancelled on bids side

An order will not be really cancelled if it's executed or not belong to you

**Ratelimit:**
60 requests / 60 seconds, per pair

## Get orders
```
GET /orders/{code}
```
Get orders by order id or client order id

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /orders/ceteos
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`order_ids` | string | one_of `1/2` | comma separated | i.e. `"1,2,3"`
`client_order_ids` | string | one_of `2/2` | comma separated | i.e. `"my-order-1, my-order-2"`

**`one_of` means one and only one parameter of the two above is required**

*Demo*

```
order_ids=1,2,3
```
```
client_order_ids=my-order1,my-order-2
```

**Response:**
```json
[
  {
    "id": 1,
    "direction": "bid",
    "type": "limit",
    "state": "filled",
    "price": 0.1,
    "quantity": 0.0,
    "volume": 10.0,
    "created_at": 1543224496502,
    "updated_at": 1543224962319,
    "client_order_id": "my-order-1",
  },
  {
    "id": 2,
    "direction": "ask",
    "type": "limit",
    "state": "cancelled",
    "price": 0.2,
    "quantity": 10.0,
    "volume": 0.0,
    "created_at": 1543224536882,
    "updated_at": 1543224817490,
    "client_order_id": null,
  }
]
```

Key | Description
------------ | ------------
`id` | native order id
`state` | one of `queued` `active` `filled` `cancelled`
`quantity` | quantity remaining
`volume` | volume executed
`created_at` | when order created, unix timestamp with milliseconds
`updated_at` | when last updated, unix timestamp with milliseconds

**Ratelimit:**
60 requests / 60 seconds, per pair
