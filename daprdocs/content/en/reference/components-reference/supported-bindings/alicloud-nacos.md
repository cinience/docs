---
type: docs
title: "nacos binding spec"
linkTitle: "nacos"
description: "Detailed documentation on the nacs binding component"
aliases:
- "/operations/components/setup-bindings/supported-bindings/nacos/"
---

## Setup Dapr component

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.nacos
  version: v1
  metadata:
  - name: nameServer
    value: "console1.nacos.io"
  - name: endpoint
    value: "acm.aliyun.com:8080"
  - name: region
    value: "cn-shanghai"
  - name: namespace
    value: "e525eafa-f7d7-4029-83d9-008937f9d468"
  - name: accessKey
    value: "LTAI4G8KxxxxxxxxxxxxxbwZLBr"
  - name: secretKey
    value: "n5jTL9YxxxxxxxxxxxxaxmPLZV9"
  - name: timeout
    value: 5000
  - name: cacheDir
    value: "/tmp/nacos/cache"
  - name: updateThreadNum
    value: 20
  - name: notLoadCacheAtStart
    value: false
  - name: updateCacheWhenEmpty
    value: false
  - name: username
    value: "username"
  - name: password
    value: "password"
  - name: logDir
    value: "/tmp/nacos/log"
  - name: rotateTime
    value: "1h"
  - name: maxAge
    value: 3
  - name: logLevel
    value: "debug"
  - name: config
    value: "dataID:group"
  - name: watches
    value: "dataID1:group1,dataID2:group2"
```
{{% alert title="Warning" color="warning" %}}
The above example uses secrets as plain strings. It is recommended to use a secret store for the secrets as described [here]({{< ref component-secrets.md >}}).
{{% /alert %}}
## Spec metadata fields
| Field              | Required | Binding support | Details | Example |
|--------------------|:--------:|--------|--------|---------|
| nameServer                | Y        | Input/Output |the name for get Nacos server | `"console1.nacos.io"`
| endpoint                | Y        | Input/Output |the endpoint for get Nacos server addresses | `"acm.aliyun.com:8080"`
| region                | Y        | Input/Output |the regionId for kms | `"cn-shanghai"`
| namespace                | Y        | Input/Output |the namespaceId of Nacos | `"e525eafa-f7d7-4029-83d9-008937f9d468"`
| config                | Y        | Output |A comma separated string of key | `"dataID:group"`
| accessKey                | N        | Input/Output |the AccessKey for kms| `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| secretKey                | N        | Input/Output |the SecretKey for kms | `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| timeout                | N        | Input/Output |timeout for requesting Nacos server, default value is 10000 ms | `1000`
| cacheDir                | N        | Input/Output |the directory for persist nacos service info,default value is current path | `"/tmp/nacos/cache"`
| updateThreadNum                | N        | Input/Output |the number of gorutine for update nacos service info,default value is 20 | `20`
| notLoadCacheAtStart                | N       | Input/Output |not to load persistent nacos service info in CacheDir at start time | `"false"`
| updateCacheWhenEmpty                | N        | Input/Output |update cache when get empty service instance from server | `"false"`
| username                | N        | Input/Output |the password for nacos auth | `"username"`
| password                | N        | Input/Output |the password for nacos auth | `"password"`
| logDir                | N        | Input/Output |the directory for log, default is current path | `"/tmp/nacos/log"`
| rotateTime                | N        | Input/Output |the rotate time for log, eg: 30m, 1h, 24h, default is 24h| `"1h"`
| maxAge                | N        | Input/Output |the max age of a log file, default value is 3 | `3`
| logLevel                | N        | Input/Output |the level of log, it's must be debug,info,warn,error, default value is info | `"info"`
| watches                | N        | Input |A comma separated string of watch keys | `"dataID1:group1,dataID2:group2"`

## Binding support

This component supports both **input and output** binding interfaces.

This component supports **output binding** with the following operations:
- `create`
- `get`

## Create an AliCloud Nacos
Follow the instructions [here](https://www.alibabacloud.com/help/doc-detail/59963.htm?spm=a3c0i.215387.6930108420.8.74d16ce2Mdj7te) on setting up AliCloud nacos.
Follow the instructions [here](https://help.aliyun.com/document_detail/85466.html?spm=a2c4g.11186623.6.550.33551eb02LpHHe) on setting up AliCloud nacos.

## Specifying a partition key

When invoking the nacos binding, its possible to provide an optional partition key by using the `metadata` section in the request body.
You can specifie`config-id` and `config-group` in `metadata`

Example:

```shell
curl -X POST http://localhost:3500/v1.0/bindings/mynacos \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "metadata": {
          "config-id": "key1",
          "config-group": "group1"
        },
        "operation": "create"
      }'
```

```shell
curl -X POST http://localhost:3500/v1.0/bindings/mynacos \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "metadata": {
          "config-id": "key1",
          "config-group": "group1"
        },
        "operation": "get"
      }'
```
## Related links

- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Bindings building block]({{< ref bindings >}})
- [How-To: Trigger application with input binding]({{< ref howto-triggers.md >}})
- [How-To: Use bindings to interface with external resources]({{< ref howto-bindings.md >}})
- [Bindings API reference]({{< ref bindings_api.md >}})
