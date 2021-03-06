# 读取资料点

## 描述

使用**HTTPs GET** 来取得装置回传的资料点


## 请求 URL

读取特定资料通道中的资料点：

```
https://api.mediatek.com/mcs/v2/devices/:deviceId/datachannels/:datachannelId/datapoints?start=:startTime&end=:endTime&limit=:limit&offset=:offset

```


API请求默认值为json格式，如欲使用csv格式，请在API请求URL最后端加上`.csv`。

使用此API，您可以选择您要的资料范围：

* 只读取最后一个资料点：


* 读取一段时间范围内的资料点：


    在请求url尾端加上`?start=:startTime&end=:endTime`


* 限制您要读取的资料点数目(举例来说, 如果您输入:limit=5, 则您会读取前五笔资料点):


    在请求url尾端加上 `?offset=:offset`


* 读取从某一个资料点受开始的资料(举例来说, 如果您输入offset=5, 则您会读取第五笔资料点之后的所有资料点):


    Use the `?offset=:offset` at the end.



您亦可以将以上四种方式混合使用。

使用**HTTPs GET** 来取得装置回传的资料点


## 请求 URL

读取特定资料通道中的资料点：
API请求默认值为json格式，如欲使用csv格式，请在API请求URL最后端加上`.csv`。
使用此API，您可以选择您要的资料范围：

* 只读取最后一个资料点：
* 读取一段时间范围内的资料点：
    在请求url尾端加上`?start=:startTime&end=:endTime`


* 限制您要读取的资料点数目(举例来说, 如果您输入:limit=5, 则您会读取前五笔资料点):


    在请求url尾端加上 `?limit=:limit`


* 读取从某一个资料点受开始的资料(举例来说, 如果您输入offset=5, 则您会读取第五笔资料点之后的所有资料点):


    在请求url尾端加上 `?offset=:offset`


您亦可以将以上四种方式混合使用。

### 查询字串
下面几种查询方式亦可以使用

| 栏位名称 | 格式 | 是否必填 |描述|
| --- | --- | --- | --- |
| start_time | Number | Optional | 查询起始时间 |
| end_time | Number | Optional | 查询中止时间 |
| limit | Number | Optional | 要回传的资料点数目( 默认值= 1 ) |
| offset | Number | Optional | 第几笔开始的资料点 |


| end_time | Number | Optional | 查询中止时间 |
| limit | Number | Optional | 要回传的资料点数目( 默认值= 1 ) |
| offset | Number | Optional | 第几笔开始的资料点 |

**请注意：**

1.
使用回传最后*n (n=数目)* 的资料点时，将不能指定*开始时间* 与*结束时间*时。

2.　当您指定*start_time* 和*end_time*时，若还是有使用设定回传资料点数目，此时，此回传资料点数目的查询将会被忽略。


**每次请求最多只能读取1000笔资料点**


## 动作
HTTPs GET

## 参数

### Header

Device Key
```
deviceKey: `device_key_here`
```

### 回覆格式
您能选择回覆JSON或是CSV格式

JSON:

请求url结尾为 *datapoints*


CSV:

请求url结尾为 *datapoints.csv*



## 回覆

### 回覆代码
200

### 回覆 Header
JSON 格式:
```
Content-Type:`application/json`
```
CSV 格式:
```
Content-Type: `text/csv`
```

### 回覆内容

***资料格式: JSON***

若使用JSON回覆格式，则会包含以下栏位：

| 栏位名称 | 格式 | 描述|
| --- | --- | --- | --- |
| dataChannels | Object Array | 含有资料点的资料通道 |

**资料详情**

**资料通道**

| 栏位名称 | 格式 | 描述|
| --- | --- | --- | --- |
| dataChnId | Number | 资料通道 ID |
| isOverflow | Bool | 要求的资料点是否超过数目限制|
| dataPoints | Object Array | 资料点 |


**资料点**

| 栏位名称 | 格式 | 描述|
| --- | --- | --- | --- |
| createdAt | Number | 以Unix timestamp 格式纪录的资料点建立时间|
| values | Object | 资料点的值 |

请注意，我们在此使用的unix timestamp miniseconds时间值，若须转换成可读格式，您可以使用以下连结：
http://www.epochconverter.com/

**范例：**

请求 URL
```
https://api.mediatek.com/mcs/v2/devices/a1234567890/datachannels/10001/datapoints?start=946684800&end=946784800

```

JSON格式的回覆内容：

```
{
    "deviceId": "DXLQwmnN",
    "dataChannels": [
        {
            "dataChnId": "test01",
            "isOverflow": false,
            "dataPoints": [
                {
                    "recordedAt": 1426457820600,
                    "values": {
                        "value": "HI"
                    }
                }
            ]
        }
    ]
}
```

CSV格式的回覆内容：

```
test_data_channel,94668480,100
```



CSV格式的回覆内容：

## 错误回覆

当错误发生时，回覆代码为非200之其他代码。回覆内容为JSON格式并会包括以下资讯：

CSV格式的回覆内容：

## 错误回覆

当错误发生时，回覆代码为非200之其他代码。回覆内容为JSON格式并会包括以下资讯：


| 栏位名称 | 格式 |描述|
| --- | --- | --- |
| code | Integer | 错误代码 |
| url | String | API错误页面url|
| description | String | 错误描述 |



**范例：**


```
{
    "results": "you dont have permission!"
}
```

