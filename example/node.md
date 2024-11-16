# Node 请求数据示例





## 请求K线

```node
const http = require('https');

const options = {
  method: 'GET',
  hostname: 'api.itick.org',
  port: null,
  path: '/stock/kline?region=hk&code=700.HK&kType=1',
  headers: {
    accept: 'application/json',
    token: 'you_apikey'
  }
};

const req = http.request(options, function (res) {
  const chunks = [];

  res.on('data', function (chunk) {
    chunks.push(chunk);
  });

  res.on('end', function () {
    const body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```



## 请求实时报价

```shell
const http = require('https');

const options = {
  method: 'GET',
  hostname: 'api.itick.org',
  port: null,
  path: '/stock/tick?region=HK&code=700.HK',
  headers: {
    accept: 'application/json',
    token: 'you_apikey'
  }
};

const req = http.request(options, function (res) {
  const chunks = [];

  res.on('data', function (chunk) {
    chunks.push(chunk);
  });

  res.on('end', function () {
    const body = Buffer.concat(chunks);
    console.log(body.toString());
  });
});

req.end();
```



## 订阅实时报价

`npm install ws`

```shell
const WebSocket = require('ws');
const url = 'wss://api.itick.org/sws';

# 用于鉴权
const authMessage = {
  "ac":"auth",
  "params":"you_apikey"
}

# 用于订阅的消息格式，这里假设订阅一个名为 "your_channel" 的频道
const subscribeMessage = {
  "ac":"subscribe",
  "params":"AM.LPL,AM.LPL",
  "types":"depth,quote"
}

// 创建WebSocket连接
const ws = new WebSocket(url);

ws.on('open', () => {
    console.log('WebSocket连接已打开，正在发送鉴权消息...');
    
    # 发送鉴权消息
    ws.send(JSON.stringify(authMessage));
    
    // 将订阅消息转换为JSON格式并发送
    ws.send(JSON.stringify(subscribeMessage));
});

ws.on('message', (message) => {
    console.log('收到消息:', message);
    // 这里可以根据收到的消息内容进行进一步的处理，比如解析JSON数据等
    const data = JSON.parse(message);
    if (data.hasOwnProperty('data')) {
        console.log('数据内容:', data['data']);
    }
});

ws.on('error', (error) => {
    console.log('WebSocket错误:', error);
});

ws.on('close', (code, reason) => {
    console.log('WebSocket连接已关闭，代码:', code, '原因:', reason);
});
```

