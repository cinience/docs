---
type: docs
title: "Alibaba Cloud DingTalk binding spec"
linkTitle: "Alibaba Cloud DingTalk"
description: "Detailed documentation on the Alibaba Cloud DingTalk binding component"
aliases:
- "/operations/components/setup-bindings/supported-bindings/dingtalk/"
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
  - name: id
    value: "test_webhook_id"
  - name: url
    value: "dingtalk.aliyun.com:8080/post/message"
  - name: secret
    value: "LTAI4G8KxxxxxxxxxxxxxbwZLBr"
```
{{% alert title="Warning" color="warning" %}}
The above example uses secrets as plain strings. It is recommended to use a secret store for the secrets as described [here]({{< ref component-secrets.md >}}).
{{% /alert %}}
## Spec metadata fields
| Field              | Required | Binding support | Details | Example |
|--------------------|:--------:|--------|--------|---------|
| id                | Y        | Input/Output |DingTalk's Webhook id | `"test_webhook_id"`
| url                | Y        | Input/Output |DingTalk's Webhook url | `"dingtalk.aliyun.com:8080/post/message"`
| secret                | N        | Input/Output |the secret of DingTalk's Webhook | `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`

## Binding support

This component supports both **input and output** binding interfaces.

This component supports **output binding** with the following operations:
- `create`
- `get`

## Specifying a partition key

Example:

```shell
curl -X POST http://localhost:3500/v1.0/bindings/mydingtalk \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "operation": "create"
      }'
```

```shell
curl -X POST http://localhost:3500/v1.0/bindings/mydingtalk \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "metadata": {},
        "operation": "get"
      }'
```
## Related links

- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Bindings building block]({{< ref bindings >}})
- [How-To: Trigger application with input binding]({{< ref howto-triggers.md >}})
- [How-To: Use bindings to interface with external resources]({{< ref howto-bindings.md >}})
- [Bindings API reference]({{< ref bindings_api.md >}})
