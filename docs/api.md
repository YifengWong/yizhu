
# 一、用户类

## 注册前发送短信验证码
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|400|FAILED|手机号已存在|
|403|FORBIDDEN|手机号（用户名）无效|

URI:

```
GET /user/sendSms/{userId}
```

GET参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|userId|用户名（手机号）|string|


成功例子：

```json
{
	"userId":"11111111111"
}
```

## 注册
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|400|FAILED|用户名已存在|
|403|FORBIDDEN|用户名不是手机号或参数格式错误|
|450|MISS|验证码错误|

URI:

```
POST /user/register
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|userId|用户名（手机号）|string|
|password|密码（未处理）|string|
|code|短信验证码|string|
|name|姓名|string|
|gender|性别|string - "male"/"female"|
|birthDate|出生日期|string - YYYY-MM-DD|
|location|常住地|string|


成功例子：

```json
{
	"userId":"11111111111"
}
```

## 登录
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|403|FORBIDDEN|用户名不是手机号或参数格式错误|
|400|NOT FOUND|用户名或密码错误|

URI:

```
POST /user/login
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|userId|用户名（手机号）|string|
|password|密码（未处理）|String|


成功例子：
```json
{
	"userId":"11111111111"
}
```

## 修改个人信息
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|403|FORBIDDEN|性别或日期参数格式不正确|

URI:

```
POST /user/modifyInfo
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|name|姓名|string|
|gender|性别|string - "male"/"female"|
|birthDate|出生日期|string - YYYY-MM-DD|
|location|常住地|string|


成功例子：

```json
{
	"userId":"11111111111"
}
```

## 获取个人信息
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|

URI:

```
POST /user/info
```

POST参数：无


成功例子：

```json
{
	"userId":"11111111111",
	"name":"张三",
	"gender":"male",
	"birthDate":"1995-12-02",
	"location":"中山大学"	
}
```

## （推送）更新用户objectId（leancloud SDK获取）
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|404|MISS|objectId不存在|

URI:

```
POST /user/updateObjectId
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|objectId|安装Id|string|


成功例子：

```json
{
	"userId":"11111111111"
}
```

## （推送）更新用户定位位置
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|403|FORBIDDEN|纬度不在-90~90或经度不在-180~180间|
|450|MISS|用户未记录安装Id|
|500|Error|服务器错误|

URI:

```
POST /user/updateLocation
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|latitude|纬度|double|
|longitude|经度|double|

成功例子：

```json
{
	"userId":"11111111111"
}
```

# 二、提问类

## 提问
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|

URI:

```
POST /question/ask
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|title|标题|string|
|content|提问内容|string|

成功例子：
```json
{
	"questionId":1
}
```

## 回答
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|450|MISS|该提问id不存在|

URI:

```
POST /question/answer
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|questionId|问题id|int|
|content|回答内容|string|


成功例子：
```json
{
	"answerId":1
}
```

## 赞同
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|450|MISS|该回答id不存在|

URI:

```
POST /question/agreeAnswer
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|answerId|回答id|int|
|agreeOrNot|是否赞同|bool - true赞同/false不赞同|

成功例子：
```json
{
	"answerAgreeId":1
}
```

## 获取所有问题Id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功

URI:

```
GET /question/getAllId
```


成功例子：
```json
{
	"count":2,
	"data":[1,2]
}
```

## 根据问题Id获取问题摘要
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND| questionId不存在|

URI:

```
GET /question/digest/{questionId}
```


成功例子：
```json
{
	"questionId":1,
	"userId":"133133123456",
	"userName":"张三",
	"title":"扶老奶奶过马路是一种怎样的体验？",
	"createDate":"2017-05-17"
}
```

## 根据问题Id获取完整问题内容
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND| questionId不存在|

URI:

```
GET /question/detail/{questionId}
```


成功例子：
```json
{
	"questionId":1,
	"userId":"133133123456",
	"userName":"张三",
	"title":"扶老奶奶过马路是一种怎样的体验？",
	"content":"bla bla blabla",
	"createDate":"2017-05-17"
}
```

## 根据问题Id获取回答所有Id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND| questionId不存在|

URI:

```
GET /question/getAnswerIds/{questionId}
```


成功例子：
```json
{
	"count":2,
	"data":[1,2]
}
```

## 根据回答Id获取回答内容
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND|answerId不存在|

URI:

```
GET /question/getAnswer/{answerId}
```


成功例子：
```json
{
	"answerId":1,
	"userId":"133133123456",
	"userName":"张三",
	"content":"无可奉告",
	"createDate":"2017-05-17",
	"good":5,
	"bad":30
}
```


# 三、一键求救（推送、导航、评价）



## 发起求救
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|403|FORBIDDEN|纬度不在-90~90或经度不在-180~180间|
|450|MISS|用户未记录安装Id|
|500|Error|服务器错误|

URI:

```
POST /sos/push
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|latitude|纬度|double|
|longitude|经度|double|

成功例子：

```json
{
	"sosId":1
}
```

## 响应求救
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|403|FORBIDDEN|已响应，不能重复响应|
|404|NOT FOUND|SOS id 不存在或已完成|
|450|MISS|用户未记录安装Id|

URI:

```
POST /sos/response
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|sosId|求救Id|string|

成功例子：

```json
{
	"userId":"11111111111"
}
```

## 查询所有有效求救id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|

URI:

```
GET /sos/allValidId
```


成功例子：
```json
{
	"count":2,
	"data":[1,2]
}
```

## 根据sos id获取sos内容
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND|SOS id 不存在|

URI:

```
GET /sos/get/{sosId}
```

成功例子：
```json
{
	"sosId": 2,
	"latitude": 0.0,
    "longitude": 5.0,
	"createTime":"2017-06-27 10:15",
	"finished":false,
	"pushUserId": "11111111111"
}
```

## 根据sos id获取所有响应者id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND|SOS id 不存在或已完成|

URI:

```
GET /sos/response/{sosId}
```


成功例子：
```json
{
	"count":2,
	"data":["1234568911", "12345678922"]
}
```

## 结束求救
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|404|NOT FOUND|SOS id 不存在或已完成|
|450|MISS|非发起用户无法结束该求救|

URI:

```
POST /sos/finish
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|sosId|sos id|integer|

成功例子：

```json
{
	"sosId":2
}
```


# 四、求助 （图片、文字、评价）
## 发起求助
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|402|WRONG|需求人数无效，应在1-10人之间|
|403|FORBIDDEN|纬度不在-90~90或经度不在-180~180间|
|450|MISS|用户未记录安装Id|
|500|Error|服务器错误|

URI:

```
POST /help/push
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|latitude|纬度|double|
|longitude|经度|double|
|title|事件标题|string|
|detail|事件信息|string|
|needs|需要多少帮助者|integer|

成功例子：

```json
{
	"helpId":1
}
```

## 响应求助
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|402|WRONG|该求助人数已满|
|403|FORBIDDEN|已响应，不能重复响应|
|404|NOT FOUND|Help id 不存在或已完成|
|450|MISS|用户未记录安装Id|

URI:

```
POST /help/response
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|helpId|求助Id|string|

成功例子：

```json
{
	"userId":"11111111111"
}
```

## 查询所有有效求助id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|

URI:

```
GET /help/allValidId
```


成功例子：
```json
{
	"count":2,
	"data":[1,2]
}
```

## 根据help id获取求助内容
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND|Help id 不存在|

URI:

```
GET /help/get/{helpId}
```

成功例子：
```json
{
	"helpId": 2,
	"latitude": 0.0,
    "longitude": 5.0,
	"finished":false,
	"title":"帮忙抬米",
	"detail":"抬三袋米上五楼",
	"needs":3,
	"responseNum":2,
	"pushUserId": "11111111111"
}
```

## 根据help id获取所有响应者id
| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|404|NOT FOUND|Help id 不存在或已完成|

URI:

```
GET /help/response/{helpId}
```


成功例子：
```json
{
	"count":2,
	"data":["1234568911", "12345678922"]
}
```


## 结束求助
该接口需要登录

| Code | Content | Description |
|------|---------|-------------|
|200|OK|请求成功|
|401|FAILED|未登录|
|404|NOT FOUND|Help id 不存在或已完成|
|450|MISS|非发起用户无法结束该求救|

URI:

```
POST /help/finish
```

POST参数

| 字段 | 描述 | 类型 |
|----------|-------------|------|
|helpId|help id|integer|

成功例子：

```json
{
	"helpId":2
}
```