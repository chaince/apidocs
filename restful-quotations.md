# Restful Public Quotations

## Endpoints

[Tickers](#tickers)

[Ticker](#ticker)

[Klines](#klines)

## Tickers

```
GET /tickers
```
Tickers of all pairs

**Response:**
```json
{
  "ceteos": {
    "base_contract": "eosiochaince",
    "price": "0.0110",
    "code": "CET/EOS",
    "change": "16.82%",
    "high": "0.0118",
    "low": "0.0102",
    "volume": "10325590.5"
  },
  "iqeos": {
    "base_contract": "everipediaiq",
    "price": "0.0035",
    "code": "IQ/EOS",
    "change": "-3.37%",
    "high": "0.0040",
    "low": "0.0032",
    "volume": "131297013.1"
  },
}
```

Key | Description
------------ | ------------
`base_contract` | contract address of base currency
`price` | current quote price
`code` | full code
`change` | 24 hours change in percent
`high` | highest price in past 24 hours
`low` | lowest price in past 24 hours
`volume` | trading volume in past 24 hours

**Ratelimit:**
20 requests / 60 seconds

## Ticker
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
    "volume": "10325590.5",
    "high": "0.0040",
    "low": "0.0032"
  }
}
```

Key | Description
------------ | ------------
`price` | current quote price
`volume` | 24 hours trading volume
`change` | 24 hours change in percent
`high` | 24 hours highest price
`low` | 24 hours lowest price

**Ratelimit:**
30 requests / 60 seconds, per pair

## Klines

```
GET /klines/{code}
```
Klines of a specified pair

**Resouces:**

Name | Description
------------ | ------------
`code` | pair code, no slash '/', lowercase, ie. `ceteos`, `iqeos`

*Demo*

```
GET /klines/ceteos
```

**Parameters:**

Name | Type | Required | Constraint | Description
------------ | ------------ | ------------ | ------------ | ------------
`level` | string | no | one of `"m1", "m5", "m30", "h1", "h4", "d", "w"`, <br /> default: `"d"` | abbrev: `m`: minute, `h`: hour, `d`: day, `w`: week
`first` | long | no | UTC, Unix timestamp, **WITHOUT** milliseconds | the time of **first** kline
`last` | long | no | UTC, Unix timestamp, **WITHOUT** milliseconds | the time of **last** kline
`limit` | int | no | scope: `1..500`, default: `2` | max klines returned

**If `first` and `last` are both set, `last` will be ignored**

*Demo*

```
level=m1&first=1533715680
```
```
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
`t` | time
`o` | open
`h` | high
`l` | low
`c` | close
`v` | volume

**Ratelimit:**
30 requests / 60 seconds, per pair
