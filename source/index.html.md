---
title: Sygna API Spec

language_tabs: # must be one of https://git.io/vQNgJ


toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

The Sygna API allows you to validate the source or recipient of a Blockchain transaction.

**API METHODS:**

_**`GET`**_   
- **getAddress** -  Generates a Sygna-Certified address for a user who has already been KYC'd.  
- **getAddressStatus** - Queries the status of an address (ex: External, Whitelisted, Banned, Restricted, ...) 

_**`POST`**_   
- **addUser** - Adds a user to the Sygna whitelist - requires the User to have already gone through the exchange KYC  
- **linkAddress**  - Links an existing Address to an existing Sygna UserID. (Individual Addresses have to be linked to KYC identities)


#Request URLs
Test requests should use the following endpoint:  
`https://test.sygna.com/api/syg/v1`

Production server URL for the API is located at:  
`https://api.sygna.com/api/syg/v1`

# Authentication

> **To authorize, use this code:**

```shell
# remember to pass the sygnaAPIkey in the header with each request
curl "https://api.sygna.com/api/syg/v1/<request>"
  -H "Authorization: <sygnaAPIkey>"
```

Sygna uses API keys for access control. Register a new Sygna API key through our [developer portal](http://sygna.com/developers).

The API key must be included in the message header **for every request**, using the following format:`Authorization: <sygnaAPIkey>`

API access is over **https**. All data is sent and received via **json**.   
Blank fields are included as null instead of being omitted. TLS (v1.2) is supported.

A sample curl command might look like this:

`curl -X GET --header 'Accept: application/json' --header 'Authorization: <sygnaAPIkey>' 'https://api.sygna.com/api/syg/v1/getaddress'`

Or this:

`curl -X POST --header 'Accept: application/json' --header 'Authorization: <sygnaAPIkey>' --header 'Content-Type: application/json' --data '[{"asset": "BTC", "transferReference": "<TX_HASH>:<TX_INDEX>"}]' 'https://api.sygna.com/api/syg/v1/<USER_ID>/transfers/received'`



# Users

## Add User





Adds a user to the Sygna whitelist - requires the User to have already gone through the exchange KYC

### HTTP Request
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

`POST /users/addUser`

### Request Parameters


| Parameter      | Description                  |
| -------------- | ---------------------------- |
| exchangeUserId | The user id in your exchange |
| name           | User name                    |
| email          | User email                   |

### Response Parameters
> Response Body

```json
[
      {
          "sygnaUserId": <string>
      }
]
```

| Parameter   | Description          |
| ----------- | -------------------- |
| sygnaUserId | The user id in Sygna |

<aside class="success">
Remember — A user first has to be added before his addresses can be Sygna-Certified
</aside>

## Get Sygna Address for the User


When the user wants to transfer value from exchange to Sygna wallet, return the address.

### HTTP Request

`GET /users/{userId}/getAddress/{coinType}`

### URL Parameters

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| userId    | The user id in Sygna                      |
| coinType  | Which crypto of the address (BTC/ETH/BCH) |

### Response Body
> Response Body

```json
{
  "sygnaAddress": <string>
}
```

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



When you want to query the status of an address

### HTTP Request

`GET /addresses/{address}/getStatus`

### Response Body
> Response Body

```json
{
   "status": "External|Whitelisted|Banned|Restricted"
}
```

| Parameter | Description               |
| --------- | ------------------------- |
| status    | The status of the address |
