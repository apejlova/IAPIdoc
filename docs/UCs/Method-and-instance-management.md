# Method and instance management

This section documents API callbacks for management of methods and instances, i.e. manual un/blocking, deactivating.
1. [Get method information](#get-method-information) **`IAPI/methodInfo`**
2. [Block instance](#block-instance) **`IAPI/blockInstance`**
3. [Unblock instance](#unblock-instance) **`IAPI/unblockInstance`**
4. [Deactivate instance](#deactivate-instance) **`IAPI/deactivateInstance`**
5. [Block method](#block-method) **`IAPI/blockMethod`**
6. [Unblock method](#unblock-method) **`IAPI/unblockMethod`**
7. [Deactivate method](#deactivate-method) **`IAPI/deactivateMethod`**

### Get method information
* get information about state and blocking rules of specified method types
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodTypes` | array of types of used methods |  false |  ["PASSWORD"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/methodInfo`**
```
POST http://${BASE_URL}/case-iapi/v1/methodInfo
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodTypes": [
    "PASSWORD"
  ]
}
{
  "status": "success",
  "data": {
    "methodInfo": [
      {
        "methodType": "PASSWORD",
        "state": "ACTIVE",
        "activeInstances": 0,
        "instances": [],
        "failedAttempts": 0,
        "remainingAttempts": 3
      }
    ]
  }
}
```
### Block instance
* manually block specified instance
* method state remains unchanged
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `instanceId` | instance identifier |  false |  PASSWORD:cg2t1:nL6ZlKzC06ecUQV9yQs7ghx37jgutgwa |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/blockInstance`**
```
POST http://${BASE_URL}/case-iapi/v1/blockInstance
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD",
  "instanceId": "PASSWORD:cg2t1:29dc89e8-4711-4517-8fbe-2f810a9732c5"
}
{
  "status": "success",
  "data": {}
}
```
### Unblock instance
* unblock specified instance
* method state remains unchanged
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `instanceId` | instance identifier |  false |  PASSWORD:cg2t1:nL6ZlKzC06ecUQV9yQs7ghx37jgutgwa |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/unblockInstance`**
```
POST http://${BASE_URL}/case-iapi/v1/unblockInstance
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD",
  "instanceId": "PASSWORD:cg2t1:29dc89e8-4711-4517-8fbe-2f810a9732c5"
}
{
  "status": "success",
  "data": {}
}
```
### Deactivate instance
* deactivate specified instance
* deactivated instance cannot be re-activated
* method state remains unchanged
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `instanceId` | instance identifier |  false |  PASSWORD:cg2t1:nL6ZlKzC06ecUQV9yQs7ghx37jgutgwa |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/deactivateInstance`**
```
POST http://${BASE_URL}/case-iapi/v1/deactivateInstance
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD",
  "instanceId": "PASSWORD:cg2t1:29dc89e8-4711-4517-8fbe-2f810a9732c5"
}
{
  "status": "success",
  "data": {}
}
```
### Block method
* manually block method
* instances state remains unchanged but their usage is limited
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/blockMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/blockMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD"
}
{
  "status": "success",
  "data": {}
}
```
### Unblock method
* unblock method
* instances state remains unchanged
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/unblockMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/unblockMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD"
}
{
  "status": "success",
  "data": {}
}
```
### Deactivate method
* deactivate method
* all usable instances are deactivated
* used parameters:

| Parameter | Description | Optional | Value example |
| --- | --- | --- | --- |
| `methodType` | type of used method |  false |  ["PASSWORD","ACTIVATION_CODE","SMS","CM","SPNEGO","TLS_CLIENT"] |
| `muid` | user identifier |  false |  cg2t1 |
| `tenant` | organisation name, if not supplied, default value from configuration is taken |  true |  Monet+ |

* REST API callback:
**`IAPI/deactivateMethod`**
```
POST http://${BASE_URL}/case-iapi/v1/deactivateMethod
{
  "tenant": "Monet+",
  "muid": "cg2t1",
  "methodType": "PASSWORD"
}
{
  "status": "success",
  "data": {}
}
```
