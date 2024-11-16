# Go 请求数据示例





## 请求K线

```shell
package main

import (
	"fmt"
	"net/http"
	"io"
)

func main() {

	url := "https://api.itick.org/stock/kline?region=hk&code=700.HK&kType=1"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("token", "you_apikey")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := io.ReadAll(res.Body)

	fmt.Println(string(body))

}
```



## 请求实时报价

```shell
package main

import (
	"fmt"
	"net/http"
	"io"
)

func main() {

	url := "https://api.itick.org/stock/tick?region=HK&code=700.HK"

	req, _ := http.NewRequest("GET", url, nil)

	req.Header.Add("accept", "application/json")
	req.Header.Add("token", "you_apikey")

	res, _ := http.DefaultClient.Do(req)

	defer res.Body.Close()
	body, _ := io.ReadAll(res.Body)

	fmt.Println(string(body))

}
```



## 订阅实时报价

确保你已经安装了`golang.org/x/net/websocket`包。如果你的 Go 环境支持`go get`命令，可以通过以下命令进行安装：

`go get golang.org/x/net/websocket`

```shell
package main

import (
    "fmt"
    "golang.org/x/net/websocket"
    "io"
    "log"
    "net/url"
)

// WebSocket服务器的地址
const websocketServerURL = "wss://api.itick.org/sws"

// 授权信息
type AuthMessage struct {
    Ac  string `json:"ac"`
    Params string `json:"params"`
}

// 用于订阅的消息格式，这里假设订阅一个名为 "your_channel" 的频道
type SubscribeMessage struct {
    Ac  string `json:"ac"`
    Params string `json:"params"`
    Types string `json:"types"`
}

func main() {

    // 用于鉴权
    authMessage := AuthMessage{
        Ac:  "auth",
        Params: "you_apikey",
    }
    
    // 创建订阅消息结构体实例
    subscribeMessage := SubscribeMessage{
        Ac:  "subscribe",
        Params: "AM.LPL,AM.LPL",
        Types： "depth,quote",
    }

    // 解析WebSocket服务器地址为URL对象
    u, err := url.Parse(websocketServerURL)
    if err!= nil {
        log.Fatal(err)
    }

    // 创建WebSocket连接
    ws, err := websocket.Dial(u.String(), "", websocketServerURL)
    if err!= nil {
        log.Fatal(err)
    }
    defer ws.Close()
    
    // 用于鉴权
    err = websocket.JSON.Send(ws, authMessage)
    if err!= nil {
        log.Fatal(err)
    }

    // 将订阅消息转换为JSON格式并发送
    err = websocket.JSON.Send(ws, subscribeMessage)
    if err!= nil {
        log.Fatal(err)
    }

    // 循环接收服务器发送的消息
    for {
        var message string
        err := websocket.JSON.Receive(ws, &message)
        if err == io.EOF {
            break
        } else if err!= nil {
            log.Fatal(err)
        }
        fmt.Println("收到消息:", message)
        // 这里可以根据收到的消息内容进行进一步的处理，比如解析JSON数据等
    }
}
```

