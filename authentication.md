# API Authentication

* `JWT` is used as authentication mechanism
* **`ed25519`** is used as signing algorithm
* Get your `api_key` and `secret_key` from hoo.com
* The `api_key` is the user identification, **16** characters long
* The `secret_key` is the **`ed25519` private key**, **32** characters long

## Payload

Key | Description
------------ | ------------
`key` | the `api_key`
`iat` | issued at, UTC, Unix timestamp, **WITHOUT** milliseconds <br /> also used as `nonce` to prevent replay <br /> the request will be rejected after 10 seconds from `iat`

*Demo*

```json
{"key": "cnc6666666666666", "iat": 1599999999}
```

## Bearer

**Sign** the payload to be the `bearer` using ed25519 private key according to `JWT` specification

*Demo*

Name | Value
------------ | ------------
`payload` | `{"key": "cnc6666666666666", "iat": 1599999999}`
`secret_key` | `"CNC88888888888888888888888888888"`

will generate the `bearer`:

```
eyJhbGciOiJFZDI1NTE5IiwidHlwIjoiSldUIn0.eyJpYXQiOjE1OTk5OTk5OTksImtleSI6ImNuYzY2NjY2NjY2NjY2NjYifQ.RJzhQwRI6g0YZg-Mh201G7aEGcpxm8vN8wf-rgpK6UySeMKRgUHzZV6WLxc93PptrKNb4CLW8XQo48OYR-stDw
```
