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
	{"user":"test","password":"password"}

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

## 修改密码 ##

调用此API，提交用户名，旧密码和新密码，向服务器发送请求，如果账号已经绑定了邮箱，服务器允许其修改密码，否则密码不能被修改，返回相应的错误提示，成功返回code = 0，失败返回code = 1；

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: modifyPass
	
	BODY:
	{"user":"xsf" ,"oldPass":"xsf123","newPass":"321xsf"}
	

	RESPONSE:
	成功｛"code":0,"msg":"success"｝
	失败｛"code":1,"msg":"failed"｝
注解： oldPass,newPass 为用户明文密码MD5加密后的密码

## 找回密码 ##

调用此API，提交用户名，服务器发送密码到绑定的邮箱里。若该账号没有绑定邮箱，做相应的错误提示；

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: retrievePass
	
	BODY:
	{"user":"xsf" }
	

	RESPONSE:
	成功 {"code":0,"msg":"success"}	
	失败 {"code":1,"msg":"failed"}
	
## 绑定邮箱 ##

调用此API，提交用户名和邮箱，服务器发送一条激活链接到邮箱里，用户点击激活链接，成功绑定邮箱。 如果账号已经绑定了一个邮箱，需要绑定到另一个邮箱也用这个api实现。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: bindMailbox
	
	BODY:
	{"user":"xsf","mail":"cqtek1234@126.com" }
	

	RESPONSE:
	成功｛"code":0,"msg":"已发送一条激活链接到邮箱，请进入邮箱点击激活链接才能成功绑定"｝
	失败｛"code":1,"msg":"failed"｝
	
## 查询账户信息 ##

调用此API，提交用户名，成功，返回用户绑定的邮箱，若未绑定邮箱返回为空。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE:  userInfo
	
	BODY:
	{"user":"xsf"}
	

	RESPONSE:
	成功｛"code":0,"mail":"cqtek1234@126.com"｝
	失败｛"code":1,"msg":"failed"｝
	




## 获取设备列表 ##

### 1. 获取用户名下所有设备列表 ###

调用此API，用户可以知道自己所拥有的所有设备列表及对应权限。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getAllDevice
	
	BODY:
	{"user":"test"}	
	

	RESPONSE:
	[{"snaddr": "35340401911111400", "authority": 0，"dev_name": "设备1"，"area": "仓库"},
	{"snaddr": "35340401911111401", "authority": 0，"dev_name": "设备2"，"area": "仓库"},
	{"snaddr": "35340401911111500", "authority": 1，"dev_name": "设备3"，"area": "仓库"}]

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
	{"snaddr":"设备唯一id","maxTemp":最高温度，"minTemp"：最低温度，"maxHumi"：最大湿度，"minHumi"：最小湿度，"tempHC":温度回差,"humiHC":湿度回差}
	

	RESPONSE:
	成功：{"code":0,"msg":"success"}
	失败：{"code":1,"msg":"fail"}
	{"code":1,"msg":"最低阈值必须小于最高阈值"}
	
注解： 更新接口会将字段都更新，请将不需要修改的设置字段按原有值传过来，严禁传递空值	

## 实时温湿度获取 ##

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

## 设备分组查询 ##

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


## 设备分组修改 ##

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

## 设备信息查询 ##

调用此API，用户可以获得设备名称等信息。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDevInfo
	BODY:
	{"snaddr":"设备唯一id"}
	

	RESPONSE:
	成功｛"code":0, "devName":"设备名称","area": "仓库","devGap":"30"｝
	失败｛"code":1, "msg":"request failed"}

##  修改设备名称 ##

调用此API，用户可以修改设备名称。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setDevName	
	BODY:
	{"snaddr":"设备唯一id"，"devName":"设备名称"}
	

	RESPONSE:
	成功｛"code":0,"msg":"success"｝
	失败｛"code":1,"msg":"change failed"｝
	

##  修改设备分组 ##

调用此API，用户可以修改设备所属分组。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setDevArea	
	BODY:
	{"snaddr":"设备唯一id"，"area":"机房"}
	

	RESPONSE:
	成功｛"code":0,"msg":"success"｝
	失败｛"code":1,"msg":"change failed"｝
	
##  修改设备上传间隔 ##

调用此API，用户可以修改设备上传间隔。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: modifyDeviceGap	
	BODY:
	{"snaddr":"设备唯一id","devGap":"60"}
	

	RESPONSE:
	｛"code": 0，"msg": "change success"｝
	｛"code": 1，"msg": "change failed"｝
	
##  通过mac地址新增设备 ##

调用此API，用户可以通过mac地址增加设备。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: addDevice	
	BODY:
	{"mac":"mac地址","user":"test"}
	

	RESPONSE:
	｛"code" : 0，"msg": "success","snList":["snaddr1","snaddr2"]｝
	｛"code" : 1，"msg": "failed"｝
注解： snaddr为首次添加的用户对该设备有authority管理员权限，后面做关联添加的用户只有使用者权限

##  通过snaddr和ac码新增设备接口 ##

调用此API，用户可以通过snaddr和ac码增加设备。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: addDeviceBySN	
	BODY:
	{"snaddr":"snaddr地址","user":"test","ac":"ac码","devName":"设备名称，选填参数"}
	

	RESPONSE:
	｛"code" : 0，"msg": "success"｝
	｛"code" : 1，"msg": "failed"｝
	｛"code" : 10，"msg": "code！=0则失败，失败原因"｝
	
注解： snaddr为首次添加的用户对该设备有authority管理员权限，后面做关联添加的用户只有使用者权限，返回code!=0则添加失败,code=2表示用户已添加过该设备


##  删除设备 ##

调用此API，用户可以增加设备。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: delDevice	
	BODY:
	{"snaddr":"设备唯一id","user":"test"}
	

	RESPONSE:
	｛"code" : 0，"msg": "success"｝
	｛"code" : 1，"msg": "failed"｝
注解： 只有authority管理员权限用户才能物理删除该设备，使用者权限用户只能删除自己跟该设备的关联关系

##  心跳数据 ##

此API为设备发送给后台的心跳信息。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: beat	
	BODY:
	[{"SN":"设备唯一id","MAC":"mac地址",Device:[{"Addr":"00","GAP":"0060"}]}]	

	RESPONSE:
	ERROR:1;00;GAP:0005;
	ERROR:1;00;dMaxTemp:35315;dMinTemp:27315;dMaxHumi:9900;dMinHumi:0000; dTempHC:010; dHumiHC:100;   

##  修正温度 ##

此API为app发送给后台的用于修正设备温度值
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setTemp
	BODY:
	{"snaddr":"snaddr地址","temp":"温度值"}

	RESPONSE:
	{"code" : 0，"msg": "success"｝
	{"code" : 1，"msg": "failed"｝


##  修正湿度 ##

此API为app发送给后台的用于修正设备湿度值
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setHumi
	BODY:
	{"snaddr":"snaddr地址","humi":"湿度值"}

	RESPONSE:
	{"code" : 0，"msg": "success"｝
	{"code" : 1，"msg": "failed"｝


##  设置实时开关报警 ##

此API为app发送给后台的用于设置设备实时开关报警
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setRealAlarm	
	BODY:
	BODY:	
	{"snaddr":"snaddr地址","alarm":"0或者1"}

	RESPONSE:
	{"code" : 0，"msg": "success"｝
	{"code" : 1，"msg": "failed"｝
注解：0表示关闭实时开关报警，1表示打开实时开关报警

##  获取设备所有关联账号 ##


> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDevAuthority	
	BODY:
	BODY:	
	{"snaddr":"snaddr地址"}

	RESPONSE:
	{"code" : 0，"msg": "success"｝
	{"code" : 1，"msg": "failed"｝
注解：0表示关闭实时开关报警，1表示打开实时开关报警

##  补发历史数据 ##

此API为设备发送给后台的历史数据信息。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: re_put
	BODY:
	[{"SN":"设备唯一id","Time":"16-01-01 19:00",Device:[{"Addr":"00","Temp":28667,"Humi":5020,"IN1":"0000"}]}]	

	RESPONSE:
	ERROR:0;
	
##  故障信息接口 ##

此API为App获取账号对应所有设备的异常列表。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getAccountErr
	BODY:
	{"user":"xsf"}

	RESPONSE:
	[
		{"snaddr":"W2000101",
		"devName":"1号设备"，
		"area":"仓库" ,
		"detail":[
			{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"0"},
			{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"1"},
			{"msg":"湿度过低","type":"4","alarmTime":"报警时间","beginEndMark":"0"},
			
		]},
		{"snaddr":"W2000201",
		"devName":"仓库设备"，
		"area":"仓库" ,
		"detail":[
			{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"0"},
			{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"1"},
			{"msg":"湿度过低","type":"4","alarmTime":"报警时间","beginEndMark":"0"},
		]}
	]

异常type描述 --- 1:温度过高;2:温度过低;3:湿度过高;4:湿度过低;5:开关报警;6:设备离线;7:传感器异常;8:传感器未连接


##  设备故障信息接口 ##

此API为App获取设备的异常列表，以设备为维度的设备历史信息,提交查询设备的其实和结束时间，查询设备在这一段时间内所有的数据。
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDeviceErr
	BODY:
	{"snaddr":"设备唯一id","startTime":"起始时间","endTime":"结束时间"}

	RESPONSE:
	{
	"snaddr":"W2000201",
	"devName":"仓库设备"，
	"area":"仓库" ,
	"detail":[
		{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"0"},
		{"msg":"设备离线","type":"6","alarmTime":"报警时间","beginEndMark":"1"},
		{"msg":"湿度过低","type":"4","alarmTime":"报警时间","beginEndMark":"0"},
	]}

异常type描述 --- 1:温度过高;2:温度过低;3:湿度过高;4:湿度过低;5:开关报警;6:设备离线;7:传感器异常;8:传感器未连接

##  设备历史数据接口 ##

此API为App获取设备的历史数据，输入间隔参数(单位分钟)，开始结束时间(格式为 YYYY-mm-dd HH:MM:SS)，以设备为维度返回历史数据。
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getHisData
	BODY:
	{"snaddr":"设备唯一id","startTime":"起始时间","endTime":"结束时间","rangeTime":"间隔时间"}

	RESPONSE:
	{
		"timeList":["2017-01-01 11:12:11","2017-01-01 11:13:11"],
		"humiList":["12","24"],
		"tempList":["23","24"]
	}

##  添加Android设备token ##

此API用于添加android设备的umeng token，用于推送报警
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setAndroidDevToken
	BODY:
	{"user":"用户名","token":"设备token"}

	RESPONSE:
	{
		"code":0,
		"msg":"设备token"
	}

##  修改设备NodeID ##

此API用于修改设备NodeID
> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: setNodeId
	BODY:
	{"user":"用户名","snaddr":"设备snaddr","nodeId":"待绑定nodeId"}

	RESPONSE:
	{
		"code":0,
		"msg":"nodeId"
	}


## 设备NodeId查询 ##

调用此API，用户可以获得设备NodeId等信息。

> POST

	REQUEST:
	POST
	
	HEADER:
	HTTP_TYPE: getDeviceSet
	BODY:
	{"snaddr":"设备唯一id","user":"用户名"}
	

	RESPONSE:
	成功 {"array": {"snaddr": "W21L7601", "humiHC": "1.00", "maxTemp": "38.00", "minTemp": "-20.00", "devName": "W21L7601", "nodeId": "W21L7101", "tempHC": "0.10", "maxHumi": "95.00", "minHumi": "5.00"}, "code": 0}
	失败｛"code":1, "msg":"request failed"}

