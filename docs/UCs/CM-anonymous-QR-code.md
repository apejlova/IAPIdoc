# CM anonymous QR code

This flow describes how to initiate transaction and obtain data for anonymous QR code. There are more ways how to determine [transaction state](transaction-state) to determine if the anonymous transaction is already acquired.
1. [Initiate transaction for anonymous QR code](#initiate-transaction-for-anonymous-qr-code) **`IAPI/initiateTransaction`**
2. [Scan and authorize transaction in mobile app](#scan-and-authorize-transaction-in-mobile-app)

### Initiate transaction for anonymous QR code
* initiate anonymous transaction (aka anonymous QR code) for authorization with online mobile instance
* specific parameters setting:
  * `operationType = AUTHORIZATION`
  * `type = PIN` for PIN only authorization, `type = ALT_SECRET` for PIN and biometric authorization
  * `.processingOptions.authorizationFlow = ONLINE`
  * `muid` is missing => anonymous transaction
 * in response `methodSpecific.qrData` is base64-encoded text data for QR code
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","SMS","CM"] |
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
  "methodType": "CM",
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
    "caseId": "oA3AqI5G8NyzU5zIrdSfTd96YOrTNhQRYnUF7If3tX21lYFnZVWKfjAJDrvA+LMZsofVTZAI7IoaJJg3J2w+Y+uO2vaBDlo5LMz4Z0Wx1Ry9FhKLHOsl5yCg2WasBbxL",
    "methodSpecific": {
      "qrData": "UVIxfDJ8NTFCNzg3Q0N8MXxvQTNBcUk1RzhOeXpVNXpJcmRTZlRkOTZZT3JUTmhRUlluVUY3SWYzdFgyMWxZRm5aVldLZmpBSkRydkErTE1ac29mVlRaQUk3SW9hSkpnM0oydytZK3VPMnZhQkRsbzVMTXo0WjBXeDFSeTlGaEtMSE9zbDV5Q2cyV2FzQmJ4THw4UDEqNEI="
    }
  }
}
```
### Scan and authorize transaction in mobile app
* scan QR code with help of mobile application and authorize transaction
