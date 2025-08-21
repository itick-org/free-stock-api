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
websocket_url = "wss://api.itick.org/stock"

# 替换为你的实际API密钥
api_key = "your_actual_apikey_here"

# 请求头部参数：通过header传递token鉴权，替代消息体鉴权
headers = {
    "token": api_key
}

# 用于订阅的消息格式（仅保留必要的订阅逻辑）
subscribe_message = {
    "ac": "subscribe",
    "params": "AAPL$US",
    "types": "depth,quote"
}

def on_open(ws):
    """WebSocket连接打开后，直接发送订阅消息（无需鉴权消息）"""
    print("WebSocket连接已打开，正在发送订阅消息...")
    # 发送订阅消息
    ws.send(json.dumps(subscribe_message))

def on_message(ws, message):
    """接收并解析WebSocket消息"""
    print(f"收到消息: {message}")
    try:
        data = json.loads(message)
        if "data" in data:
            print(f"数据内容: {data['data']}")
    except json.JSONDecodeError:
        print("消息解析失败，非有效JSON格式")

def on_error(ws, error):
    """捕获WebSocket连接错误"""
    print(f"WebSocket错误: {error}")

def on_close(ws, close_status_code, close_msg):
    """处理WebSocket连接关闭"""
    print(f"WebSocket连接已关闭，状态码: {close_status_code}，消息: {close_msg}")

if __name__ == "__main__":
    # 创建WebSocket对象，核心是通过header参数传入token鉴权
    ws = websocket.WebSocketApp(
        websocket_url,
        on_open=on_open,
        on_message=on_message,
        on_error=on_error,
        on_close=on_close,
        header=headers  # 鉴权关键：握手阶段携带token头部
    )

    # 启动连接并持续监听消息
    ws.run_forever()
```

