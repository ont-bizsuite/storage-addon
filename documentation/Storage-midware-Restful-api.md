# Storage-midware-Restful-api

[TOC]

## Storage-midware 接口规范：

### 上传并注册dataId

```text
url：/api/v1/data/dataId/register
method：POST
```

- 请求：

```json
{
	"ontid": "did:ont:xxxx",
	"file": null,
	"domain": "hello.app.ont",
	"signature":""
}
```

| Field_Name | Type   | Description |
|:-----------|:-------|:------------|
| ontid   | String | 文件上传者ONT ID    |
|file|MultipartFile|上传的文件|
|domain|String|app对应的域名|
|signature|String|``domain``对应的ONT ID对``upload``字符串的签名|

- 响应：

```json
{
    "action": "registerDataId",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "qrCode": {
            "dataUrl": "http://192.168.1.129:10660/api/v1/param/b2264f82-cfdd-49b5-b336-9ca9cbf00045",
            "chainNet": "Testnet",
            "ons": "mpdemo.app.ont",
            "expire": 1579506445,
            "callbackUrl": "http://192.168.1.129:8099/api/v2/ontid/invoke",
            "id": "5d46f2c9-bbcb-4f9c-9d11-f5baf3c67989",
            "type": "ontid",
            "version": "v2.0.0"
        },
        "param": "{\"action\":\"signTransaction\",\"params\":{\"gasLimit\":20000,\"contractHash\":\"0300000000000000000000000000000000000000\",\"functions\":[{\"args\":[{\"name\":\"dataId\",\"value\":\"String:did:ont:AR2TbHipPeVzoT42cXAC9rAVv5r7oH9ivV\"},{\"name\":\"ontId\",\"value\":\"String:did:ont:AcFNy7vYUu8acN7c5tXPZZPbVE6r1JTh7\"},{\"name\":\"index\",\"value\":1}],\"operation\":\"regIDWithController\"}],\"payer\":\"AcdBfqe7SG8xn4wfGrtUbbBDxw2x1e8UKm\",\"gasPrice\":500}}",
        "id": "b2264f82-cfdd-49b5-b336-9ca9cbf00045"
    },
    "version": "v1"
}
```

| Field_Name | Type   | Description                   |
|:-----------|:-------|:------------------------------|
| action     | String | 动作标志                      |
| version    | String | 版本号                        |
| error       | int    | 错误码                        |
| desc        | String | 成功为SUCCESS，失败为错误描述 |
| result     | Map | 成功返回二维码相关参数，失败返回""     |
| qrCode     | Map | 二维码相关参数，需要自行填充``signature``作为key     |
| param     | String | 注册DATA ID的交易参数，需要使用``domain``对应的ONT ID对其签名作为``signature``填充至``qrCode``     |
| id     | String | midware为请求生成的唯一标识，可用于查询扫码验签结果     |


### 校验DataToken并凭借DataToken生成jwt

```text
url：/api/v1/data/token
method：POST
```

- 请求：

```json
{
    "tokenId": 10,
    "domain": "hello.app.ont",
    "signature": ""
}
```

| Field_Name | Type   | Description |
|:-----------|:-------|:------------|
| tokenId   | Long | data生成的TOKEN ID    |
|domain|String|app对应的域名|
|signature|String|``domain``对应的ONT ID对``use``字符串的签名|

- 响应：

```json
{
    "action": "consumeToken",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "qrCode": {
            "dataUrl": "http://192.168.1.129:10660/api/v1/param/6aa1344e-5ec0-43fd-bed8-4e2e60c7b36f",
            "chainNet": "Testnet",
            "ons": "mpdemo.app.ont",
            "expire": 1579513258,
            "callbackUrl": "http://192.168.1.129:8099/api/v2/ontid/invoke",
            "id": "9e5a4414-b73a-4360-b04f-c70c15fc2332",
            "type": "ontid",
            "version": "v2.0.0"
        },
        "param": "{\"action\":\"signTransaction\",\"params\":{\"gasLimit\":20000,\"contractHash\":\"62b46b80858a9b0bb7b118abb082b06eae7b65d0\",\"functions\":[{\"args\":[{\"name\":\"tokenId\",\"value\":1}],\"operation\":\"consumeToken\"}],\"payer\":\"AcdBfqe7SG8xn4wfGrtUbbBDxw2x1e8UKm\",\"gasPrice\":500}}",
        "id": "6aa1344e-5ec0-43fd-bed8-4e2e60c7b36f"
    },
    "version": "v1"
}
```

| Field_Name | Type   | Description                   |
|:-----------|:-------|:------------------------------|
| action     | String | 动作标志                      |
| version    | String | 版本号                        |
| error       | int    | 错误码                        |
| desc        | String | 成功为SUCCESS，失败为错误描述 |
| result     | Map | 成功返回二维码相关参数，失败返回""     |
| qrCode     | Map | 二维码相关参数，需要自行填充``signature``作为key     |
| param     | String | 消费token的交易参数，需要使用``domain``对应的ONT ID对其签名作为``signature``填充至``qrCode``     |
| id     | String | midware为请求生成的唯一标识，可用于查询扫码验签结果     |


### 刷新未使用的jwt

```text
url：/api/v1/data/renew
method：POST
```

- 请求：

```json
{
    "accessToken": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJhdWQiOiIiLCJpc3MiOiJkaWQ6b250OkFldE1UdWNxWWpLaHN5d1BRQ0hvVFI0c0Q4NWlmbllETm8iLCJleHAiOjE1Nzc0OTg1OTUsImlhdCI6MTU3NzQxMjE5NSwianRpIjoiMDA3OGE4N2JmYmIxNDQ1MmE3MjJjZGUyYjQ4OWU5OTkiLCJjb250ZW50Ijp7ImRhdGFJZCI6ImRpZDpvbnQ6QWFHTWRrVWFyUUVwWGVvRUNnQUhLV3J3a25lUnhMYkhxZSIsInR5cGUiOiJhY2Nlc3NfdG9rZW4ifX0.MDE3YzdiMGE4NTA4MWMyMmRkMTJmNmFiNjI1Y2JkYTZjZTk4NjljMTQ3NGJlMDJlMTNlODgzYjIzNjZkYTJhODgwN2YzYjAyYjIzYTI1YzVlZjFjZDQyNjU2ZDM3MWQ3OTU0NGVhODM4MDRjMjU1NmNhNmIwYThkMmYzODM0MTU3Mg"
}
```

| Field_Name | Type   | Description |
|:-----------|:-------|:------------|
|accessToken|String|未使用的token(jwt)|

- 响应：

```json
{
	"action": "renewJwt",
	"error": 0,
	"desc": "SUCCESS",
	"result": {
		"access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJhdWQiOiIiLCJpc3MiOiJkaWQ6b250OkFldE1UdWNxWWpLaHN5d1BRQ0hvVFI0c0Q4NWlmbllETm8iLCJleHAiOjE1Nzc0OTk3MDYsImlhdCI6MTU3NzQxMzMwNiwianRpIjoiYzJhZDMyY2FhMTlkNDFhZjlhYWU1NWMxNTkwOWVlYzQiLCJjb250ZW50Ijp7ImRhdGFJZCI6ImRpZDpvbnQ6QUdUcmJ5RHBLQWFiQTVpRnRrdjc5UlJUdHpxZmN2NDU0SiIsInR5cGUiOiJhY2Nlc3NfdG9rZW4ifX0.MDFiOTI2MDRmNzRkNGU3YjlmMWQzY2I2OTg3NDI0MWU0ZWYwNmFhY2E5NDhlYjhhZWZkY2UyNjIxY2QwNjU0MWY2YmY1MTYwNGYxOTUwNDViNDdmMzU0NGYyMTkxZGYxMDgwMDE1OWZjNTk4ZTk1N2E4M2EzMjk1Y2FiMjU1MzE3Yg"
	},
	"version": "v1"
}
```

| Field_Name | Type   | Description                   |
|:-----------|:-------|:------------------------------|
| action     | String | 动作标志                      |
| version    | String | 版本号                        |
| error       | int    | 错误码                        |
| desc        | String | 成功为SUCCESS，失败为错误描述 |
| result     | Map | 成功返回token(jwt)，失败返回""     |
| access_token     | String | 下载用token(jwt)     |


### 查询二维码的扫码结果

```text
url：/api/v1/result/{id}
method：GET
```

- 请求：

| Field_Name | Type   | Description |
|:-----------|:-------|:------------|
|id|String|二维码id|

- 响应：

```json
{
    "action": "purchaseOrderResult",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "result": "2",
        "tokenId": [
            6,
            6
        ],
        "orderId": "1efeb11772a9c4c454f95d8518d42aac0b39eafb"
    },
    "version": "v1"
}
```
```json
{
    "action": "consumeTokenResult",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "result": "2",
        "downloadUrl": "http://192.168.1.129:10665/api/v1/data/access?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJFUzI1NiJ9.eyJhdWQiOiIiLCJpc3MiOiJkaWQ6b250OkFldE1UdWNxWWpLaHN5d1BRQ0hvVFI0c0Q4NWlmbllETm8iLCJleHAiOjE1Nzc0OTg4NzEsImlhdCI6MTU3NzQxMjQ3MSwianRpIjoiOGZmNTcxMmRjYjk1NDU1N2E5NDczZWI5NmU1NjcxYTEiLCJjb250ZW50Ijp7ImRhdGFJZCI6ImRpZDpvbnQ6QWFHTWRrVWFyUUVwWGVvRUNnQUhLV3J3a25lUnhMYkhxZSIsInR5cGUiOiJhY2Nlc3NfdG9rZW4ifX0.MDE1NzA1ZTAyYzY1M2UwY2IyNWYyYjg5Y2U3N2E4NTQ1YWE3MDY5NDBiNzI5ZTk5ZjZhMTUyYjg3YjVhZDdiM2VjZmMzNzQ1N2U2YTBkZjljMzg5ODQ5MTNhOTc5NDNiYWRlMWU3MWQ3NzgxZTRjODIyNTgxOWQ3ZmYwYmU1Yjk4MA"
    },
    "version": "v1"
}
```

```json
{
    "action": "registerDataIdResult",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "result": "2",
        "dataId": "did:ont:AVtMs8xvPLx1se6yV2E2XriMamr5GTrdPk"
    },
    "version": "v1"
}
```

```json
{
    "action": "authOrderResult",
    "error": 0,
    "desc": "SUCCESS",
    "result": {
        "result": "2",
        "authId": "35d3bcafa3e84d34be6ffff8713fde7d5e6f9e30"
    },
    "version": "v1"
}
```

| Field_Name | Type   | Description                   |
|:-----------|:-------|:------------------------------|
| action     | String | 二维码对应的动作标志                      |
| version    | String | 版本号                        |
| error       | int    | 错误码                        |
| desc        | String | 成功为SUCCESS，失败为错误描述 |
| result     | Map | 成功返回，失败返回""     |
| result        | String | null-未扫码；0-交易失败；1-交易发送成功；2-上链成功 |
| orderId        | String | 合约生成的订单号 |
| tokenId        | List | 以Data ID生成的Token ID，第一位表示起始ID，第二位表示结束ID |
| dataId        | String | Data ID |
| authId        | String | 合约生成的授权上架编号 |
| downloadUrl   | String | 数据下载地址 |