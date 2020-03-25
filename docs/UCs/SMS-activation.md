# SMS activation

This flow describes how to activate SMS method.
1. [Activate method SMS](#activate-method-sms) **`IAPI/activateMethod`**
2. [Set instance property](#set-instance-property) **`IAPI/setInstanceProperty`**

### Activate method SMS
* the SMS method must be activated at first
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/activateMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/activateMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "SMS"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "SMS:cg2t1:3d67e42c-8227-4be1-b441-77a0af3d0fc9",
      "state": "ACTIVE"
    }
  }
}
```
### Set instance property
* optional step
* setting of instance parameters:
  * `name` - change default friendly name of instance
  * `validTo` - change default instance expiration date and time in UTC format
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `instanceId` | instance identifier |  false |  PASSWORD:cg2t1:nL6ZlKzC06ecUQV9yQs7ghx37jgutgwa |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `name` | instance friendly name |  true |  Swagger instance test name |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/setInstanceProperty`**
```
POST http://${BASE_URL}/case-iapi/v1/setInstanceProperty
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "SMS",
  "name": "JMTest - 2020-01-13 11-23-05.386",
  "instanceId": "SMS:cg2t1:3d67e42c-8227-4be1-b441-77a0af3d0fc9"
}
{
  "status": "success",
  "data": {}
}
```
