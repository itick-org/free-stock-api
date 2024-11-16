# 外汇 WebSocket 文档

iTick.org Forex WebSocket API 提供全球最新外汇数据的流式访问。 您可以通过以操作形式发送指令来指定要使用的频道。当您订阅的频道中发生事件时，我们的 WebSockets 会发出事件以通知您。

我们的 WebSocket API 基于授权，授权可控制您可以连接到哪些 WebSocket 集群以及您可以访问哪些类型的数据。 您可以登录查看包含您的 API 密钥并根据您的授权进行个性化的示例。

## 第 1 步：连接

使用高级计划，您将能够使用单个连接到集群。如果另一个连接同时尝试连接到集群，则当前连接将被断开。 如果您需要同时连接到此集群的更多连接，您可以联系支持人员。

连接到集群：

```shell
wscat -c wss://api.itick.org/fws
```

连接后您将收到以下消息：

```json json
{
  "code":1,
  "msg": "Connected Successfully"
}
```

<br />

## 第 2 步：验证

您必须先进行身份验证，然后才能提出任何其他请求。

```json
{
  "ac":"auth",
  "params":"dVEIODs9rmbxOtFJAL_SRvwLXjmddLKg"
}
```

验证成功后，您将收到以下消息：

```json
{
  "code":1,
  "resAc":"auth",
  "msg": "authenticated"
}
```

验证失败，会断开连接，流程终止

```json
{
  "code":0,
  "resAc":"auth",
  "msg": "auth failed"
}
```

<br />

## 第 3 步：订阅

验证身份后，即可请求流。您可以在同一请求中请求多个流。

```json
{
  "ac":"subscribe",
  "params":"EURUSD,GBPUSD",
  "types":"quote"
}
```

> params：标的Code，支持订阅多个  
> types: 订阅的类型，depth盘口、quote报价

<br />

订阅成功返回内容。

```json
{
  "code":1,
  "resAc":"subscribe",
  "msg": "subscribe Successfully"
}
```

<br />

订阅失败返回内容。如下：分别是超出套餐计划最大数量，订阅参数错误。

```json
{
  "code":0,
  "resAc":"subscribe",
  "msg": "exceeding the maximum subscription limit"
}
```

```json
{
  "code":0,
  "resAc":"subscribe",
  "msg": "cannot be resolved action"
}
```

<br />

## 第 4 步：响应内容

iTick.org WebSocket 客户端必须能够每秒处理许多传入消息。由于 WebSocket 协议的性质，如果客户端从服务器获取消息的速度很慢，iTick.org 的服务器必须缓冲消息，并以客户端可以接收的速度发送消息。如果客户端长时间以太慢的速度消费消息， iTick.org的服务器端缓冲区可能会变得太大。如果发生这种情况，iTick.org 将终止 WebSocket 连接。如果您经常遇到这种情况，请考虑订阅较少的符号或频道。

订阅成功后数据按照如下内容发送。

### 报价响应内容

```json
{
    "code": 1,
    "data": {
        "s": "EURUSD",
        "ld": 225.215,
        "o": 226.27,
        "h": 226.92,
        "l": 224.44,
        "t": 1731689407000,
        "v": 16742235,
        "tu": 3774688301.452,
        "ts": 0,
        "type": "quote"
    }
}
```

<br />

### 盘口响应内容

```json
{
    "code": 1,
    "data": {
        "s": "EURUSD",
        "a": [
            {
                "po": 1,
                "p": 3034.01,
                "v": 10.6023,
                "o": 10.6023
            }
        ],
        "b": [
            {
                "po": 1,
                "p": 3034,
                "v": 20.9758,
                "o": 20.9758
            }
        ],
        "type": "depth"
    }
}
```

<br />

### K线响应内容

```json
{
    "code": 0,
    "msg": null,
    "data": {
        "s":"EURUSD",
        "t":1,
        "k":
        {
            "tu": 157513,
            "c": 3059.39,
            "t": 1731660060000,
            "v": 28,
            "h": 3061.41,
            "l": 3055.24,
            "o": 3055.36
        }
     }
}
```

> t Kline 周期： 周期 1分钟、2五分钟、3十分钟、4三十分钟、5一小时、6两小时、7四小时、8一天、9一周、10一月

<br />

## 第 5 步：保持心跳

客户端向服务器发送

```json
{
  "ac":"ping",
  "params":"1731688569840"
}
```

服务端向客户端发送

```json
{
  "resAc":"pong",
  "data": {"params":"1731688569840"}
}
```

> ping、pong的时间戳需要保持一致
