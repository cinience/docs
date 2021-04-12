---
type: docs
title: "Alibaba Cloud RocketMQ binding spec"
linkTitle: "Alibaba Cloud RocketMQ"
description: "Detailed documentation on the Alibaba Cloud RocketMQ binding component"
aliases:
- "/operations/components/setup-bindings/supported-bindings/rocketmq/"
---

## Setup Dapr component

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: bindings.rocketmq
  version: v1
  metadata:
    - name: accessProto
      value: "tcp"
    - name: accessKey
      value: "LTAI4G8KxxxxxxxxxxxxxbwZLBr"
    - name: secretKey
      value: "n5jTL9YxxxxxxxxxxxxaxmPLZV9"
    - name: nameServer
      value: "http://mq-access.mq.aliyuncs.com"
    - name: endpoint
      value: "http://mqrest.cn.aliyuncs.com"
    - name: consumerGroup
      value: "MQ-TCP"
    - name: consumerBatchSize
      value: 1024
    - name: consumerThreadNums
      value: 1
    - name: instanceId
      value: "MQ_INST_"
    - name: nameServerDomain
      value: ""
    - name: retries
      value: 0
    - name: topics
      value: "TOPIC_TEST"
```
{{% alert title="Warning" color="warning" %}}
The above example uses secrets as plain strings. It is recommended to use a secret store for the secrets as described [here]({{< ref component-secrets.md >}}).
{{% /alert %}}
## Spec metadata fields
| Field              | Required | Binding support | Details | Example |
|--------------------|:--------:|--------|--------|---------|
| nameServer                | N        | Input/Output |RocketMQ's name server, optional| `"http://mq-access.mq.aliyuncs.com"`
| endpoint                | Y        | Input/Output |RocketMQ's endpoint, optional, just for HTTP proto | `"http://mqrest.cn.aliyuncs.com"`
| accessProto                | Y        | Input/Output |sdk proto (HTTP or TCP),default TCP| `"tcp"`
| accessKey                | N        | Input/Output |RocketMQ Credentials| `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| secretKey                | N        | Input/Output |RocketMQ Credentials | `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| consumerGroup                | N        | Input |consumer group for RocketMQ's subscribers, suggested to provide | `"MQ-TCP"`
| consumerBatchSize                | N        | Input |consumer group for RocketMQ's subscribers, suggested to provide, just for http proto | `1024`
| consumerThreadNums                | N        | Input |consumer group for RocketMQ's subscribers, suggested to provide, just for cgo proto | `20`
| instanceId                | N       | Input/Output |RocketMQ's namespace, optional | `"MQ_INST_"`
| nameServerDomain                | N        | Input/Output |RocketMQ's name server domain, optional| `"mqrest.cn.aliyuncs.com"`
| retries                | N        | Input/Output |retry times to connect RocketMQ's broker, optional | `0`
| topics                | Y        | Input/Output | topics to subscribe, use delimiter ',' to separate if more than one topics are configured | `"TOPIC1,TOPIC_2"`

## Binding support

This component supports both **input and output** binding interfaces.

This component supports **output binding** with the following operations:
- `create`

## Create an AliCloud RocketMQ
- Follow the instructions [here(en)](https://www.alibabacloud.com/help/doc-detail/200153.htm?spm=a2c63.p38356.b99.177.45c3542eayXM1V) on setting up AliCloud RocketMQ
- Follow the instructions [here(zh-cn)](https://help.aliyun.com/document_detail/200153.html?spm=a2c4g.11186623.6.737.24e97f90px1YRf) on setting up AliCloud RocketMQ

## Specifying a partition key

When invoking the rocketmq binding, its possible to provide an optional partition key by using the `metadata` section in the request body.
You can specifie`rocketmq-topic`,`rocketmq-tag`,`"rocketmq-key"` in `metadata`

Example:

```shell
curl -X POST http://localhost:3500/v1.0/bindings/myrocketmq \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        },
        "metadata": {
          "rocketmq-topic": "topic1",
          "rocketmq-tag": "tag",
          "rocketmq-key": "key1",
        },
        "operation": "create"
      }'
```

## Related links

- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Bindings building block]({{< ref bindings >}})
- [How-To: Trigger application with input binding]({{< ref howto-triggers.md >}})
- [How-To: Use bindings to interface with external resources]({{< ref howto-bindings.md >}})
- [Bindings API reference]({{< ref bindings_api.md >}})
