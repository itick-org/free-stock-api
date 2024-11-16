

# REST API

Base URLs:

* <a href="https://api.itick.org">正式环境: https://api.itick.org</a>

# Authentication

* API Key (apikey-header-token)
    - Parameter Name: **token**, in: header. 

# Stock

## GET K线查询

GET /stock/kline

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|
|kType|query|string| 是 |周期 1分钟、2五分钟、3十分钟、4三十分钟、5一小时、6两小时、7四小时、8一天、9一周、10一月|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": [
    {
      "tu": 7,
      "c": 1.568,
      "t": 1729090560000,
      "v": 4,
      "h": 1.568,
      "l": 1.568,
      "o": 1.568
    }
  ]
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[[Kline](#schemakline)]|true|none||响应数据|
|»» tu|integer|false|none||成交金额|
|»» c|number|false|none||该K线收盘价|
|»» t|integer|false|none||时间戳|
|»» v|integer|false|none||成交数量|
|»» h|number|false|none||该K线最高价|
|»» l|number|false|none||该K线最低价|
|»» o|number|false|none||该K线开盘价|

## GET 实时报价

GET /stock/tick

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "ld": 67600,
    "t": 1729087440311,
    "v": 0.02989,
    "tu": 2020.564,
    "ts": 0
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[Tick](#schematick)|true|none||响应数据|
|»» s|string|true|none||symbol标的代码|
|»» ld|integer|true|none||最新价|
|»» t|integer|true|none||最新成交的时间戳|
|»» v|number|true|none||成交量|
|»» tu|number|true|none||成交额|
|»» ts|integer|true|none||标的交易状态|

## GET 实时盘口

GET /stock/depth

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "a": [
      {
        "po": 1,
        "p": 67800,
        "v": 0.36339,
        "o": 0.36339
      },
      {
        "po": 2,
        "p": 67800.04,
        "v": 0.00037,
        "o": 0.00037
      },
      {
        "po": 3,
        "p": 67800.28,
        "v": 0.00018,
        "o": 0.00018
      },
      {
        "po": 4,
        "p": 67800.51,
        "v": 0.00019,
        "o": 0.00019
      },
      {
        "po": 5,
        "p": 67800.52,
        "v": 0.0001,
        "o": 0.0001
      }
    ],
    "b": [
      {
        "po": 1,
        "p": 67799.99,
        "v": 2.49817,
        "o": 2.49817
      },
      {
        "po": 2,
        "p": 67799.98,
        "v": 0.06575,
        "o": 0.06575
      },
      {
        "po": 3,
        "p": 67799.96,
        "v": 0.09079,
        "o": 0.09079
      },
      {
        "po": 4,
        "p": 67799.95,
        "v": 0.1001,
        "o": 0.1001
      },
      {
        "po": 5,
        "p": 67799.05,
        "v": 0.0001,
        "o": 0.0001
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[Depth](#schemadepth)|true|none||响应数据|
|»» s|string|true|none||标的代码|
|»» a|[object]|true|none||卖盘|
|»»» po|integer|true|none||档位|
|»»» p|number|true|none||价格|
|»»» v|number|true|none||挂单量|
|»»» o|number|true|none||订单数量|
|»» b|[object]|true|none||买盘|
|»»» po|integer|true|none||档位|
|»»» p|number|true|none||价格|
|»»» v|number|true|none||挂单量|
|»»» o|number|true|none||订单数量|

## GET 标的查询

GET /stock/base?region=hk&code=700.HK

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场|
|code|query|string| 是 |代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "839.HK",
    "nc": "中教控股",
    "ne": "CHINA EDU GROUP",
    "nh": "中教控股",
    "e": "SEHK",
    "c": "HKD",
    "l": 1000,
    "t": 2713791221,
    "cs": 2713791221,
    "hs": 2713791221,
    "ep": 0.5555219630532146,
    "ept": 0.5984943570767414,
    "bps": 6.618762900073513,
    "dy": 0.3387696962454725,
    "b": "HKEquity"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|string|true|none||异常信息,不为0时存在|
|» data|[Base](#schemabase)|true|none||响应数据|
|»» s|string|true|none||产品代码|
|»» nc|string|true|none||中文简体标的名称|
|»» ne|string|true|none||英文标的名称|
|»» nh|string|true|none||中文繁体标的名称|
|»» e|string|true|none||标的所属交易所|
|»» c|string|true|none||交易币种|
|»» l|integer|true|none||每手股数|
|»» t|integer|true|none||总股本|
|»» cs|integer|true|none||流通股本|
|»» hs|integer|true|none||港股股本 (仅港股)|
|»» ep|integer|true|none||每股盈利|
|»» ept|integer|true|none||每股盈利 (TTM)|
|»» bps|integer|true|none||每股净资产|
|»» dy|integer|true|none||股息|
|»» b|string|true|none||标的所属板块|
|»» sd|string|true|none||如果标的是正股，可提供的衍生品行情类型,1 - 期权；2 - 轮证|

# Forex

## GET K线查询

GET /forex/kline

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|
|kType|query|string| 是 |周期 1分钟、2五分钟、3十分钟、4三十分钟、5一小时、6两小时、7四小时、8一天、9一周、10一月|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": [
    {
      "tu": 7,
      "c": 1.568,
      "t": 1729090560000,
      "v": 4,
      "h": 1.568,
      "l": 1.568,
      "o": 1.568
    }
  ]
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|[object]|true|none|响应数据|none|
|»» tu|integer|true|none||none|
|»» c|number|true|none||none|
|»» t|integer|true|none||none|
|»» v|integer|true|none||none|
|»» h|number|true|none||none|
|»» l|number|true|none||none|
|»» o|number|true|none||none|

## GET 实时报价

GET /forex/tick

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "ld": 67600,
    "t": 1729087440311,
    "v": 0.02989,
    "tu": 2020.564,
    "ts": 0
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[Tick](#schematick)|true|none||响应数据|
|»» s|string|true|none||symbol标的代码|
|»» ld|integer|true|none||最新价|
|»» t|integer|true|none||最新成交的时间戳|
|»» v|number|true|none||成交量|
|»» tu|number|true|none||成交额|
|»» ts|integer|true|none||标的交易状态|

## GET 实时盘口

GET /forex/depth

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "a": [
      {
        "po": 1,
        "p": 67800,
        "v": 0.36339,
        "o": 0.36339
      },
      {
        "po": 2,
        "p": 67800.04,
        "v": 0.00037,
        "o": 0.00037
      },
      {
        "po": 3,
        "p": 67800.28,
        "v": 0.00018,
        "o": 0.00018
      },
      {
        "po": 4,
        "p": 67800.51,
        "v": 0.00019,
        "o": 0.00019
      },
      {
        "po": 5,
        "p": 67800.52,
        "v": 0.0001,
        "o": 0.0001
      }
    ],
    "b": [
      {
        "po": 1,
        "p": 67799.99,
        "v": 2.49817,
        "o": 2.49817
      },
      {
        "po": 2,
        "p": 67799.98,
        "v": 0.06575,
        "o": 0.06575
      },
      {
        "po": 3,
        "p": 67799.96,
        "v": 0.09079,
        "o": 0.09079
      },
      {
        "po": 4,
        "p": 67799.95,
        "v": 0.1001,
        "o": 0.1001
      },
      {
        "po": 5,
        "p": 67799.05,
        "v": 0.0001,
        "o": 0.0001
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|object|true|none|响应数据|none|
|»» s|string|true|none|标的代码|none|
|»» a|[object]|true|none|卖盘|none|
|»»» po|integer|true|none|档位|none|
|»»» p|integer|true|none|价格|none|
|»»» v|number|true|none|挂单量|none|
|»»» o|number|true|none|订单数量|none|
|»» b|[object]|true|none|买盘|none|
|»»» po|integer|true|none|档位|none|
|»»» p|number|true|none|价格|none|
|»»» v|number|true|none|挂单量|none|
|»»» o|number|true|none|订单数量|none|

## GET 标的查询

GET /forex/base

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "839.HK",
    "nc": "中教控股",
    "ne": "CHINA EDU GROUP",
    "nh": "中教控股",
    "e": "SEHK",
    "c": "HKD",
    "l": 1000,
    "t": 2713791221,
    "cs": 2713791221,
    "hs": 2713791221,
    "ep": 0.5555219630532146,
    "ept": 0.5984943570767414,
    "bps": 6.618762900073513,
    "dy": 0.3387696962454725,
    "b": "HKEquity"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|object|true|none|响应数据|none|
|»» s|string|true|none|产品代码|none|
|»» nc|string|true|none|中文简体标的名称|none|
|»» ne|string|true|none|英文标的名称|none|
|»» nh|string|true|none|中文繁体标的名称|none|
|»» e|string|true|none|标的所属交易所|none|
|»» c|string|true|none|交易币种|CNY:USD；SGD:HKD|
|»» l|integer|true|none|每手股数|none|
|»» t|integer|true|none|总股本|none|
|»» cs|integer|true|none|流通股本|none|
|»» hs|integer|true|none|港股股本 (仅港股)|none|
|»» ep|number|true|none|每股盈利|none|
|»» ept|number|true|none|每股盈利 (TTM)|none|
|»» bps|number|true|none|每股净资产|none|
|»» dy|number|true|none|股息|none|
|»» b|string|true|none|标的所属板块|none|
|»» sd|string|true|none|如果标的是正股，可提供的衍生品行情类型|1 - 期权；2 - 轮证|

# Indices

## GET 标的查询

GET /indices/base

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "839.HK",
    "nc": "中教控股",
    "ne": "CHINA EDU GROUP",
    "nh": "中教控股",
    "e": "SEHK",
    "c": "HKD",
    "l": 1000,
    "t": 2713791221,
    "cs": 2713791221,
    "hs": 2713791221,
    "ep": 0.5555219630532146,
    "ept": 0.5984943570767414,
    "bps": 6.618762900073513,
    "dy": 0.3387696962454725,
    "b": "HKEquity"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|object|true|none|响应数据|none|
|»» s|string|true|none|产品代码|none|
|»» nc|string|true|none|中文简体标的名称|none|
|»» ne|string|true|none|英文标的名称|none|
|»» nh|string|true|none|中文繁体标的名称|none|
|»» e|string|true|none|标的所属交易所|none|
|»» c|string|true|none|交易币种|CNY:USD；SGD:HKD|
|»» l|integer|true|none|每手股数|none|
|»» t|integer|true|none|总股本|none|
|»» cs|integer|true|none|流通股本|none|
|»» hs|integer|true|none|港股股本 (仅港股)|none|
|»» ep|number|true|none|每股盈利|none|
|»» ept|number|true|none|每股盈利 (TTM)|none|
|»» bps|number|true|none|每股净资产|none|
|»» dy|number|true|none|股息|none|
|»» b|string|true|none|标的所属板块|none|
|»» sd|string|true|none|如果标的是正股，可提供的衍生品行情类型|1 - 期权；2 - 轮证|

## GET K线查询

GET /indices/kline

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|
|kType|query|string| 是 |周期 1分钟、2五分钟、3十分钟、4三十分钟、5一小时、6两小时、7四小时、8一天、9一周、10一月|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": [
    {
      "tu": 7,
      "c": 1.568,
      "t": 1729090560000,
      "v": 4,
      "h": 1.568,
      "l": 1.568,
      "o": 1.568
    }
  ]
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|[object]|true|none|响应数据|none|
|»» tu|integer|true|none||none|
|»» c|number|true|none||none|
|»» t|integer|true|none||none|
|»» v|integer|true|none||none|
|»» h|number|true|none||none|
|»» l|number|true|none||none|
|»» o|number|true|none||none|

## GET 实时报价

GET /indices/tick

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "ld": 67600,
    "t": 1729087440311,
    "v": 0.02989,
    "tu": 2020.564,
    "ts": 0
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[Tick](#schematick)|true|none||响应数据|
|»» s|string|true|none||symbol标的代码|
|»» ld|integer|true|none||最新价|
|»» t|integer|true|none||最新成交的时间戳|
|»» v|number|true|none||成交量|
|»» tu|number|true|none||成交额|
|»» ts|integer|true|none||标的交易状态|

## GET 实时盘口

GET /indices/depth

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 否 |市场代码|
|code|query|string| 否 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "a": [
      {
        "po": 1,
        "p": 67800,
        "v": 0.36339,
        "o": 0.36339
      },
      {
        "po": 2,
        "p": 67800.04,
        "v": 0.00037,
        "o": 0.00037
      },
      {
        "po": 3,
        "p": 67800.28,
        "v": 0.00018,
        "o": 0.00018
      },
      {
        "po": 4,
        "p": 67800.51,
        "v": 0.00019,
        "o": 0.00019
      },
      {
        "po": 5,
        "p": 67800.52,
        "v": 0.0001,
        "o": 0.0001
      }
    ],
    "b": [
      {
        "po": 1,
        "p": 67799.99,
        "v": 2.49817,
        "o": 2.49817
      },
      {
        "po": 2,
        "p": 67799.98,
        "v": 0.06575,
        "o": 0.06575
      },
      {
        "po": 3,
        "p": 67799.96,
        "v": 0.09079,
        "o": 0.09079
      },
      {
        "po": 4,
        "p": 67799.95,
        "v": 0.1001,
        "o": 0.1001
      },
      {
        "po": 5,
        "p": 67799.05,
        "v": 0.0001,
        "o": 0.0001
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none|响应码|0:成功|
|» msg|null|true|none|异常信息|不为0时存在|
|» data|object|true|none|响应数据|none|
|»» s|string|true|none|标的代码|none|
|»» a|[object]|true|none|卖盘|none|
|»»» po|integer|true|none|档位|none|
|»»» p|integer|true|none|价格|none|
|»»» v|number|true|none|挂单量|none|
|»»» o|number|true|none|订单数量|none|
|»» b|[object]|true|none|买盘|none|
|»»» po|integer|true|none|档位|none|
|»»» p|number|true|none|价格|none|
|»»» v|number|true|none|挂单量|none|
|»»» o|number|true|none|订单数量|none|

# Crypto

## GET K线查询

GET /crypto/kline

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|
|kType|query|string| 是 |周期 1分钟、2五分钟、3十分钟、4三十分钟、5一小时、6两小时、7四小时、8一天、9一周、10一月|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": [
    {
      "tu": 7,
      "c": 1.568,
      "t": 1729090560000,
      "v": 4,
      "h": 1.568,
      "l": 1.568,
      "o": 1.568
    }
  ]
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

*返回结果*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码 0:成功|
|» msg|null|true|none||异常信息 不为0时存在|
|» data|[object]|true|none||响应数据|
|»» tu|integer|true|none||成交金额|
|»» c|number|true|none||该K线收盘价|
|»» t|integer|true|none||时间戳|
|»» v|integer|true|none||成交数量|
|»» h|number|true|none||该K线最高价|
|»» l|number|true|none||该K线最低价|
|»» o|number|true|none||该K线开盘价|

## GET 实时报价

GET /crypto/tick

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "ld": 67600,
    "t": 1729087440311,
    "v": 0.02989,
    "tu": 2020.564,
    "ts": 0
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

*返回结果*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码，0:成功|
|» msg|null|true|none||异常信息，不为0时存在|
|» data|object|true|none||响应数据|
|»» s|string|true|none||symbol标的代码|
|»» ld|integer|true|none||最新价|
|»» t|integer|true|none||最新成交的时间戳|
|»» v|number|true|none||成交量|
|»» tu|number|true|none||成交额|
|»» ts|integer|true|none||标的交易状态|

## GET 实时盘口

GET /crypto/depth

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "BA_BTC_USDT",
    "a": [
      {
        "po": 1,
        "p": 67800,
        "v": 0.36339,
        "o": 0.36339
      },
      {
        "po": 2,
        "p": 67800.04,
        "v": 0.00037,
        "o": 0.00037
      },
      {
        "po": 3,
        "p": 67800.28,
        "v": 0.00018,
        "o": 0.00018
      },
      {
        "po": 4,
        "p": 67800.51,
        "v": 0.00019,
        "o": 0.00019
      },
      {
        "po": 5,
        "p": 67800.52,
        "v": 0.0001,
        "o": 0.0001
      }
    ],
    "b": [
      {
        "po": 1,
        "p": 67799.99,
        "v": 2.49817,
        "o": 2.49817
      },
      {
        "po": 2,
        "p": 67799.98,
        "v": 0.06575,
        "o": 0.06575
      },
      {
        "po": 3,
        "p": 67799.96,
        "v": 0.09079,
        "o": 0.09079
      },
      {
        "po": 4,
        "p": 67799.95,
        "v": 0.1001,
        "o": 0.1001
      },
      {
        "po": 5,
        "p": 67799.05,
        "v": 0.0001,
        "o": 0.0001
      }
    ]
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码,0:成功|
|» msg|null|true|none||异常信息,不为0时存在|
|» data|[Depth](#schemadepth)|true|none||响应数据|
|»» s|string|true|none||标的代码|
|»» a|[object]|true|none||卖盘|
|»»» po|integer|true|none||档位|
|»»» p|number|true|none||价格|
|»»» v|number|true|none||挂单量|
|»»» o|number|true|none||订单数量|
|»» b|[object]|true|none||买盘|
|»»» po|integer|true|none||档位|
|»»» p|number|true|none||价格|
|»»» v|number|true|none||挂单量|
|»»» o|number|true|none||订单数量|

## GET 标的查询

GET /crypto/base

### 请求参数

|名称|位置|类型|必选|说明|
|---|---|---|---|---|
|region|query|string| 是 |市场代码|
|code|query|string| 是 |产品代码|

> 返回示例

```json
{
  "code": 0,
  "msg": null,
  "data": {
    "s": "839.HK",
    "nc": "中教控股",
    "ne": "CHINA EDU GROUP",
    "nh": "中教控股",
    "e": "SEHK",
    "c": "HKD",
    "l": 1000,
    "t": 2713791221,
    "cs": 2713791221,
    "hs": 2713791221,
    "ep": 0.5555219630532146,
    "ept": 0.5984943570767414,
    "bps": 6.618762900073513,
    "dy": 0.3387696962454725,
    "b": "HKEquity"
  }
}
```

### 返回结果

|状态码|状态码含义|说明|数据模型|
|---|---|---|---|
|200|[OK](https://tools.ietf.org/html/rfc7231#section-6.3.1)|none|Inline|

### 返回数据结构

状态码 **200**

*返回结果*

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|» code|integer|true|none||响应码，0:成功|
|» msg|null|true|none||异常信息，不为0时存在|
|» data|object|true|none||响应数据|
|»» s|string|true|none||产品代码|
|»» nc|string|true|none||中文简体标的名称|
|»» ne|string|true|none||英文标的名称|
|»» nh|string|true|none||中文繁体标的名称|
|»» e|string|true|none||标的所属交易所|
|»» c|string|true|none||交易币种, CNY:USD；SGD:HKD|
|»» l|integer|true|none||每手股数|
|»» t|integer|true|none||总股本|
|»» cs|integer|true|none||流通股本|
|»» hs|integer|true|none||港股股本 (仅港股)|
|»» ep|number|true|none||每股盈利|
|»» ept|number|true|none||每股盈利 (TTM)|
|»» bps|number|true|none||每股净资产|
|»» dy|number|true|none||股息|
|»» b|string|true|none||标的所属板块|
|»» sd|string|true|none||如果标的是正股，可提供的衍生品行情类型,1 - 期权；2 - 轮证|

# 数据模型

<h2 id="tocS_Kline">Kline</h2>

<a id="schemakline"></a>
<a id="schema_Kline"></a>
<a id="tocSkline"></a>
<a id="tocskline"></a>

```json
{
  "tu": 0,
  "c": 0,
  "t": 0,
  "v": 0,
  "h": 0,
  "l": 0,
  "o": 0
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|tu|integer|false|none||成交金额|
|c|number|false|none||该K线收盘价|
|t|integer|false|none||时间戳|
|v|integer|false|none||成交数量|
|h|number|false|none||该K线最高价|
|l|number|false|none||该K线最低价|
|o|number|false|none||该K线开盘价|

<h2 id="tocS_Base">Base</h2>

<a id="schemabase"></a>
<a id="schema_Base"></a>
<a id="tocSbase"></a>
<a id="tocsbase"></a>

```json
{
  "s": "string",
  "nc": "string",
  "ne": "string",
  "nh": "string",
  "e": "string",
  "c": "string",
  "l": 0,
  "t": 0,
  "cs": 0,
  "hs": 0,
  "ep": 0,
  "ept": 0,
  "bps": 0,
  "dy": 0,
  "b": "string",
  "sd": "string"
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|s|string|true|none||产品代码|
|nc|string|true|none||中文简体标的名称|
|ne|string|true|none||英文标的名称|
|nh|string|true|none||中文繁体标的名称|
|e|string|true|none||标的所属交易所|
|c|string|true|none||交易币种|
|l|integer|true|none||每手股数|
|t|integer|true|none||总股本|
|cs|integer|true|none||流通股本|
|hs|integer|true|none||港股股本 (仅港股)|
|ep|integer|true|none||每股盈利|
|ept|integer|true|none||每股盈利 (TTM)|
|bps|integer|true|none||每股净资产|
|dy|integer|true|none||股息|
|b|string|true|none||标的所属板块|
|sd|string|true|none||如果标的是正股，可提供的衍生品行情类型,1 - 期权；2 - 轮证|

<h2 id="tocS_Tick">Tick</h2>

<a id="schematick"></a>
<a id="schema_Tick"></a>
<a id="tocStick"></a>
<a id="tocstick"></a>

```json
{
  "s": "string",
  "ld": 0,
  "t": 0,
  "v": 0,
  "tu": 0,
  "ts": 0
}

```

Tick

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|s|string|true|none||symbol标的代码|
|ld|integer|true|none||最新价|
|t|integer|true|none||最新成交的时间戳|
|v|number|true|none||成交量|
|tu|number|true|none||成交额|
|ts|integer|true|none||标的交易状态|

<h2 id="tocS_Depth">Depth</h2>

<a id="schemadepth"></a>
<a id="schema_Depth"></a>
<a id="tocSdepth"></a>
<a id="tocsdepth"></a>

```json
{
  "s": "string",
  "a": [
    {
      "po": 0,
      "p": 0,
      "v": 0,
      "o": 0
    }
  ],
  "b": [
    {
      "po": 0,
      "p": 0,
      "v": 0,
      "o": 0
    }
  ]
}

```

### 属性

|名称|类型|必选|约束|中文名|说明|
|---|---|---|---|---|---|
|s|string|true|none||标的代码|
|a|[object]|true|none||卖盘|
|» po|integer|true|none||档位|
|» p|number|true|none||价格|
|» v|number|true|none||挂单量|
|» o|number|true|none||订单数量|
|b|[object]|true|none||买盘|
|» po|integer|true|none||档位|
|» p|number|true|none||价格|
|» v|number|true|none||挂单量|
|» o|number|true|none||订单数量|

