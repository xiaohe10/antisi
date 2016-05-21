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
### 用户登陆：第三方app通过userID和token来登陆
> * /apis/user/login

> * Input Parameters
>> * appUserID:requested
>> * appToken:requested

> * Successful Return
>> * {user:{userID,token,firstTime},status}
>> * firstTime == true : should use app userinfo to configure antisi user info

> * Error Return
>> * errcode = 0: appUSerID格式不正确
>> * errcode = 1: appToken格式不正确
>> * errcode = 2: app验证失败

> * example

```
{"user":{"userID":"1001","token":"48133892c5ade1235b69458ebcec8818",firstTime:"true"},"status":"success"}
```


### 用户登陆：绑定时直接通过用户名和密码登陆
> * /apis/user/login

> * Input Parameters
>> * username:requested
>> * password:requested

> * Successful Return
>> * {user:{userID,token,firstTime},status}
>> * firstTime == true : should use app userinfo to configure antisi user info

> * Error Return
>> * errcode = 0: username 格式不正确
>> * errcode = 1: password 格式不正确
>> * errcode = 2: username 不存在
>> * errcode = 3: password 错误

> * example

```
{"user":{"userID":"1001","token":"48133892c5ade1235b69458ebcec8818",firstTime:"true"},"status":"success"}
```
### 用户注册：获取验证码
> * /apis/user/getvericode

> * Input Parameters
>> * phone:requested

> * Successful Return
>> * {phone,status}

> * Error Return
>> * errcode = 0: phone格式错误或者不存在
>> * errcode = 1: 此手机号已经注册过

> * example

```
{"phone":"18813101200","status":"success"}
```
### 用户注册：注册新用户
> * /apis/user/register

> * Input Parameters
>> * phone:requested
>> * vericode : requested
>> * password: requested

> * Successful Return
>> * {user:{userID,phone,token},status}

> * Error Return
>> * errcode = 0: phone格式错误或者不存在
>> * errcode = 1: 此手机号已经注册过

> * example

```
{"user":{"userID":"1001","token":"48133892c5ade1235b69458ebcec8818","phone":"18813101200"},"status":"success"}
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
> * /apis/user/getinfo

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
>> * {order:{orderID,rentUserName,rentUserIDCard,rentUserPhone,rentDeviceNumber,rentTime,rentAddress,rentFee,rentPayMethod,createdTime},status}

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:deviceID不存在

> * example

```
{"order":{"orderID":12,"rentUserName":"关羽","rentUserPhone":"18813101111","rentDeviceNumber":"1","rentTime":"1年","rentAddress":"北京市农机院","rentFee":"1000元","rentPayMethod":"支微信"},"status":"success"}
```

### 租用设备:修改订单
> * /apis/device/updateorder

> * Input Parameters
>> * token:requested
>> * deviceID:requested
>> * rentUserName:optional
>> * rentUserIDCard
>> * rentUserPhone
>> * rentDeviceNumber
>> * rentTime
>> * rentAddress

> * Successful Return:
>> * {order:{orderID,rentUserName,rentUserIDCard,rentUserPhone,rentDeviceNumber,rentTime,rentAddress,rentFee,rentPayMethod},status}

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:deviceID不存在
>> * errocde = 2:参数格式错误

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

### 获取用户支付历史
> * /apis/device/renthistory

> * Input Parameters
>> * token:requested
>> * begin: optional
>> * end: optional
>> * page: optional
>> * pageSize:optiaonal

> * Successful Return:
>> * {status:"success",rentlist:{orderID,rentUserName,rentUserIDCard,rentUserPhone,deviceID,rentDeviceNumber,rentTime,rentAddress,rentFee,createdTime}}
>> * 这里的rentlist是一个数组，按照创建时间createdTime的倒叙返回

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:begin、end时间格式有问题，或者超出规定时间范围
>> * errcode = 2:页数、页数格式有问题



##设备使用
###返回用户使用日志
> * /apis/device/records

> * Input Parameters
>> * token:requested
>> * begin: optional
>> * end: optional
>> * page: optional
>> * pageSize:optiaonal

> * Successful Return:
>> * {status:"success",records:{recordID,createdTime,linklength,otherdata}}
>> * 这里的records是一个数组，按照创建时间createdTime的倒叙返回

> * Error Return
>> * errcode = 0:token不存在
>> * errcode = 1:begin、end时间格式有问题，或者超出规定时间范围
>> * errcode = 2:页数、页数格式有问题


##后台支付接口
###生成微信支付的预订单，由服务器调用微信统一下单接口，
> * 需要返回：

>> * appId
>> * partnerId
>> * prepayId
>> * nonceStrtimeStamp
>> * packageValue
>> * sign
>> * extData

> * 请求时发送：
>> * orderID，
>> * token
