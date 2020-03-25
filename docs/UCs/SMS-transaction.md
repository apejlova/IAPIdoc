# SMS transaction

This flow describes how to initiate and verify SMS transaction.
1. [Initiate login with SMS](#initiate-login-with-sms) **`IAPI/initiateTransaction`**
2. [Wait for SMS](#wait-for-sms)
3. [Encrypt SMS code with RSA key](#encrypt-sms-code-with-rsa-key)
4. [Verify SMS transaction](#verify-sms-transaction) **`IAPI/verifyTransaction`**

### Initiate login with SMS
* initiate transaction for specified `muid` and `methodType = SMS`
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","SMS","CM"] |
| `muid` | user identifier |  true |  cg2t1 |
| `operationType` | type of initiated transaction, deafault value is AUTHORIZATION |  true |  ["AUTHENTICATION","AUTHORIZATION"] |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
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
  "methodType": "SMS",
  "operationType": "AUTHENTICATION",
  "transactionData": {
    "data": "PFdZU0lXWVMgeG1sbnM9Imh0dHA6Ly9tZXAubW9uZXRwbHVzLmN6IiB4bWxuczp4c2k9Imh0dHA6Ly93d3cudzMub3JnLzIwMDEvWE1MU2NoZW1hLWluc3RhbmNlIiB2ZXJzaW9uPSIxLjIiIHhzaTpzY2hlbWFMb2NhdGlvbj0iaHR0cDovL21lcC5tb25ldHBsdXMuY3ogbWVwX3d5c2l3eXNfMV8yLnhzZCI+DQoJPHQ+DQoJCTxsdlNwZWMgbD0iTsOhemV2IHRyYW5zYWtjZSIgbHdzPSIiIHdzPSIiPlDFmWlobMOhxaFlbsOtIGRvIGFwbGlrYWNlPC9sdlNwZWM+DQoJCTxsdiBpZD0iQVBQX0lEIiBsPSJBcGxpa2FjZSI+QkxVRTwvbHY+DQoJCTxsdiBpZD0iVFJBTlNBQ1RJT05fSUQiIGw9IklEIG9wZXJhY2UiPjE2MDcyNjAwMDAwMDA1NzwvbHY+DQoJCTxsdiBpZD0iQVBQTElDQVRJT05fTkFNRSIgbD0iTsOhemV2IGFwbGlrYWNlIj5CTFVFPC9sdj4NCgkJPGx2IGlkPSJUSU1FU1RBTVAiIGw9IkRhdHVtIGEgxI1hcyB0cmFuc2FrY2UiPjI2LjA3LjIwMTYgMTU6MjQ6MjA8L2x2Pg0KCQk8bHYgaWQ9IkNBU0VfTkFNRSIgbD0iTsOhemV2IHRyYW5zYWtjZSI+UMWZaWhsw6HFoWVuw60gZG8gYXBsaWthY2U8L2x2Pg0KCTwvdD4NCjwvV1lTSVdZUz4=",
    "locale": "cs",
    "template": "AUTHENTICATION"
  }
}
{
  "status": "success",
  "data": {
    "caseId": "x67jDmw/9reeXXajpH9Bv3TLlYI7/rMqxGqL/V7xgLDmfDZUrgzChmrfkw9fvRMmIPViRXZFjRrL6i83hkzj5V6uxrs6hgfUdDnnpQR1vc7zIu3GCf4Mfi06yEqfrerY",
    "methodSpecific": {
      "cipherPublicKey": "MIHfMA0GCSqGSIb3DQEBAQUAA4HNADCByQKBwQCbA+nA4Oyfe4OiFeYGRK8O02+q3ObJ3IZPhYw7SJ5ULhygpZNhIcL5X0c1c2/yHuVoD7PKmoguQUu5Jj5uRC2ovvC8+X+xPRfohrhw8IXQ/DJC8AqRifCCUWshL8qzA4NNzIDIcMG+gLstSHdcMt6+opQb7AemGPfKiWVYw8wsTI9omkfT5QeMWTGJjBD38DFTLzEua/E56lm4MKDM4rk2PxD0Va0h2aZG7T0F6RwqhM7YYLhbc9LVwr840U9/EfUCAwEAAQ=="
    }
  }
}
```
### Wait for SMS
* wait for SMS with verification code sent by IAM
### Encrypt SMS code with RSA key
* optional step
* encrypt code from SMS with RSA `methodSpecific.cipherPublicKey` from **`IAPI/initiateTransaction`** response (if provided)
### Verify SMS transaction
* transaction verification with `code` = (encrypted) SMS code from previous step
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
  "methodType": "SMS",
  "caseId": "x67jDmw/9reeXXajpH9Bv3TLlYI7/rMqxGqL/V7xgLDmfDZUrgzChmrfkw9fvRMmIPViRXZFjRrL6i83hkzj5V6uxrs6hgfUdDnnpQR1vc7zIu3GCf4Mfi06yEqfrerY",
  "code": "IO+72yt/q+7flGWnvtphgV8sP6ViAicDQ5d45jjlDFZ0CUJgDTPirovuZ0StdC83Imcz+sY0x6dRc/Fu0iiL9DcbZBgGn5+WyBBDOKcUq/199bT0TEvrwBXapHRQf1SVwHY87al3fGqxZI3heHuxQP63NWfHDHEXsQ+oNn7hUM87rH8fZL3JkhtuJw3CKJoAtTFv8WpF+tbuW/uH+kMxi5B3PXxXDY/4uD7HaWrRmw73EcmgUGivSeGtOUOtWq4n"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "SMS:cg2t1:3d67e42c-8227-4be1-b441-77a0af3d0fc9",
      "state": "ACTIVE",
      "instanceName": "JMTest - 2020-01-13 11-23-05.386",
      "lastAccess": "2020-01-13T10:23:05Z"
    }
  }
}
```
