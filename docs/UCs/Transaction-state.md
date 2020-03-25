# Transaction state

There two procedures how to determine transaction state.
1. [Polling](#polling) **`IAPI/transactionState`**
2. [Notification](#notification)

### Polling
* get current state of the transaction
* `muid` is optional only in case of anonymous transaction
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `caseId` | transaction identifier |  false |  41QHE14SDOdId+d+g9isQVRgpkPKRRAoYWcaLVt//BdW4VjjSf0QfEmMMPzRGo6wl1TCcx5GUtGFr8sfh315Tuj4AT/ea4sSyv9z7Tgklo2RhV9zMhDOh7bBI5vp+uPf |
| `muid` | user identifier |  true |  cg2t1 |

* REST API callback:
**`IAPI/transactionState`**
```
POST https://${BASE_URL}/case-iapi/v1/transactionState
{
{
  "caseId": "bcZq5SEIuqSvCQELv6F4nrU3PH9xo2aOBUK/Lo9Qz2BbYhjTHwX5hHYN9z7OGoQXOdcv/h95LKXTd4lwMnejdW7Nhg0f5BO/CxSoX82GJ132nXEb9osnWpZgkfzyKe+S",
  "muid": "cg2t1"
}
{
  "status": "success",
  "data": {
    "state": "AUTHORIZED"
  }
}
```

### Notification
* if there is specified property `idport.transaction.${dest_key}.destination: ${notification-URL}` in IDport Config, it is possible to use `${dest_key}` as `notificationDestination` parameter during transaction initiation
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","SMS","CM"] |
| `muid` | user identifier |  true |  cg2t1 |
| `notificationDestination` | destination where the notification of transaction state change is sent | true | ${dest_key} |
| `operationType` | type of initiated transaction, deafault value is AUTHORIZATION |  true |  ["AUTHENTICATION","AUTHORIZATION"] |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
| `type` | secret that can be used for transaction verification, secrets hierarchy: PIN > ALT_SECRET > NO_PIN, stronger secret can be used always, default value is PIN |  true |  ["PIN","NO_PIN","ALT_SECRET"] |
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
  "notificationDestination": "${dest_key}",
  "operationType": "AUTHENTICATION",
  "type": "PIN",
  "transactionData": {
    "data": "PFdZU0lXWVMgeG1sbnM9Imh0dHA6Ly9tZXAubW9uZXRwbHVzLmN6IiB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIiB2ZXJzaW9uPSIxLjIiIHhzaTpzY2hlbWFMb2NhdGlvbj0iaHR0cDovL21lcC5tb25ldHBsdXMuY3ogbWVwX3d5c2l3eXNfMV8yLnhzZCI+DQoJPHQ+DQoJCTxsdlNwZWMgbD0iTsOhemV2IHRyYW5zYWtjZSIgbHdzPSIiIHdzPSIiPlDFmWlobMOhxaFlbsOtIGRvIGFwbGlrYWNlPC9sdlNwZWM+DQoJCTxsdiBpZD0iQVBQX0lEIiBsPSJBcGxpa2FjZSI+QkxVRTwvbHY+DQoJCTxsdiBpZD0iVFJBTlNBQ1RJT05fSUQiIGw9IklEIG9wZXJhY2UiPjE2MDcyNjAwMDAwMDA1NzwvbHY+DQoJCTxsdiBpZD0iQVBQTElDQVRJT05fTkFNRSIgbD0iTsOhemV2IGFwbGlrYWNlIj5CTFVFPC9sdj4NCgkJPGx2IGlkPSJUSU1FU1RBTVAiIGw9IkRhdHVtIGEgxI1hcyB0cmFuc2FrY2UiPjI2LjA3LjIwMTYgMTU6MjQ6MjA8L2x2Pg0KCQk8bHYgaWQ9IkNBU0VfTkFNRSIgbD0iTsOhemV2IHRyYW5zYWtjZSI+UMWZaWhsw6HFoWVuw60gZG8gYXBsaWthY2U8L2x2Pg0KCTwvdD4NCjwvV1lTSVdZUz4=",
    "locale": "cs",
    "template": "AUTHENTICATION"
  },
  "processingOptions": {
    "authorizationFlow": "ONLINE"
  }
}
{
  "status": "success",
  "data": {
    "caseId": "VL30CzJy6keB56sfFCuNSits//oRS4S+QAe92rRTl9DQIMQwIobzPIjwlLOtGS//nPyd2pA/YzA5Gz+CHVdcB5aOTaFi6/5oOTOhZdxfkQkt7kaMcJ3dIZcIaKAJk2x+"
  }
}
```
* if the transaction state is changed, IAPI sends REST callback to specified notification destination
* there must be third-party service listening at the endpoint and processing the callback

```
POST http://${notification-URL}/

{
  "caseId": "VL30CzJy6keB56sfFCuNSits//oRS4S+QAe92rRTl9DQIMQwIobzPIjwlLOtGS//nPyd2pA/YzA5Gz+CHVdcB5aOTaFi6/5oOTOhZdxfkQkt7kaMcJ3dIZcIaKAJk2x+",
  "transactionState": "CANCELED",
  "muid": "cg2t1",
  "notificationDestination": "${dest_key}"
}
```
