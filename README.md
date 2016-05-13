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

## 用户接口
### 用户绑定：登录
> * /apis/user/login

> * Input Parameters
>> * appUserID:requested
>> * appToken:requested

> * Successful Return
>> * {user:{userID,token,firstTime},status}

> * Error Return
>> * errcode = 0: appUSerID格式不正确
>> * errcode = 1: appToken格式不正确
>> * errcode = 2: app验证失败

> * example

```
{"user":{"userID":"1001","token":"48133892c5ade1235b69458ebcec8818",firstTime:"true"},"status":"success"}
```
### 设置用户信息
> * /apis/user/setinfo

> * Input Parameters
>> * token:requested
>> * name:optional
>> * phone:optional
>> * wechat:optional
>> * IDCard:optional
>> * address:optional

> * Successfule return
>> * {user:{name,phone,wechat,IDCard,address},status}

> * Error return
>> * errcode = 0:token不存在
>> * errcode = 0:参数格式错误

> * example:

```
{"user":{"name":"张三","phone":"18813101111","wechat":"w111","IDCard":"110112341234234","address":"北京市农机院"},status:"success"}
```

### 获取用户信息
> * /apis/user/setinfo

> * Input Parameters
>> * token:requested

> * Successfule return
>> * {user:{name,phone,wechat,IDCard,address},status}

> * Error return
>> * errcode = 0:token不存在

> * example:

```
{"user":{"name":"张三","phone":"18813101111","wechat":"w111","IDCard":"110112341234234","address":"北京市农机院"},status:"success"}
```


### 用户信息查询:用户信息主页
> * /apis/user/profile

> * Input Parameters
>> * token:requested

> * Successfule return
>> * {user:{balance,point},orderlist:{orderID},usinglog:{logID},status}

> * Error return
>> * errcode = 0:token不存在

> * example

```
{"user":{"balance":"13元","point":"122分"},orderlist:{"1",
"2","3"},usinglog:{"1","2"},status:"success"}
``` 



## 设备租用
### 展示设备
> * /apis/device/show

> * Input Parameters
>> * deviceID:requested

> * Successful Return:
>> * {device:{deviceID,deviceName,deviceDescription,deviceImage},status}

> * Error Return
>> * errcode = 0:appToken不存在

> * example

```
{"device":{"deviceID":12,"deviceName":"device1","deviceDescription":"这个设备可以让你放松","deviceImage":"/deviceImage/image1.jpg"},"status":"success"}
```

### 租用设备:生成订单
> * /apis/device/rent

> * Input Parameters
>> * token:requested
>> * deviceID:requested

> * Successful Return:
>> * {order:{orderID,rentUserName,rentUserIDCard,rentUserPhone,rentDeviceNumber,rentTime,rentAddress,rentFee,rentPayMethod},status}

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:deviceID不存在

> * example

```
{"order":{"orderID":12,"rentUserName":"关羽","rentUserPhone":"18813101111","rentDeviceNumber":"1","rentTime":"1年","rentAddress":"北京市农机院","rentFee":"1000元","rentPayMethod":"支微信"},"status":"success"}
```

### 跳转到支付：去支付
> * /apis/device/rent

> * Input Parameters
>> * token:requested
>> * orderID:requested

> * Successful Return:
>> * {status:"success"}

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:orderID不存在
>> * errcode = 2:支付失败
>> * errcode = 3:订单超时









