# enableCCIpLimit


## 描述
开启 CC 防护每 IP 的限速

## 请求方式
POST

## 请求地址
https://ipanti.jdcloud-api.com/v1/regions/{regionId}/instances/{instanceId}:enableCCIpLimit

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**regionId**|String|True| |Region ID|
|**instanceId**|Long|True| |实例 ID|

## 请求参数
无


## 返回参数
|名称|类型|描述|
|---|---|---|
|**result**|Result| |
|**requestId**|String| |
|**error**|Error| |

### Error
|名称|类型|描述|
|---|---|---|
|**code**|Integer|请求错误状态码|
|**status**|String|请求错误状态码|
|**message**|String|请求错误提示|
### Result
|名称|类型|描述|
|---|---|---|
|**code**|Integer|0: 开启 CC 防护每 IP 的限速失败, 1: 开启 CC 防护每 IP 的限速成功|
|**message**|String|开启 CC 防护每 IP 的限速失败时给出具体原因|

## 返回码
|返回码|描述|
|---|---|
|**200**|OK|
|**404**|NOT_FOUND|
