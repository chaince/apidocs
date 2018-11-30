# Websocket for Trading

## Join

### Connect

Connect the websocket endpoint **wss://api.chaince.com/websocket**

### Bearer
See [Authentication](./authentication.md)

### Sent
Sent a `join` request to server in json format

*Demo*

```json
{
  "topic": "trading:cnc6666666666666",
  "payload": {"bearer": "eyJhbG..."},
  "event": "phx_join",
  "ref": 1
}
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`topic` | string | yes | "trading:{api_key}" | the channel to join
`payload` | object | yes | {"bearer": bearer} | the bearer to authorize
`event` | string | yes | "phx_join" | set it as is, required by protocol
`ref` | integer | yes | 1 | set it as is, required by protocol

**Reply:**

```json
{
  "topic": "trading:cnc6666666666666",
  "payload": {"status": "ok", "response": {}},
  "event": "phx_reply"
}
```

Key | Type | Value | Description
------------ | ------------ | ------------ | ------------
`topic` | string | "trading:{api_key}" | the channel just joined
`event` | string | "phx_reply" | joining reply from server
`payload` | object | {"status": "ok"} | joined sucessfully

## Stream

When your order involved in any trading, placed into orderbook, or cancelled, stream will be pushed down

```json
{
  "topic": "trading:cnc6666666666666",
  "event": "pair:ceteos",
  "payload": {
    "orders": [
      {
        "id": 79,
        "type": "limit",
        "direction": "ask",
        "state": "active",
        "price": 0.29,
        "quantity": 62,
        "volume": 38,
        "client_order_id": null,
        "created_at": 1540209460784,
        "updated_at": 1543398225955
      },
      {
        "id": 155,
        "type": "limit",
        "direction": "bid",
        "state": "filled",
        "price": 0.3,
        "quantity": 0,
        "volume": 2,
        "client_order_id": "my-order-1",
        "created_at": 1543398225950,
        "updated_at": 1543398225955
      }
    ],
    "accounts": {
      "cet": {
        "available": "3298.7988",
        "balance": "3100.7988",
        "locked": "198.0"
      },
      "eos": {
        "available": "832.6890",
        "balance": "601.6890",
        "locked": "231.0"
      }
    }
  }
```

**Order:**

Key | Description
------------ | ------------
`id` | native order id
`state` | `active` or `filled`
`quantity` | quantity remaining
`volume` | volume executed
`created_at` | when order created, unix timestamp with milliseconds
`updated_at` | when last updated, unix timestamp with milliseconds

**Account:**

Key | Description
------------ | ------------
`balance` | total quantity in account, available + locked
`available` | available in account, free to use
`locked` | may be locked by ordering or withdrawal
