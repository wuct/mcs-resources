# 上传资料点

## 描述

使用 **HTTPs POST** 来上传资料点

## 请求 URL

```
https://api.mediatek.com/mcs/v2/devices/:deviceId/datapoints

```

API请求默认值为json格式，如欲使用csv格式，请在API请求URL最后端加上`.csv`。

## 动作
HTTPs POST

## 参数

### Header

**Token**

若是装置：

```
deviceKey: `device_key_here`
```
若是使用者：
```
Authorization: Bearer `{token}`
```

**Content Type**

JSON 格式:
```
Content-Type:`application/json`
```

Comma Separated Value (CSV) 格式:
```
Content-Type:`text/csv`
```



### 内容

#### CSV 格式:

语法:

*:Data_Channel_Id_1, :Timestamp, :Value_1, :Value_2, :Value_3\n*

*:Data_Channel_Id_2, :Timestamp, :Value_1\n*

如欲参考更多详细的资料通道类型之格式，请参考以下连结 [here](api_references#data_channel_format).


请注意：若您不需要上传装置的时间点,则您可保持*:Timestamp*为空(但保留逗号)，此时时间点则会由MCS所收到资料点的时间。


范例：
```
1,946684800,26
2,,26.34,12,59
```
第一行：资料通道ID为1，并且给予时间点，26为上传的值(此时的资料通道类型为整数)。

第二行：资料通道ID为2，并且不给予时间点，26.34为上传的值(此时的资料通道类型为浮点数)。


#### JSON 格式

语法:

每个JSON格式的资料点，都包含下列三种型态的资料：

*dataChnId, timestamp, values*

Values是资料点的值，大部分情况下代表一个值。但也有例外，例如GPS资料就会有三个值。


```
{
   "datapoints":[
      {
         "dataChnId":"1",
         "timestamp":946684800,
         "values":{
            "value":"26"
         }
      },
      {
         "dataChnId":"2",
         "timestamp":946684800,
         "values":{
            "latitude":"26.34",
            "longitude":"12",
            "altitude":"59"
         }
      }
   ]
}

```
请注意，我们在此使用的unix timestamp miniseconds时间值，若须转换成可读格式，您可以使用以下连结：
http://www.epochconverter.com/

## 回覆

### 回覆代码
200

### 回覆 Header

Content-Type:`application/json`
### 回覆内容

**范例:**

请求 URL：
```
https://api.mediatek.com/mcs/v2/devices/d1234567890/datapoints
```

请求内容：

```
1,946684800,26
2,,26.34,12,59
```

回覆内容：

```
{
    "results": "success"
}
```

## 错误回覆

当错误发生时，回覆代码为非200之其他代码。回覆内容为JSON格式并会包括以下资讯：

| 栏位名称 | 格式 |描述|
| --- | --- | --- |
| code | Integer | 错误代码 |
| url | String | API错误页面url |
| description | String | 错误描述 |

**范例:**

```
{
    "results": "None of the data points is valid.",
    "descriptions": [
        "The type of uploaded data point for data channel test01 is not matched to Switch"
    ]
}
```
