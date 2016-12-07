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
	[{"device_key": "35340401911111400", "authority": 0，"dev_name": "设备1"，"area": "仓库"},
	{"device_key": "35340401911111401", "authority": 0，"dev_name": "设备2"，"area": "仓库"},
	{"device_key": "35340401911111500", "authority": 1，"dev_name": "设备3"，"area": "仓库"}]

## 查询温湿度阈值 ##

调用此API，用户可以查询设备温湿度阈值。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getThreshold
	
	BODY:
	{"snaddr":"设备唯一id" }
	

	RESPONSE:
	{"snaddr":"设备唯一id","maxTemp":"最高温度"，"minTemp"："最低温度"，"maxHumi"："最大湿度"，"minHumi"："最小湿度"，"tempHC":"温度回差","humiHC":"湿度回差"}
	

## 修改温湿度阈值 ##

调用此API，用户可以修改设备温湿度阈值和设备名称。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: modifyTH
	
	BODY:
	{"snaddr":"设备唯一id","maxTemp":最高温度，"minTemp"：最低温度，"maxHumi"：最大湿度，"minHumi"：最小湿度，"tempHC":温度回差,"humiH,C":湿度回差}
	

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
	HTTP_TYPE: getRTData
	
	BODY:
	{"snaddr":"设备唯一id","curve":"allLast"}
	

	RESPONSE:
	{"humi": { "value"："52.59","status":"-1"}, "in1": "0000", "temp": { "value"："25.59","status":"1"}, "time": "16-10-10 10:18", "abnormal": "0"}
	
注解： humi,in1,temp,time为设备最后一次上传数据,数据是否超过了报警阈值，增加一个标志位abnormal，设备温湿度是否超出阈值，增加一个标志位status，-1为低于阈值下限，0为正常处于阈值内，1超出最高阈值，温度和湿度各有一个阈值。
abnormal具体含义定义:
0 -- 无异常;
1 -- 备离线（优先级最高）
2 -- 传感器异常（优先级第二高，一旦传感器异常，无视下列所有异常，损坏）
 --3 传感器未连接

1，离线判断：服务器150秒没有收到心跳包（30秒发送一次）；
2，异常判断：检测到设备实际数据 温度 -39.6 湿度1%判断为异常；
3，检测到设备实际数据 温度 12.34 湿度56.78%判断为传感器未连接。

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

调用此API，用户可以获得设备名称。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDevName	
	BODY:
	{"snaddr":"设备唯一id"}
	

	RESPONSE:
	成功｛"code":"0","dev_name":"设备名称"｝
	失败｛"code":"1","msg":"request failed"}

##  修改设备名称##

调用此API，用户可以修改设备名称。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setDevName	
	BODY:
	{"snaddr":"设备唯一id"，"dev_name":"设备名称"}
	

	RESPONSE:
	成功｛"code":"0","msg":"success"｝
	失败｛"code":"1","msg":"change failed"｝
	

##  修改设备分组##

调用此API，用户可以修改设备所属分组。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setDevArea	
	BODY:
	{"snaddr":"设备唯一id"，"area":"机房"}
	

	RESPONSE:
	成功｛"code":"0","msg":"success"｝
	失败｛"code":"1","msg":"change failed"｝

##  查询设备上传间隔##

调用此API，用户可以查询设备上传间隔。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDeviceGap	
	BODY:
	{"snaddr":"设备唯一id" }
	

	RESPONSE:
	{"snaddr":"设备唯一id","deviceGap":"30"}
	
##  修改设备上传间隔##

调用此API，用户可以修改设备上传间隔。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: modifyDeviceGap	
	BODY:
	{"snaddr":"设备唯一id","deviceGap":"60"}
	

	RESPONSE:
	｛"code" = "0"，"msg" = "change success"｝
	｛"code" = "1"，"msg" = "change failed"｝
	
