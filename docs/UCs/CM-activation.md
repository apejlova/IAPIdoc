# CM activation

This flow describes steps need to be done prior activation of the first instance of mobile application.
1. [Activate method CM](#activate-method-cm) **`IAPI/activateMethod`**
2. [Activate method ACTIVATION_CODE](#activate-method-activation_code) **`IAPI/activateMethod`**
3. [Get activation code](#get-activation-code) **`IAPI/activationCode`**
4. [Activation flow in mobile app](#activation-flow-in-mobile-app)

### Activate method CM
* the CM method must be activated at first to be allowed to activate mobile instances
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
| `methodSpecific.algType` | algorithm type for calculation of password hash and transaction verification code |  false |  2 |

* REST API callback:
**`IAPI/activateMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/activateMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "CM",
  "methodSpecific": {
    "algType": 2
  }
}
{
  "status": "success",
  "data": {
    "methodSpecific": {
      "salt": "sHHfuUGbOCDUQwJcG0fTej8o+9oHQj6AhGQWDwwtylQ="
    }
  }
}
```
### Activate method ACTIVATION_CODE
* the ACTIVATION_CODE method must at first to be allowed to generate activation codes
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
| `methodSpecific.algType` | algorithm type for calculation of password hash and transaction verification code |  false |  2 |

* REST API callback:
**`IAPI/activateMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/activateMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "ACTIVATION_CODE",
  "methodSpecific": {
    "algType": 2
  }
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "ACTIVATION_CODE:cg2t1:9d4505db-2c8a-406a-98cd-7e457ea7a7ec",
      "state": "ACTIVE"
    }
  }
}
```
### Get activation code
* generate activation code for specified `muid`
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/activationCode`**
```
POST http://${BASE_URL}/case-iapi/v1/activationCode
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "CM",
  "activationType": "FULL"
}
{
  "status": "success",
  "data": {
    "code": "025817",
    "userId": "cg2t1@monetplus.cz",
    "qrData": "UVI1fDF8RTMzMDI4OEJ8Y2cydDFAbW9uZXRwbHVzLmN6fDAyNTgxNw=="
  }
}
```
### Activation flow in mobile app
* continue through activation flow in mobile application
