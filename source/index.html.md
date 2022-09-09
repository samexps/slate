---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: false

code_clipboard: false

meta:
  - name: description
    content: Documentation for the Magc3Login API
---

# Quickstart

The Web3Magic Login SDK for JavaScript provides a rich set of client-side functionality that:
- Enables you to use the Login Button
- Enables you to use Login Login to lower the barrier for people to get wallet address id from web3wallet server
- Makes it easy to call into Web3Wallet's API.

This example API documentation page was created with [Slate](https://github.com/slatedocs/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Basic Setup

```
npm install @samexps/magiclogin@1.0.3
```

Install magiclogin sdk as npm package

For full list of released packages, please check [Released pacakges](https://github.com/samexps/magiclogin/pkgs/npm/magiclogin)

# Request and Response

> Success Response

When `status: "ok"` then data will be inside `result` attribute

```json
{
  "status": "ok",
  "result": { "data": "some data" },
  "error": null,
}
```

> Error Response

When `status: "error"` then error data will be in "error" attribute

```json
{
  "status": "error",
  "result": null,
  "error": "some error has occurred",
}
```

<aside class="notice">API responses have same structure</aside>

## Initialize Login app with API key and redirect url


> Sample code snippet

```javascript
const apiKey = "";      // get it from Web3Magic Login team
const redirectUrl = ""; // It has to be configured by organization themselves

const config = { apiKey: apiKey, redirectUrl: encodeURIComponent(redirectUrl) }

const Web3Login = new LoginApp(config);
```

> Filename: loginapp.js

```javascript
import LoginApp from '@samexps/magiclogin';

// It has to be obtained from Web3magic Login team
const apiKey = 'replace_with_api_key';

// Redirect URL from config
const redirectUrl = 'https://app.company.com/userId/ABC1234';

const config = {
  apiKey: apiKey,
  redirectUrl: encodeURIComponent(redirectUrl),
}

const Web3Login = new LoginApp(config);

export default Web3Login;
```

Pass configuration object to initialize Login app.

Parameters | Type | Description
---|---|---
apiKey|String| API Key that is created by Web3Magic Login team
redirectUrl|String| Configured by organizatio themselves

## Login by email

```javascript
import LoginApp from './loginapp'

// send email login request
const response = await LoginApp.sendEmail({
  email,
})
```

> Success response

```json
{
    "status": "ok",
    "result": "HhjUkYWpzXiMbhoFIfsYuKlpbduPweVa",
    "error": null
}
```

> Error response

```json
{
    "status": "error",
    "result": null,
    "error": "Invalid Compact JWS"
}
```
Use `app.sendEmail()` to send email login request

Parameter | Default | Description
--------- | ------- | -----------
email | string | Email request


Upon <code>success</code>, it returns a <code>token</code> that can be used to check if email has been
verified by email or not. In sample response <code>HhjUkYWpzXiMbhoFIfsYuKlpbduPweVa</code> is our token.

## Verify email

> Success response

```json
{
    "status": "ok",
    "result": "https://app.company.com/userId/ABC1234?id_wallet_address=0xt5d221a4d4568dad6f83d5fe7aa7b1fe22daf408",
    "error": null
}
```

> Error response

```json
{
    "status": "error",
    "result": null,
    "error": "not confirmed yet"
}
```

Organization has to keep polling to check if user has verified email or not. Organization
can use `LoginApp.checkTokenConfirmation()` to poll for email verification.

**Email Link Expire**
<aside class="warning">
The link in the email is valid for 10 minutes only.
</aside>

**Wallet Address ID**
<aside class="success">
Upon <code>success</code> it returns <code>id_wallet_address</code> appended to <code>redirect_url</code> configured
by organization while setting up SDK. In below response, <code>0xt5d221a4d4568dad6f83d5fe7aa7b1fe22daf408</code>
is wallet address and <code>https://app.company.com/userId/ABC1234</code> is <code>redirect_url</code> conifgured.
</aside>


## QR Code

Generate QR Code that can be rendered in the page

- Use `LoginApp.requestForQRCode()` to request for QR Code. Internally it uses
organization token to make API request.

```javascript
import LoginApp from './loginapp'

// QR Code
const { qrCodeBase64 } = await LoginApp.requestForQRCode()
```

> Success response

```json
{
    "status": "ok",
    "result": {
        "qr_code_base64": "data:image/png;base64,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3lCZKiaVqeKTLtZaa90u1lpr3S7WWmvdfviwikllqnhD5aRiUnlC5aRiUpkqTiqeUJkqpoonVN6omFSmihOVqWJSmSomlanijYo/qWJSmSomlZOKSWWqeKLipGJSeaLiRGVSmSq+6WKttdbtYq211u1irbXW7YcPU5kqJpWpYlI5qXhDZao4UZlUTlROKk5UpoonVKaKE5Wp4pNUpopJZap4QuUNlTcqPknlk1ROKiaVT6o4UZkqTlROKt64WGutdbtYa611u1hrrXWzX/xBKicV36QyVUwqU8UTKk9UfJLKExWTylRxovJGxYnKVHGi8kTFGyonFZPKExWTyknFicoTFZPKVPFJKlPFpDJVfNLFWmut28Vaa63bxVprrZv94gWVqeIJlaliUpkqJpWpYlKZKk5U3qiYVN6oOFH5pIpJ5aRiUvmkiidU/iUVk8pJxaQyVUwqJxWfpPJNFZPKScUnXay11rpdrLXWul2stda62S/+w1SmikllqnhD5YmKJ1TeqHhC5ZMqTlSeqJhUvqniCZWp4kRlqvgklZOKSWWqmFSeqHhC5aRiUpkqPulirbXW7WKttdbtYq211u2Hl1T+pIoTlaniROWNikllUnmi4gmVE5Wp4omKSeVE5aTiCZW/SWWqeELlROWk4kRlqjhR+ZNUpoonVKaKSWWqeONirbXW7WKttdbtYq211u2HD6v4JJWTiidUTiomlTcqJpWp4psqPqliUjmpmFSmipOKSWWqOFGZKiaVk4o3Kt5QOamYVJ6omFSmihOVk4onKk5UpopPulhrrXW7WGutdbtYa611++HLVJ6oeEJlqnii4qRiUnlCZaqYVJ6oOFF5o2JSOal4Q+WJihOVqeIJlTdUpopJZaqYVE4q3qh4QuUJlTdUpooTlanijYu11lq3i7XWWreLtdZatx/+z6hMFU+oPKEyVfxLKiaVE5WTikllqvikiicqJpWp4qRiUjmpOFGZKk4qnlB5QmWqmCqeUHmiYlKZKk4qJpVPulhrrXW7WGutdbtYa611++H/nMpUMak8UTGpnKhMFVPFpHKi8oTKScWkMlVMKlPFExXfpDJVnFRMKm+onKg8UTGpnFRMKicqb1RMKicqU8W/5GKttdbtYq211u1irbXW7Ycvq/gvqZhUJpU3VE4qvknlpGJSOVF5omJSeaJiUjlReaPiiYonVKaKSeWkYlKZKiaVqWJSmSq+SeWk4k+6WGutdbtYa611u1hrrXWzX7yg8idVTConFZPKScWkclLxhMpUMalMFScqT1ScqEwVT6hMFZPKScU3qbxRcaLyRMWkclLxX6YyVZyoPFHxxsVaa63bxVprrdvFWmutm/1irbUWF2uttW4Xa621bhdrrbVuF2uttW4Xa621bhdrrbVuF2uttW4Xa621bhdrrbVuF2uttW4Xa621bhdrrbVuF2uttW4Xa621bhdrrbVu/wMdADPWJoOrrgAAAABJRU5ErkJggg==",
        "mobile_app_link": "https://dev.web3connect.jp/confirm?c=eyJhbGciOiJFUzI1NksifQ.eyJjb25maXJtYXRpb25fdHlwZSI6MSwiYWNjb3VudF90eXBlIjoiZW5kX3VzZXIiLCJjb25maXJtYXRpb25fbWV0aG9kIjoiY29kZSIsInIiOiJodHRwczovL2FwcC5jb21wYW55LmNvbS91c2VySWQvQUJDMTIzNCIsInB1cnBvc2UiOiJjb25maXJtYXRpb24iLCJpYXQiOjE2NjI2NTAwMTQsInN1YiI6ImVuZF91c2VyIiwiaXNzIjoiZGV2LndlYjNjb25uZWN0LmpwIiwiYXVkIjoiY29uZmlybWF0aW9uX3Byb2Nlc3MiLCJleHAiOjE2NjI2NTA2MTR9.majGxyunEa7VVdshlco7PV1MtmScwedi4NLYGKf8PC8f_pks-fSC33lMVOcQZV3zjZHXlvwnGx1RLm7IeTzKXg",
        "end_user_confirmation_token": "eyJhbGciOiJFUzI1NksifQ.eyJjb25maXJtYXRpb25fdHlwZSI6MSwiYWNjb3VudF90eXBlIjoiZW5kX3VzZXIiLCJjb25maXJtYXRpb25fbWV0aG9kIjoiY29kZSIsInIiOiJodHRwczovL2FwcC5jb21wYW55LmNvbS91c2VySWQvQUJDMTIzNCIsInB1cnBvc2UiOiJjb25maXJtYXRpb24iLCJpYXQiOjE2NjI2NTAwMTQsInN1YiI6ImVuZF91c2VyIiwiaXNzIjoiZGV2LndlYjNjb25uZWN0LmpwIiwiYXVkIjoiY29uZmlybWF0aW9uX3Byb2Nlc3MiLCJleHAiOjE2NjI2NTA2MTR9.majGxyunEa7VVdshlco7PV1MtmScwedi4NLYGKf8PC8f_pks-fSC33lMVOcQZV3zjZHXlvwnGx1RLm7IeTzKXg"
    },
    "error": null
}
```

> Error response

```json
{
    "status": "error",
    "result": null,
    "error": "signature verification failed"
}
```