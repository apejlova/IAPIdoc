# PASSWORD set up

This flow describes how to set up new password with already active PASSWORD method.

There are several use-cases that can utilize this flow, e.g.:
* password change - [verify the old password](password-transaction) first and then set up new one
* password recovery - set up new password and authorize it by means of other method ([CM authorization](cm-online-transaction), [SMS transaction](sms-transaction))
1. [Get method parameters](#get-method-parameters) **`IAPI/methodParams`**
2. [Calculate password hash](#calculate-password-hash)
3. [Initiate instance of PASSWORD method](#initiate-instance-of-password-method) **`IAPI/initiateInstance`**
4. [Activate instance of PASSWORD method](#activate-instance-of-password-method) **`IAPI/activateInstance`**

### Get method parameters
* obtaining method specific parameters
  * eg. `salt` for `algType = 2`
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/methodParams`**
```
POST http://${BASE_URL}/case-iapi/v1/methodParams
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD"
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
    "value": "0vOfFXbcKR2PQ75rz6dYulSm7Vh/KiLQgKF041WbL0g="
  },
  "name": "JMTest - 2020-01-13 11-23-05.213"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "PASSWORD:cg2t1:1a89be86-41ff-4dba-a929-f7bd4a5bd2e5",
      "state": "INITIATED",
      "instanceName": "JMTest - 2020-01-13 11-23-05.213"
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
  "instanceId": "PASSWORD:cg2t1:1a89be86-41ff-4dba-a929-f7bd4a5bd2e5"
}
{
  "status": "success",
  "data": {
    "instanceInfo": {
      "@type": "cz.monetplus.idport.model.InstanceInfo",
      "instanceId": "PASSWORD:cg2t1:1a89be86-41ff-4dba-a929-f7bd4a5bd2e5",
      "state": "ACTIVE",
      "instanceName": "JMTest - 2020-01-13 11-23-05.213"
    }
  }
}
```
