# PASSWORD activation

This flow describes how to activate PASSWORD method.
1. [Activate method PASSWORD](#activate-method-password) **`IAPI/activateMethod`**
2. [Calculate password hash](#calculate-password-hash)
3. [Initiate instance of PASSWORD method](#initiate-instance-of-password-method) **`IAPI/initiateInstance`**
4. [Activate instance of PASSWORD method](#activate-instance-of-password-method) **`IAPI/activateInstance`**

### Activate method PASSWORD
* the PASSWORD method must be activated at first
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
  "methodType": "PASSWORD",
  "methodSpecific": {
    "algType": 2
  }
}
{
  "status": "success",
  "data": {
    "methodSpecific": {
      "algType": 2,
      "salt": "+v88KHs1CoLQkV02flyOYuyDBx59HXCRXMMWIgJwASk="
    }
  }
}
```
### Calculate password hash
* for `algType = 2`
  * calculation of `password hash = sha256(salt||password)` where `password` is password supplied by user and `salt` is parameter `.data.methodSpecific.salt` from **`IAPI/activateMethod`** response.
### Initiate instance of PASSWORD method
* creating instance in INITIATED state and saving the password hash
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `name` | instance friendly name |  true |  Swagger instance test name |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |
| `methodSpecific.value` | base64-encoded password hash (according to algorithm type) |  true |  BRS2IIsHA/vX+burYewoRgi+DMXvOb+wabBiUMtNNPM= |

* REST API callback:
**`IAPI/initiateInstance`**
```
POST http://${BASE_URL}/case-iapi/v1/initiateInstance
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD",
  "methodSpecific": {
    "value": "JLL1rW5aI9EJgxmrBZCpngGqz+FIgx1+ln1HuqTAGCA="
  },
  "name": "JMTest - 2020-01-13 11-23-05.050"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "PASSWORD:cg2t1:a8968b97-b27d-4880-a8aa-a35ed64be173",
      "state": "INITIATED",
      "instanceName": "JMTest - 2020-01-13 11-23-05.050"
    }
  }
}
```
### Activate instance of PASSWORD method
* confirmation of activation and transition of instance state to ACTIVE
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `instanceId` | instance identifier |  false |  PASSWORD:cg2t1:nL6ZlKzC06ecUQV9yQs7ghx37jgutgwa |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/activateInstance`**
```
POST http://${BASE_URL}/case-iapi/v1/activateInstance
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD",
  "instanceId": "PASSWORD:cg2t1:a8968b97-b27d-4880-a8aa-a35ed64be173"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "PASSWORD:cg2t1:a8968b97-b27d-4880-a8aa-a35ed64be173",
      "state": "ACTIVE",
      "instanceName": "JMTest - 2020-01-13 11-23-05.050"
    }
  }
}
```
