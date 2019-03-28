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

<aside class="warning">
All the production and test api are still in development
</aside>

The Sygna API allows you to validate the source or recipient of a Blockchain transaction.

**API METHODS:**

_**`GET`**_   
- **getAddress** -  Generates a Sygna-Certified address for a user who has already been KYC'd.  
- **getAddressStatus** - Queries the status of an address (ex: External, Whitelisted, Banned, Restricted, ...) 

_**`POST`**_   
- **addUser** - Adds a user to the Sygna whitelist - requires the User to have already gone through the exchange KYC  
- **linkAddress**  - Links an existing Address to an existing Sygna UserID. (Individual Addresses have to be linked to KYC identities)

<aside class="success">
Note — (API <b>not currently active</b> as it is being worked on)
</aside>
#Request URLs
Test requests should use the following endpoint:  
`https://test.sygna.com/api/syg/v1`

Production server URL for the API is located at:  
`https://api.sygna.com/api/syg/v1`

# Authentication
NOTE: Still in progress

We are planning to follow [SAML 2.0](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) protocol as SSO solution.
The implementation is still being worked on and will be updated at a later time

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

## getAddress

Request a Sygna Address (for existing Sygna user)  

_usage_: When a user wants a Sygna-certified address (ex: to move funds into a Sygna-certified address to facilitate future Sygna transaction)

### HTTP Request

`GET /users/{userId}/getAddress/{coinType}`

### URL Parameters

| Parameter | Description                               |
| --------- | ----------------------------------------- |
| userId    | The user id in Sygna                      |
| coinType  | Crypto address format (BTC/ETH/BCH)       |

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

`POST /users/{sygnaUserId}/linkAddress`

### URL Parameters

| Parameter | Description          |
| --------- | -------------------- |
| sygnaUserId    | The user id in Sygna |

### Request Body

| Parameter       | Description                               |
| --------------- | ----------------------------------------- |
| coinType        | Crypto address format (BTC/ETH/BCH)       |
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
