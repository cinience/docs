---
type: docs
title: "RocketMQ"
linkTitle: "RocketMQ"
description: "Detailed documentation on the RocketMQ pubsub component"
aliases:
- "/operations/components/setup-pubsub/supported-pubsub/setup-rocketmq/"
---

## Component format

```yaml
apiVersion: dapr.io/v1alpha1
kind: Component
metadata:
  name: <NAME>
  namespace: <NAMESPACE>
spec:
  type: pubsub.rocketmq
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
    - name: content-type
      value: "text/plain"
    - name: concurrency
      value: "parallel"
```
{{% alert title="Warning" color="warning" %}}
The above example uses secrets as plain strings. It is recommended to use a secret store for the secrets as described [here]({{< ref component-secrets.md >}}).
{{% /alert %}}

## Spec metadata fields
| Field              | Required | Details | Example |
|--------------------|:--------:|--------|---------|
| nameServer                | N        | rocketmq's name server, optional| `"http://mq-access.mq.aliyuncs.com"`
| endpoint                | Y        | rocketmq's endpoint, optional, just for http proto | `"http://mqrest.cn.aliyuncs.com"`
| accessProto                | Y        |sdk proto (http or tcp),default tcp| `"tcp"`
| accessKey                | N        | rocketmq Credentials| `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| secretKey                | N        | rocketmq Credentials | `"LTAI4G8KxxxxxxxxxxxxxbwZLBr"`
| consumerGroup                | N        | consumer group for rocketmq's subscribers, suggested to provide | `"MQ-TCP"`
| consumerBatchSize                | N        | consumer group for rocketmq's subscribers, suggested to provide, just for http proto | `1024`
| consumerThreadNums                | N        |consumer group for rocketmq's subscribers, suggested to provide, just for cgo proto | `20`
| instanceId                | N       | rocketmq's namespace, optional | `"MQ_INST_"`
| nameServerDomain                | N        |rocketmq's name server domain, optional| `"mqrest.cn.aliyuncs.com"`
| retries                | N        | retry times to connect rocketmq's broker, optional | `0`
| topics                | Y        | topics to subscribe, use delimiter ',' to separate if more than one topics are configured, optional | `"TOPIC1,TOPIC_2"`
| content-type                | N        | msg's content-type eg:"application/cloudevents+json; charset=utf-8", application/octet-stream | `"text/plain"`

## Create an AliCloud Rocketmq
Follow the instructions [here](https://www.alibabacloud.com/help/doc-detail/200153.htm?spm=a2c63.p38356.b99.177.45c3542eayXM1V) on setting up AliCloud Rocketmq
Follow the instructions [here](https://help.aliyun.com/document_detail/200153.html?spm=a2c4g.11186623.6.737.24e97f90px1YRf) on setting up AliCloud Rocketmq


## Per-call metadata fields

### Partition Key

When invoking the rocketmq pub/sub, its possible to provide an optional partition key by using the `metadata` query param in the request url.

You need specifie `rocketmq-tag`,`"rocketmq-key"` in `metadata`

Example:

```shell
curl -X POST http://localhost:3500/v1.0/publish/myrocketmq/myTopic?metadata.rocketmq-tag=?&metadata.rocketmq-key=? \
  -H "Content-Type: application/json" \
  -d '{
        "data": {
          "message": "Hi"
        }
      }'
```

## Related links
- [Basic schema for a Dapr component]({{< ref component-schema >}})
- [Pub/Sub building block]({{< ref pubsub >}})
- Read [this guide]({{< ref "howto-publish-subscribe.md#step-2-publish-a-topic" >}}) for instructions on configuring pub/sub components
