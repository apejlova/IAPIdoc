# CM offline transaction

This flow describes how to initiate transaction and verify it with OTP generated in offline mobile device.
1. [Initiate offline transaction](#initiate-offline-transaction) **`IAPI/initiateTransaction`**
2. [Get OTP from mobile app](#get-otp-from-mobile-app)
3. [Verify CM offline transaction](#verify-cm-offline-transaction) **`IAPI/verifyTransaction`**

### Initiate offline transaction
* initiate authorization from offline mobile instance
* specific parameters setting:
  * `operationType = AUTHORIZATION`
  * `type = PIN` for PIN only authorization, `type = ALT_SECRET` for PIN and biometric authorization, `type = NO_PIN` for confirmation only
  * `.processingOptions.authorizationFlow = OFFLINE`
  * `processingOptions.offlineChallenge = QR` to return data for QR code
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","SMS","CM"] |
| `muid` | user identifier |  true |  cg2t1 |
| `operationType` | type of initiated transaction, deafault value is AUTHORIZATION |  true |  ["AUTHENTICATION","AUTHORIZATION"] |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
| `type` | secret that can be used for transaction verification, secrets hierarchy: PIN > ALT_SECRET > NO_PIN, stronger secret can be used always, default value is PIN |  true |  ["PIN","NO_PIN","ALT_SECRET","INFORMATION_MESSAGE"] |
| `processingOptions.authorizationFlow` | distinguishes how the transaction can be verified, default value is ONLINE_OFFLINE |  true |  ["ONLINE","OFFLINE","ONLINE_OFFLINE"] |
| `transactionData.data` | WYSIWYS transaction data (base64-encoded) |  false |  PFdZU0lXWVMgeG1sbnM9Imh0dHA6Ly9tZXAubW9uZXRwbHVzLmN6IiB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIiB2ZXJzaW9uPSIxLjIiIHhzaTpzY2hlbWFMb2NhdGlvbj0iaHR0cDovL21lcC5tb25ldHBsdXMuY3ogbWVwX3d5c2l3eXNfMV8yLnhzZCI+DQoJPHQ+DQoJCTxsdlNwZWMgbD0iTsOhemV2IHRyYW5zYWtjZSIgbHdzPSIiIHdzPSIiPlDFmWlobMOhxaFlbsOtIGRvIGFwbGlrYWNlPC9sdlNwZWM+DQoJCTxsdiBpZD0iQVBQX0lEIiBsPSJBcGxpa2FjZSI+QkxVRTwvbHY+DQoJCTxsdiBpZD0iVFJBTlNBQ1RJT05fSUQiIGw9IklEIG9wZXJhY2UiPjE2MDcyNjAwMDAwMDA1NzwvbHY+DQoJCTxsdiBpZD0iQVBQTElDQVRJT05fTkFNRSIgbD0iTsOhemV2IGFwbGlrYWNlIj5CTFVFPC9sdj4NCgkJPGx2IGlkPSJUSU1FU1RBTVAiIGw9IkRhdHVtIGEgxI1hcyB0cmFuc2FrY2UiPjI2LjA3LjIwMTYgMTU6MjQ6MjA8L2x2Pg0KCQk8bHYgaWQ9IkNBU0VfTkFNRSIgbD0iTsOhemV2IHRyYW5zYWtjZSI+UMWZaWhsw6HFoWVuw60gZG8gYXBsaWthY2U8L2x2Pg0KCTwvdD4NCjwvV1lTSVdZUz4= |
| `transactionData.locale` | language code according to ISO 639-1 |  false |  cs |
| `transactionData.template` | transformation template |  false |  AUTHENTICATION |

* REST API callback:
**`IAPI/initiateTransaction`**
```
POST http://${BASE_URL}/case-iapi/v1/initiateTransaction
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "CM",
  "operationType": "AUTHORIZATION",
  "type": "PIN",
  "transactionData": {
    "data": "PFdZU0lXWVMgeG1sbnM9Imh0dHA6Ly9tZXAubW9uZXRwbHVzLmN6IiB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIiB2ZXJzaW9uPSIxLjIiIHhzaTpzY2hlbWFMb2NhdGlvbj0iaHR0cDovL21lcC5tb25ldHBsdXMuY3ogbWVwX3d5c2l3eXNfMV8yLnhzZCI+DQoJPHQ+DQoJCTxsdlNwZWMgbD0iTsOhemV2IHRyYW5zYWtjZSIgbHdzPSIiIHdzPSIiPlDFmWlobMOhxaFlbsOtIGRvIGFwbGlrYWNlPC9sdlNwZWM+DQoJCTxsdiBpZD0iQVBQX0lEIiBsPSJBcGxpa2FjZSI+QkxVRTwvbHY+DQoJCTxsdiBpZD0iVFJBTlNBQ1RJT05fSUQiIGw9IklEIG9wZXJhY2UiPjE2MDcyNjAwMDAwMDA1NzwvbHY+DQoJCTxsdiBpZD0iQVBQTElDQVRJT05fTkFNRSIgbD0iTsOhemV2IGFwbGlrYWNlIj5CTFVFPC9sdj4NCgkJPGx2IGlkPSJUSU1FU1RBTVAiIGw9IkRhdHVtIGEgxI1hcyB0cmFuc2FrY2UiPjI2LjA3LjIwMTYgMTU6MjQ6MjA8L2x2Pg0KCQk8bHYgaWQ9IkNBU0VfTkFNRSIgbD0iTsOhemV2IHRyYW5zYWtjZSI+UMWZaWhsw6HFoWVuw60gZG8gYXBsaWthY2U8L2x2Pg0KCTwvdD4NCjwvV1lTSVdZUz4=",
    "locale": "cs",
    "template": "AUTHENTICATION"
  },
  "processingOptions": {
    "authorizationFlow": "OFFLINE",
    "offlineChallange": "QR"
  }
}
{
  "status": "success",
  "data": {
    "caseId": "4by4ambkS5WSXh4sc6E+E6yMeO2eXWDBpiYPc2CXWRPkYUgZpSLGtjdO7I43n+uuxTd0v2LrSk4wJM86NrKIoBBXD577Npr1DYzFSE0K6zG/GFcSU0Mbxz3GhLHNednw",
    "methodSpecific": {
      "qrData": "UVIyfDJ8RkFBOEE1QjN8UElOfDB8MOKUjEJMVUXilIzilIx8MHw4UDEqNVQ8UMWZaWhsw6HFoWVuw60gZG8gYXBsaWthY2U8PEJMVUU8PDF8RGF0dW0gYSDEjWFzIHRyYW5zYWtjZXwyNi4wNy4yMDE2IDE1OjI0OjIwPjF8SUQgdHJhbnNha2NlfDE2MDcyNjAwMDAwMDA1Nw==",
      "cipherPublicKey": "MIHfMA0GCSqGSIb3DQEBAQUAA4HNADCByQKBwQCbA+nA4Oyfe4OiFeYGRK8O02+q3ObJ3IZPhYw7SJ5ULhygpZNhIcL5X0c1c2/yHuVoD7PKmoguQUu5Jj5uRC2ovvC8+X+xPRfohrhw8IXQ/DJC8AqRifCCUWshL8qzA4NNzIDIcMG+gLstSHdcMt6+opQb7AemGPfKiWVYw8wsTI9omkfT5QeMWTGJjBD38DFTLzEua/E56lm4MKDM4rk2PxD0Va0h2aZG7T0F6RwqhM7YYLhbc9LVwr840U9/EfUCAwEAAQ=="
    }
  }
}
```
### Get OTP from mobile app
* either scan QR code or enter PIN (resp. use biometrics) to get OTP from mobile application
### Verify CM offline transaction
* authorization of transaction identified by `caseId`
* `code` is transaction verification code (OTP) obtained from mobile application (previous step)
* `code` can be optionally encrypted with RSA `methodSpecific.cipherPublicKey` from **`IAPI/initiateTransaction`** response (if provided)
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `caseId` | transaction identifier |  false |  41QHE14SDOdId+d+g9isQVRgpkPKRRAoYWcaLVt//BdW4VjjSf0QfEmMMPzRGo6wl1TCcx5GUtGFr8sfh315Tuj4AT/ea4sSyv9z7Tgklo2RhV9zMhDOh7bBI5vp+uPf |
| `code` | transaction verification code |  false |  kuxejDzuNbSh1z6VGzYqo7Bv90IpfRavzGfxBYN9yl6D549zaSawq6+Cb0RDQLUz+vpFCgPBMHs73AQO1TpkVCACO/XiDfAf6P2ad61pPlXN02+L6fARtxXcOqowuM5AdPQioV4Byo1/guSjsT/BGNL0MpIjw5NgMtpB5NNw24+2PYx+8lzZM25NPTNaylTJNXBiCL3kBV/p68hc2p4EDzSSRjgA0uTH1oNMIqyNXXPOFGCKU9RSylrBnwLpCUkq |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/verifyTransaction`**
```
POST http://${BASE_URL}/case-iapi/v1/verifyTransaction
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "CM",
  "caseId": "4by4ambkS5WSXh4sc6E+E6yMeO2eXWDBpiYPc2CXWRPkYUgZpSLGtjdO7I43n+uuxTd0v2LrSk4wJM86NrKIoBBXD577Npr1DYzFSE0K6zG/GFcSU0Mbxz3GhLHNednw",
  "code": "Dl8szvtvXIineDBe8PQmE8XctbVOhfJ0UarMbvrkt3peZDw3U7bSPGW3A0r64U9lerYW/YK9lnZTGDemGM41vec16xTeYF4L/p7ENY5whqTMtcauQd3CTlYBaMOXHtWKFrkQsqgV/sKm3AdAS1O1eG1YytjaGStVT0MxoiccNi2rSNsII8YsXOxMLMaMRcvhSleNaMxb1HbZ9gqnHlJ0II3I7ALiD18YQv17pfR9Hg0a13xajcICjau15bAOjHCX"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.mobile.InstanceInfoCM",
      "instanceId": "88b8bb8d05aa9dbc7981470eb8fe5b0daa33b80f2d8f5d189bf9368c39900f62",
      "state": "ACTIVE",
      "instanceName": "LGE LG-H440n",
      "lastAccess": "2020-01-13T10:22:59Z",
      "threatFlags": "AAAAAAAAAAA=",
      "hwId": "eeb0dba54e565f99-1486128428103--121527461765273515",
      "osVersion": "6.0",
      "deviceModel": "LG-H440n",
      "manufacturer": "LG",
      "platform": "ANDROID"
    }
  }
}
```
