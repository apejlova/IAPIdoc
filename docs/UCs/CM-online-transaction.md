# CM online transaction

This flow describes how to initiate transaction and verify it in online mobile device.
1. [Initiate online transaction](#initiate-online-transaction) **`IAPI/initiateTransaction`**
2. [Authorize transaction in mobile app](#authorize-transaction-in-mobile-app)

### Initiate online transaction
* initiate authorization with online mobile instance
* specific parameters setting:
  * `operationType = AUTHORIZATION`
  * `type = PIN` for PIN only authorization, `type = ALT_SECRET` for PIN and biometric authorization, `type = NO_PIN` for confirmation only (if CM used as second factor)
  * `.processingOptions.authorizationFlow = ONLINE`
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
  "type": "NO_PIN",
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
    "caseId": "EpKQG2fqdJ5hKQ/tJ8X6tAZFPYReDyRUA2rtSpPNQNx06gDOE3ODvw6YPXb+aeJOo3dq7nzxlsAMAL7l1er9SGs64bPOjIGBkAGse+gtKZb3TQtsyG5T0lFugr60+lVf"
  }
}
```
### Authorize transaction in mobile app
* authorize transaction in mobile application (push notification is sent)
