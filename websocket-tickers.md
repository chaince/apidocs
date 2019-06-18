# Websocket for Tickers

## Join

### Connect

Connect the websocket endpoint **wss://api.hoo.com/websocket**

### Bearer
See [Authentication](./authentication.md)

### Sent
Sent a `join` request to server in json format

*Demo*

```json
{
  "topic": "ticker:ceteos",
  "payload": {"bearer": "eyJhbG..."},
  "event": "phx_join",
  "ref": 1
}
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`topic` | string | yes | "ticker:{code}" | the channel to join
`payload` | object | yes | {"bearer": {bearer}} | the bearer to authorize
`event` | string | yes | "phx_join" | set it as is, required by protocol
`ref` | integer | yes | 1 | set it as is, required by protocol

**Reply:**

```json
{
  "topic": "ticker:ceteos",
  "payload": {"status": "ok", "response": {}},
  "event": "phx_reply"
}
```

Key | Type | Value | Description
------------ | ------------ | ------------ | ------------
`topic` | string | "ticker:{code}" | the channel just joined
`event` | string | "phx_reply" | joining reply from server
`payload` | object | {"status": "ok"} | joined sucessfully

## Stream

Ticker and 10 levels depth each direction are pushed every 5 seconds

```json
{
  "topic": "ticker:ceteos",
  "event": "ticker",
  "payload": {
    "price": "0.28",
    "change": "-12.68%",
    "high": "0.33",
    "low": "0.21",
    "volume": "382796.1",
    "asks": [
      [0.31, 133.8],
      [0.32, 3289.2],
      [0.33, 985.9]
    ],
    "bids": [
      [0.29, 739.0],
      [0.28, 38.6],
      [0.27, 22.8]
    ]
  }
```

**Payload:**

Key | Description
------------ | ------------
`price` | current quote price
`change` | 24 hours change in percent
`high` | highest price in past 24 hours
`low` | lowest price in past 24 hours
`volume` | trading volume in past 24 hours
`asks` | depth asks of 10 price levels
`bids` | depth bids of 10 price levels
`[price, quantity]` | price and quantity in a price level
