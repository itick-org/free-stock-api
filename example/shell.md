# Shell 请求数据示例



## 请求K线

```shell
curl --request GET \
     --url 'https://api.itick.org/stock/kline?region=hk&code=700.HK&kType=1' \
     --header 'accept: application/json' \
     --header 'token: you_apikey'
```



## 请求实时报价

```shell
curl --request GET \
     --url 'https://api.itick.org/stock/tick?region=HK&code=700.HK' \
     --header 'accept: application/json' \
     --header 'token: you_apikey'
```



## 订阅实时报价

```shell
# 链接到 WebSocket
wscat -c wss://api.itick.org/sws

# 发送鉴权
{
  "ac":"auth",
  "params":"you_apikey"
}

# 发起订阅
{
  "ac":"subscribe",
  "params":"AM.LPL,AM.LPL",
  "types":"depth,quote"
}
```

