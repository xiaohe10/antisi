# Antisi SDK 文档
## 服务器信息
**接口 :** http://hengaigaoke.com:7070

**开发框架 :** Java1.8 + spring.io + mysql + nginx

配置文档：<https://spring.io/guides/gs/rest-service/>

## 接口说明
Requests use POST method, return JSON (all the key-value in string format).

All the error return json formated as:

```
{text, errcode, status}
if status == "success" means success
if status == "error" means error
```

## 用户相关接口
### 用户登录
> * /apis/user/login
> * Parameters
>> * username:requested
>> * password:requested
> * Successful Return
>> * {user:{userID,token},status}
> * Error Return
>> * errcode = 0: 用户名格式不正确
>> * errcode = 1: 密码格式不正确
>> * errcode = 2: 用户名不存在
>> * errcode = 3: 密码错误
> * example

```
{"user":{"id":"15901511022","token":"1b4a6933f162cfbb6aa24080fc4a8f2f","nickname":"mynickname","","avatar":"images/default_avatar","backImage":"images/default_backImage"},"date":"2015-08-13 18:57:22","status":true}
```





