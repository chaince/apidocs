# Restful Public Quotations

## Common
* The base URI: **https://api.chaince.com**
* **JSON** is used as returned data format along with HTTP status code `200`
* **TEXT** string will be returned in case of exceptions with status `4XX` `5XX`
* Quotations are **PUBLIC** for everyone, no authentication required

## Versioning
* **API Version should be set in HTTP Headers, as is: `accept-version: v1`, case sensitive**
* API version must be specified in **EVERY** request, no default value
* `404 NotFound` will be thrown if API Version not set or wrong set


## Terminology
* `base` means the currency that is the `quantity` of a pair
* `quote` means the currency that is the `price` of a pair


# Endpoints

## Root / Ping

### Root
```
GET /
```
Root path. can be used as a ping

**Response:**
```json
{
  "api": "chaince",
  "version": "v1"
}
```

**Ratelimit:**
60 requests / 60 seconds

## Market

### Pairs in market
```
GET /market/pairs
```
All pairs in market

**Response:**
```json
{
  "ceteos": {
    "base_precision": 1,
    "full_code": "CET/EOS",
    "quote_precision": 4
  },
  "iqeos": {
    "base_precision": 1,
    "full_code": "IQ/EOS",
    "quote_precision": 4
  }
}
```

Key | Description
------------ | ------------
`code` | the full code of this pair
`base_precision` | precision of base currency
`quote_precision` | precision of quote currency

**Ratelimit:**
10 requests / 60 seconds

### Currencies in market
```
GET /market/currencies
```
All currencies in market

**Response:**
```json
[
  "EOS",
  "CET"
]
```

**Ratelimit:**
10 requests / 60 seconds

## Tickers

### Tickers
```
GET /tickers
```
Tickers of all pairs

**Response:**
```json
{
  "ceteos": {
    "change": "16.82%",
    "price": "0.0110",
    "volume": "10325590.5"
  },
  "iqeos": {
    "change": "-3.37%",
    "price": "0.0035",
    "volume": "131297013.1"
  },
}
```

Key | Description
------------ | ------------
`price` | current quote price
`volume` | 24 hours trading volume
`change` | 24 hours change in percent

**Ratelimit:**
20 requests / 60 seconds

### Ticker of a pair
```
GET /tickers/{code}
```
Ticker of a specified pair

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `iqeos`

*Demo*

```
GET /tickers/ceteos
```

**Response:**
```json
{
  "ceteos": {
    "change": "16.82%",
    "price": "0.0110",
    "volume": "10325590.5"
  }
}
```

Key | Description
------------ | ------------
`price` | current quote price
`volume` | 24 hours trading volume
`change` | 24 hours change in percent

**Ratelimit:**
30 requests / 60 seconds, per pair

## Kline / Candlestick

```
GET /klines/{code}
```
Ticker of a specified pair

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `iqeos`

*Demo*

```
GET /tickers/ceteos
```

**Parameters:**

Name | Type | Required | Description
------------ | ------------ | ------------ | ------------
`level` | string | no | One of `"m1", "m5", "m30", "h1", "h4", "d", "w"`, Default: `"d"` <br /> Abbrev: `m`: minute, `h`: hour, `d`: day, `w`: week
`first` | long | no | The time of **first** kline, UTC, Unix timestamp, **WITHOUT** milliseconds
`last` | long | no | The time of **last** kline, UTC, Unix timestamp, **WITHOUT** milliseconds
`limit` | int | no | Scope: `1..500`, Default: `2`

**If `first` and `last` are both set, `last` will be ignored**

*Demo*

```
level=m1&first=1533715680
level=h4&last=1533715680&limit=10
```

**Response:**
```json
[
  {
    "c": "0.0116",
    "h": "0.0126",
    "l": "0.0105",
    "o": "0.0116",
    "t": 1533168000,
    "v": "10517935.1"
  },
  {
    "c": "0.0109",
    "h": "0.0118",
    "l": "0.0108",
    "o": "0.0116",
    "t": 1533254400,
    "v": "3406255.0"
  }
]
```

Key | Description
------------ | ------------
`o` | open
`h` | high
`l` | low
`c` | close
`v` | volume

**Ratelimit:**
30 requests / 60 seconds, per pair
