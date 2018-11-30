# Restful APIs for Pairs

## Authorization

### Bearer
See [Authentication] (./authentication.md)

### Authorization
Put the `Bearer` string `eyJhbG...` into http header `Authorization: Bearer eyJhbG...`

## Endpoints

[Get orderbook](#get-orderbook)

[Get trades](#get-trades)

[Get ticker](#get-ticker)

## Get orderbook
```
GET /pairs/{code}/orderbook
```
Get orderbook by pair code

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /pairs/ceteos/orderbook
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`levels` | integer | no | `1..1000`, default: `10` | max price levels

**Response:**
```json
{
  "bsn": 1,
  "asks": [
    [
      1.1,
      10.0
    ],
    [
      1.2,
      100.0
    ],
    [
      1.3,
      1000.0
    ]
  ],
  "bids": [
    [
      0.9,
      10.0
    ],
    [
      0.8,
      100.0
    ],
    [
      0.7,
      1000.0
    ]
  ]
}
```

Key | Description
------------ | ------------
`bsn` | orderbook serial number, +1 whenever orderbook changes
`asks` | asks orders, aggregated by price level
`bids` | bids orders, aggregated by price level
`[price, quantity]` | price and quantity in a price level

**Ratelimit:**
5 requests / 60 seconds, per pair

## Get trades
```
GET /pairs/{code}/trades
```
Get trades by pair code

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /pairs/ceteos/trades
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`limit` | integer | no | `1..500`, default: `100` | max trades

**Response:**
```json
[
  {
    "id": 1001,
    "bsn": 122,
    "trend": "up",
    "price": 0.29,
    "volume": 1.0,
    "amount": 0.29,
    "created_at": 1542880742252
  },
  {
    "id": 1000,
    "bsn": 119,
    "trend": "down",
    "price": 0.28,
    "volume": 10.0,
    "amount": 2.8,
    "created_at": 1542709903037
  }
]
```

Key | Description
------------ | ------------
`id` | native trade id
`bsn` | orderbook serial number, +1 whenever orderbook changes
`trend` | `"up"` means a proactive buying <br /> `"down"` means a proactive selling
`price` | trading price
`volume` | trading volume
`amount` | trading amount, equals price * volume

**Ratelimit:**
5 requests / 60 seconds, per pair

### Get ticker
```
GET /pairs/{code}/ticker
```
Get ticker by pair code

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /pairs/ceteos/ticker
```

**Response:**
```json
{
  "price": "3.65",
  "change": "9.26%",
  "high": "5.81",
  "low": "2.97",
  "volume": "8754934.4"
}
```

Key | Description
------------ | ------------
`price` | current quote price
`change` | 24 hours change in percent
`high` | highest price in past 24 hours
`low` | lowest price in past 24 hours
`volume` | trading volume in past 24 hours

**Ratelimit:**
10 requests / 60 seconds, per pair
