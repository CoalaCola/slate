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

**GET** 

* **getAddress** -  To obtain a Sygna-Certified address for a user who has already been KYC'd.
* **getAddressStatus** - To query the status of an address  (possible: External, Whitelisted, Banned, Restricted, ...) 

**POST**

* **addUser** - Adds a user to the Sygna whitelist - requires the User to have already gone through the exchange KYC 
* **linkAddress**  - Links an existing Address to an existing Sygna UserID. (Individual Addresses have to be Linked to KYC identities)


# Authentication

> **To authorize, use this code:**

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: <sygnaAPIkey>"
```

Sygna uses API keys to allow access to the API. You can register a new Sygna API key at our [developer portal](http://Sygna.com/developers).

Sygna expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: <sygnaAPIkey>`



#Schema
API access is over **https**. All data is sent and received via **json**.   
Blank fields are included as null instead of being omitted. TLS (v1.2) is supported.

A sample curl command might look like this:

`curl -X GET --header 'Accept: application/json' --header 'Authorization: <sygnaAPIkey>' 'https://api.sygna.com/api/syg/v1/getaddress'`

Or this:

`curl -X POST --header 'Accept: application/json' --header 'Authorization: <sygnaAPIkey>' --header 'Content-Type: application/json' --data '[{"asset": "BTC", "transferReference": "<TX_HASH>:<TX_INDEX>"}]' 'https://api.sygna.com/api/syg/v1/<USER_ID>/transfers/received'`


#Request URLs
The URL for all test requests to the API is:

`https://test.sygna.com/api/syg/v1`

Production server URL for the API is at:

`https://api.sygna.com/api/syg/v1`

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
