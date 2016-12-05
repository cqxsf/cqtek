# 企业API文档说明 #

本文档主要用于描述企业API的使用，供第三方平台开发调用设备数据使用。本文档会定期更新，请及时查看，不做另行通知。

使用HTTP POST/GET方法和服务器进行交互，数据内容使用`JSON`（UTF-8）进行编码，下面是一个**示例**:

> URL: http://42.121.53.218:2500

### 登陆 ###

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: login

	BODY:
	{"member_user":"test","member_password":"password"}

	RESPONSE:
	成功：{"code":0,"msg":"success"}
	失败：{"code":1,"msg":"login fail"}

注解： password为用户明文密码MD5加密后的密码

HTTP Header中必须包含`Api-Key`和`Api-Domain`,其中`Api-Key`为企业超级管理员登录平台管理系统中创建的Key， `Api-Domain`为企业申请帐号时取的企业域名称。

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> at </td>
<td> Number </td>
<td> 服务器UTC时间 </td>
</tr>
<tr>
<td> code </td>
<td> Number </td>
<td> 错误号 </td>
</tr>
<tr>
<td> result </td>
<td> Object </td>
<td> 查询内容结果 </td>
</tr>
</tbody>
</table>

## 名词解释  ##

**设备序列号**(`device_id`): 每一个设备拥有全局唯一的序列号，一般贴在设备上，不可修改。
 
**设备名称**(`name`)： 用户在设备添加时，对设备取的别名，此名称可在企业管理端进行修改。
 
**设备型号**(`model`): 有时候也会称作为`产品型号`，每一个设备拥有一个产品的型号，也就是说同一类的设备具有相同的型号，比如`GPRS智能温度保温箱`的型号为`DX0B801`。


## 获取设备列表 ##

### 1. 获取用户名下所有设备列表 ###

调用此API，用户可以知道自己所拥有的所有设备列表及对应权限。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getAllDevice
	
	BODY:
	{"member_user":"test"}	
	

	RESPONSE:
	[{"device_key": "35340401911111400", "authority": 0},
	{"device_key": "35340401911111401", "authority": 0},
	{"device_key": "35340401911111500", "authority": 1}]

## 修改温湿度阈值 ##

调用此API，用户可以修改设备温湿度阈值和设备名称。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: modifyTH
	
	BODY:
	{"snaddr":"设备唯一id","maxTemp":最高温度，"minTemp"：最低温度，"maxHumi"：最大湿度，"minHumi"：最小湿度，"tempHC":温度回差,"humiH,C":湿度回差,"dev_name":设备名称}
	

	RESPONSE:
	成功：{"code":0,"msg":"success"}
	失败：{"code":1,"msg":"fail"}
注解： 更新接口会将字段都更新，请将不需要修改的设置字段按原有值传过来，严禁传递空值	

## 实时温湿度获取##

调用此API，用户可以获得设备实时温湿度值。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: get
	
	BODY:
	{"snaddr":"设备唯一id","curve":"allLast"}
	

	RESPONSE:
	{"humi": "52.59", "in1": "0000", "temp": "25.15", "time": "16-10-10 10:18"}
	
注解： humi,in1,temp,time为设备最后一次上传数据

## 设备分组查询##

调用此API，用户可以获得所有分组。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getAreaInfo	
	BODY:
	{"user":"xsf"}
	

	RESPONSE:
	["机房","仓库","车间"]
	
注解： 为数组形式


## 设备分组修改##

调用此API，用户可以修改设备分组。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setAreaInfo	
	BODY:
	{"user":"xsf","oldArea":"机房"，"newArea":"新机房"}
	

	RESPONSE:
	{"code":1,"message":"原因"}	
注解： 操作成功返回code= 0，操作失败返回 code= 1,message = 操作失败的原因

## 设备名称查询##

调用此API，用户可以获得所有分组。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setAreaInfo	
	BODY:
	{"user":"xsf","oldArea":"机房"，"newArea":"新机房"}
	

	RESPONSE:
	{"code":1,"message":"原因"}	
注解： 操作成功返回code= 0，操作失败返回 code= 1,message = 操作失败的原因


