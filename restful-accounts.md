# Restful APIs for Accounts

## Authorization

### Bearer
See [Authentication](./authentication.md)

### Authorization
Put the `Bearer` string `eyJhbG...` into http header `Authorization: Bearer eyJhbG...`

## Endpoints

[Get pair accounts](#get-pair-accounts)

[Get currency account](#get-currency-account)

## Get pair accounts
```
GET /accounts/pair/{code}
```
Get accounts by pair code

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `eosusdt`

*Demo*

```
GET /accounts/pair/ceteos
```

**Response:**
```json
{
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
```

Key | Description
------------ | ------------
`balance` | total quantity in account, available + locked
`available` | available in account, free to use
`locked` | may be locked by order or withdrawal

**Ratelimit:**
10 requests / 60 seconds, per pair

## Get currency account
```
GET /accounts/currency/{code}
```
Get accounts by currency code

**Resouces:**

Name | Description
------------ | ------------
`code` | currency code, lowercase, ie. `cet`, `eos`

*Demo*

```
GET /accounts/currency/cet
```

**Response:**
```json
{
  "cet": {
    "available": "3298.7988",
    "balance": "3100.7988",
    "locked": "198.0"
  }
}
```

Key | Description
------------ | ------------
`balance` | total quantity in account, available + locked
`available` | available in account, free to use
`locked` | may be locked by order or withdrawal

**Ratelimit:**
10 requests / 60 seconds, per currency

## Get all accounts
```
GET /accounts/currencies
```
Get all currencies accounts

**Response:**
```json
{
  "cet": {
    "available": "3298.7988",
    "balance": "3100.7988",
    "locked": "198.0"
  },
  "eos": {
    "available": "107.5360",
    "balance": "90.5100",
    "locked": "17.0260"
  }
}
```

Key | Description
------------ | ------------
`balance` | total quantity in account, available + locked
`available` | available in account, free to use
`locked` | may be locked by order or withdrawal

**Ratelimit:**
240 requests / 60 seconds
