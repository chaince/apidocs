# Websocket for Pairs

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
  "topic": "pair:ceteos",
  "payload": {"bearer": "eyJhbG..."},
  "event": "phx_join",
  "ref": 1
}
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`topic` | string | yes | "pair:{code}" | the channel to join
`payload` | object | yes | {"bearer": {bearer}} | the bearer to authorize
`event` | string | yes | "phx_join" | set it as is, required by protocol
`ref` | integer | yes | 1 | set it as is, required by protocol

**Reply:**

```json
{
  "topic": "pair:ceteos",
  "payload": {"status": "ok", "response": {}},
  "event": "phx_reply"
}
```

Key | Type | Value | Description
------------ | ------------ | ------------ | ------------
`topic` | string | "pair:{code}" | the channel just joined
`event` | string | "phx_reply" | joining reply from server
`payload` | object | {"status": "ok"} | joined sucessfully

## Stream

In your joined pair, when orderbook changed, including trading executed, order placed into orderbook, or order cancelled, stream will be pushed down

```json
{
  "topic": "pair:ceteos",
  "event": "report",
  "payload": {
    "trades": [
      {
        "id": 1000,
        "bsn": 129,
        "trend": "up",
        "price": 0.28,
        "volume": 1,
        "amount": 0.28,
        "created_at": 1543398225959
      },
      {
        "id": 1001,
        "bsn": 129,
        "trend": "up",
        "price": 0.29,
        "volume": 1,
        "amount": 0.29,
        "created_at": 1543398225959
      }
    ],
    "depth": {
      "bids": [],
      "asks": [[0.28,"0"], [0.29,"718.0"]]
    },
    "bsn": 129
  }
}
```

**Trade:**

Key | Description
------------ | ------------
`id` | native trade id
`bsn` | orderbook serial number, +1 whenever orderbook changes
`trend` | `"up"` means a proactive buying <br /> `"down"` means a proactive selling
`price` | trading price
`volume` | trading volume
`amount` | trading amount, equals price * volume

If no trades in this pushing, the value of key `"trades"` will be `[]`

**Depth:**

Key | Description
------------ | ------------
`asks` | price and quantity in depth asks
`bids` | price and quantity in depth bids
`[price, quantity]` | price, quantity after changed, "0" means this price level should be removed
