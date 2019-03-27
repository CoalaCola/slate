---
title: Sygna API Spec

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

The Sygna API allows you to validate the source and recipients of a Blockchain transaction.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

#Schema
All API access is over HTTPS, and all data is sent and received in JSON. Blank fields are included as null instead of being omitted. TLS (v1.2) is supported.

A sample curl command might look like this:

`curl -X GET --header 'Accept: application/json' --header 'Token: <API_KEY>' 'https://test.sygna.com/api/syg/v1/get_address'`

Or this:

`curl -X POST --header 'Accept: application/json' --header 'Token: <API_KEY>' --header 'Content-Type: application/json' --data '[{"asset": "BTC", "transferReference": "<TX_HASH>:<TX_INDEX>"}]' 'https://test.sygna.com/api/syg/v1/<USER_ID>/transfers/received'`


#Request URLs
The URL for all test requests to the API is:

`https://test.sygna.com/api/syg/v1`

Production server URL for the API is at:

`https://api.sygna.com/api/syg/v1`

# Users

## Add User

> Request Body

```json
[
      {
          "exchangeUserId": <string>,
          "name": <string>,
          "email": <string>
      }
]
```

> Response Body

```json
[
      {
          "sygnaUserId": <string>
      }
]
```

Adds a user to the Sygna whitelist - requires the User to have already gone through the exchange KYC

### HTTP Request

`POST /users/addUser`

### Request Parameters

| Parameter      | Description                  |
| -------------- | ---------------------------- |
| exchangeUserId | The user id in your exchange |
| name           | User name                    |
| email          | User email                   |

### Response Parameters

| Parameter   | Description          |
| ----------- | -------------------- |
| sygnaUserId | The user id in Sygna |

<aside class="success">
Remember — Add user before in the beginning service
</aside>

## Get Sygna Address for the User

> Response Body

```json
{
  "sygnaAddress": <string>
}
```

When the user wants to transfer value from exchange to Sygna wallet, return the address.

### HTTP Request

`GET /users/{userId}/getAddress/{coinType}`

### URL Parameters

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| userId    | The user id in Sygna                      |
| coinType  | Which crypto of the address (BTC/ETH/BCH) |

### Response Body

| Parameter    | Description                      |
| ------------ | -------------------------------- |
| sygnaAddress | The user address in Sygna wallet |

## Link Address

> Request Body

```json
[
      {
          "coinType": <string>, #BTC|ETH...
          "exchangeAddress": <string>
      }
]
```

Links an existing Address to an existing Sygna UserID.

### HTTP Request

`POST /users/{userId}/linkAddress`

### URL Parameters

| Parameter | Description          |
| --------- | -------------------- |
| userId    | The user id in Sygna |

### Request Body

| Parameter       | Description                               |
| --------------- | ----------------------------------------- |
| coinType        | Which crypto of the address (BTC/ETH/BCH) |
| exchangeAddress | The address from exchange wallet          |

###

# Address

## Get Address Status

> Response Body

```json
{
   "status": "External|Whitelisted|Banned|Restricted"
}
```

When you want to query the status of an address

### HTTP Request

`GET /addresses/{address}/getStatus`

### Response Body

| Parameter | Description               |
| --------- | ------------------------- |
| status    | The status of the address |
