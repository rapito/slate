---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
- <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true
---

# Scope


<aside class="notice">
Please note, this document is not meant to be a final design document, but an illustration of what the service might look like.
</aside>

# API

# Authentication

> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
```shell
curl "https://globohq.com/api/v2/backend_provider/calls/{unique_id}/intake.json" \
  -H "GLOBO-AUTH-HMAC-SHA256 {auth_token}: {signature}"
```

> Make sure to replace `auth_token`, `signature` with your API credentials, and `unique_id` with the call's id

In order to authenticate with the API you must create an API token and Auth Secret to use in the authentication process. 
These are created from an appropriately credentialed user and can be assigned specific API Grants.

This API token and Secret will be used to sign each request using an HMAC-SHA256 based algorithm. 
The API Key will be passed as a header, named Authorization in the following format:

`GLOBO-AUTH-HMAC-SHA256 Auth Token:HMAC Signature.` 

Additionally the following headers will need to be included in the request:

Header | Description
--------- | -----------
DATE | Request Timestamp
Content-MD5 | MD5 of the request body

<aside class="notice">
These two values along with Request Type, Method and URI will be embedded 
in the signature.
</aside>

# Intake


## Answer / Next Question

```shell
curl "https://globohq.com/api/v2/backend_provider/calls/{unique_id}/intake.json" \
  -H "GLOBO-AUTH-HMAC-SHA256 {auth_token}: {signature}"
```

> The GET command returns JSON structured like this:

```json
{
  "next_question": {
    "id": 23,
    "type": "SingleLine",
    "text": "This is the question that should be asked",
    "help": "Text that might be helpful for answering the question",
    "mandatory": true
  }
}
```

> The POST command returns JSON structured like this:

```json
{
  "result": true,
  "next_question": {
    "id": 23,
    "type": "SingleLine",
    "text": "This is the question that should be asked",
    "help": "Text that might be helpful for answering the question",
    "mandatory": true
  }
}
```

This Endpoint will return the next action for the call.
For example, if a multiple choice intake question is next, you will see something like the following in the JSON response.

### HTTP Request

- `GET https://globohq.com/api/v2/backend_provider/calls/{unique_id}/intake.json`
- `POST https://globohq.com/api/v2/backend_provider/calls/{unique_id}/intake.json`

### Query Parameters

Parameter | required | Description
--------- | ------- | -----------
unique_id | true | Unique ID of the call that is being handled.

<aside class="notice">
GET request will only return next available question.
</aside>

<aside class="success">
Remember — All requests must be authenticated!
</aside>

### Request Body Parameters

If these parameters for hte root object `question` are present, then the question will be attempted to be answered. 

Parameter | required | Description
--------- | ------- | -----------
id | true | ID of the question to answer
value | true | answer of the question


