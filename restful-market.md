# Restful APIs for Market

## Pairs in market
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
`full_code` | the full code of this pair
`base_precision` | precision of base currency
`quote_precision` | precision of quote currency

**Ratelimit:**
10 requests / 60 seconds

## Currencies in market
```
GET /market/currencies
```
All currencies in market

**Response:**
```json
{
  "CET": {
    "contract_address": "eosiochaince"
  },
  "EOS": {
    "contract_address": "eosio.token"
  },
  "ETH": {
    "contract_address": null
  }
}
```

Key | Description
------------ | ------------
`contract_address ` | contract address of eos token

**Ratelimit:**
10 requests / 60 seconds
