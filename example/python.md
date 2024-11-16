# Python 请求数据示例



## 请求K线

`python -m pip install requests`

```python
import requests

url = "https://api.itick.org/stock/kline?region=hk&code=700.HK&kType=1"

headers = {
    "accept": "application/json",
    "token": "you_apikey"
}

response = requests.get(url, headers=headers)

print(response.text)
```



## 请求实时报价

```python
import requests

url = "https://api.itick.org/stock/tick?region=HK&code=700.HK"

headers = {
    "accept": "application/json",
    "token": "you_apikey"
}

response = requests.get(url, headers=headers)

print(response.text)
```



## 订阅实时报价

`pip install websocket-client`

```python
import websocket
import json

# WebSocket服务器的地址
websocket_url = "wss://api.itick.org/sws"

# 用于鉴权
auth_message = {
  "ac":"auth",
  "params":"you_apikey"
}

# 用于订阅的消息格式，这里假设订阅一个名为 "your_channel" 的频道
subscribe_message = {
  "ac":"subscribe",
  "params":"AM.LPL,AM.LPL",
  "types":"depth,quote"
}

def on_open(ws):
    """
    当WebSocket连接打开时调用的函数
    """
    print("WebSocket连接已打开，正在发送鉴权消息...")
    
    # 发送鉴权消息
    ws.send(json.dumps(auth_message))
    
    # 将订阅消息转换为JSON格式并发送
    ws.send(json.dumps(subscribe_message))

def on_message(ws, message):
    """
    当收到WebSocket消息时调用的函数
    """
    print(f"收到消息: {message}")
    # 这里可以根据收到的消息内容进行进一步的处理，比如解析JSON数据等
    data = json.loads(message)
    if "data" in data:
        print(f"数据内容: {data['data']}")

def on_error(ws, error):
    """
    当WebSocket连接出现错误时调用的函数
    """
    print(f"WebSocket错误: {error}")

def on_close(ws, close_status_code, close_msg):
    """
    当WebSocket连接关闭时调用的函数
    """
    print(f"WebSocket连接已关闭，状态码: {close_status_code}，消息: {close_msg}")

if __name__ == "__main__":
    # 创建WebSocket对象并设置回调函数
    ws = websocket.WebSocketApp(websocket_url,
                                on_open=on_open,
                                on_message=on_message,
                                on_error=on_error,
                                on_close=on_close)

    # 启动WebSocket连接，开始监听消息
    ws.run_forever()
```

