# Java 请求数据示例





## 请求K线

```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.itick.org/stock/kline?region=hk&code=700.HK&kType=1"))
    .header("accept", "application/json")
    .header("token", "you_apikey")
    .method("GET", HttpRequest.BodyPublishers.noBody())
    .build();
HttpResponse<String> response = HttpClient.newHttpClient().send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
```



## 请求实时报价

```java
HttpRequest request = HttpRequest.newBuilder()
    .uri(URI.create("https://api.itick.org/stock/tick?region=HK&code=700.HK"))
    .header("accept", "application/json")
    .header("token", "you_apikey")
    .method("GET", HttpRequest.BodyPublishers.noBody())
    .build();
HttpResponse<String> response = HttpClient.newHttpClient().send(request, HttpResponse.BodyHandlers.ofString());
System.out.println(response.body());
```



## 订阅实时报价

```xml
<dependency>
    <groupId>javax.websocket</groupId>
    <artifactId>javax.websocket-api</artifactId>
    <version>1.1</version>
</dependency>
```



```java
import javax.websocket.ClientEndpoint;
import javax.websocket.CloseReason;
import javax.websocket.ContainerProvider;
import javax.websocket.OnClose;
import javax.websocket.OnError;
import javax.websocket.OnMessage;
import javax.websocket.OnOpen;
import javax.websocket.Session;
import javax.websocket.WebSocketContainer;
import java.io.IOException;
import java.net.URI;
import java.net.URISyntaxException;

// 定义WebSocket客户端端点
@ClientEndpoint
public class WebSocketSubscriber {

    // WebSocket服务器的地址
    private static final String WEBSOCKET_SERVER_URL = "wss://api.itick.org/sws";
    
		// 用于鉴权
    private static final String AUTH_MESSAGE = "{\n" +
                    "  \"ac\":\"auth\",\n" +
                    "  \"params\":\"you_apikey\"\n" +
                    "}";

    // 用于订阅的消息格式，这里假设订阅一个名为 "your_channel" 的频道
    private static final String SUBSCRIBE_MESSAGE = "{\n" +
                    "  \"ac\":\"subscribe\",\n" +
                    "  \"params\":\"AM.LPL,AM.LPL\",\n" +
                    "  \"types\":\"depth,quote\"\n" +
                    "}";

    public static void main(String[] args) {
        try {
            // 创建WebSocket容器
            WebSocketContainer container = ContainerProvider.getContainer();

            // 连接到WebSocket服务器并获取会话
            Session session = container.connectToServer(WebSocketSubscriber.class, new URI(WEBSOCKET_SERVER_URL));
            
            // 发送鉴权消息
            session.getBasicRemote().sendText(AUTH_MESSAGE);

            // 发送订阅消息
            session.getBasicRemote().sendText(SUBSCRIBE_MESSAGE);

        } catch (URISyntaxException | IOException e) {
            e.printStackTrace();
        }
    }

    @OnOpen
    public void onOpen(Session session) {
        System.out.println("WebSocket连接已打开");
    }

    @OnMessage
    public void onMessage(Session session, String message) {
        System.out.println("收到消息: " + message);
        // 这里可以根据收到的消息内容进行进一步的处理，比如解析JSON数据等
    }

    @OnError
    public void onError(Session session, Throwable error) {
        System.out.println("WebSocket错误: " + error.getMessage());
    }

    @OnClose
    public void onClose(Session session, CloseReason closeReason) {
        System.out.println("WebSocket连接已关闭，原因: " + closeReason.getReasonPhrase());
    }
}
```

