
[TOC]

#游戏服务端说明

-------
## 全局状态消息描述 


> 0 : 成功
> 1 : 缺失必要字段
> 2 : 用户名已被注册
> 3 : token过期，需要重新登录
> 4 : 用户不存在
> 5 : 更新用户基本信息失败
> 6 : 金币总额超过上限或者下限
> 7 : 发送消息失败  
> 8 : 用户已经在好友列表里
> 9 : 没有发送过好友请求

-------


##服务端接口

### 1.注册 /reg 调用默认token 直接传递一个加密包
 >  参数：{"username":"admin","password":"123","imei":"imei12345","mac_addr":"00fcff..","phone_type":"iphone 6","os_type":"iOS 8.3","reg_mode":"0"}  

 > 返回：{"status" : 0, "message" : "status的描述"}
`. 注册成功 status=0 `
`. 其它status的值参考全局消息描述`

### 2.登录 /login 调用默认token 直接传递一个加密包
 >  参数：  {"username":"admin","password":"123","imei":"imei12345","mac_addr":"00fcff..","phone_type":"iphone 6","os_type":"iOS 8.3","reg_mode":"0"}  
`这里同样需要上传手机相关信息，为以后分析用户行为做准备`

 > 返回： {"status" : 0, "message" : "status的描述", "data" : {"token" : "token的值,用来加密用"}}
` . 登录成功 status=0`
` . token: 用来代替nonce来做加密key`
` . 其它status的值参考全局消息描述 `
 
### 3.刷新token /refreshtoken 
 `登录获取到的token 2小时有效（暂定），用户可以在token有效期内刷新延长时效，也可用作心跳检测`
 >  参数：{"username":"admin","token":"token....","imei":"imei12345","mac_addr":"00fcff..","phone_type":"iphone 6","os_type":"iOS 8.3","reg_mode":"0"}  
`这里同样需要上传手机相关信息，为以后分析用户行为做准备`

 > 返回：{"status" : 0, "message" : "status的描述", "data" : {"token" : "token的值,用来加密用"}}
` . 成功 status=0`
 `. token: 用来代替nonce来做加密key`
` . 其它status的值参考全局消息描述 `
 
### 4.获取用户基本信息 /userinfo  
 >  参数：{"username":"admin","token":"token...."}  
`登录用户获取自己的基本信息`

 > 返回：{"status" : 0, "message" : "status的描述", "data" : {"_id":"...","username":"admin","money":1000,...}}
`  . 成功 status=0`
` . 其它status的值参考全局消息描述 `
 
### 5.更新用户信息 /updateuserinfo 
  >  参数：{"username":"admin","token":"token....","password":"","nickname":"","headimg":""}  
`登录用户只允许更新 password,nickname,headimg这三个字段，或者只更新其中几个，以后再确定是否开放其它字段`

 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
`  .其它status的值参考全局消息描述 `
 
### 6.更新金币 /money 
  >  参数：{"username":"admin","token":"token....","money":"","money_type":"1"}  
`登录用户更新money password,nickname,headimg这三个字段，或者只更新其中几个，以后再确定是否开放其它字段`
`money : 总额不超过999999999，可以为负值`
**<font color="red">money_type : 类型 1：游戏收入 2：购买扣除 3： ..这一块需要进一步确定，会根据类型进行逻辑判断</font>**

 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
` .其它status的值参考全局消息描述 `

### 7.发消息 /sendmesasge
  >  参数：{"username":"admin","token":"token....","friend_username":"","message":""}  
`登录用户发送消息给其它用户` 
`friend_username : 要接收消息的用户的用户名` 
`message : 消息内容，不要超过70个字符`

 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
` .其它status的值参考全局消息描述 `

### 8.获取全部消息 /messages
 >  参数：{"username":"admin","token":"token...."}  
`登录用户获取最新消息` 
`消息类型为 0 系统消息 1 私信 2. 加好友 3. 回复好友请求 带状态`

 > 返回：{"status" : 0, "message" : "status的描述", "data":{}}
` .成功 status=0`
` .其它status的值参考全局消息描述 `
` .data: 返回消息数组`

### 9.添加好友 /addfriend
  >  参数：{"username":"admin","token":"token....","friend_username":"","message":""}  
`登录用户发起添加好友请求` 
`friend_username : 要请求添加好友的用户的用户名` 
`message : 请求好友简介，不要超过70个字符`

 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
` .其它status的值参考全局消息描述 `

### 10.同意好友请求 /agreefriend
 >  参数：{"username":"admin","token":"token....","from_username":""}  
`登录用户同意好友请求` 
`from_username : 发起请求添加好友的用户的用户名` 


 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
` .其它status的值参考全局消息描述 `

### 11.拒绝添加好友 /disagreefriend
 >  参数：{"username":"admin","token":"token....","from_username":""}  
`登录用户拒绝好友请求` 
`from_username : 发起请求添加好友的用户的用户名` 


 > 返回：{"status" : 0, "message" : "status的描述", }
` .成功 status=0`
` .其它status的值参考全局消息描述 `

### 13.好友列表 /friends
 >  参数：{"username":"admin","token":"token...."}  
`登录用户获取最新朋友列表

 > 返回：{"status" : 0, "message" : "status的描述", "data":{}}
` .成功 status=0`
` .其它status的值参考全局消息描述 `
` .data: 返回消息数组`

