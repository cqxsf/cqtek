# 企业API文档说明 #

本文档主要用于描述企业API的使用，供第三方平台开发调用设备数据使用。本文档会定期更新，请及时查看，不做另行通知。

此API使用企业管理员平台创建的KEY进行授权，使用HTTP POST/GET方法和服务器进行交互，数据内容使用`JSON`（UTF-8）进行编码，下面是一个**示例**:

> URL: http://api.duoxieyun.com/v1

### 获取服务器时间 ###

> GET /company/system/time 

	REQUEST:
	GET /company/system/time
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "time": 1444292636
        }
    }

注解： 调用API时需要加上`URL`前缀，数据返回的格式统一使用如下JSON结构体:

    {
        "at": 1444292636, // UTC时间戳，秒数
        "code": 200, // 错误号，具体请详见错误号定义部分
        "result": { // 调用不同的API服务时 result 结构体内的字段会不一样
            ... 
        }
    }

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

### 1. 获取企业名下所有设备列表 ###

调用此API，用户可以知道自己所拥有的所有设备列表。

> GET /company/devices

	REQUEST:
	GET /company/devices
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "devices": [
				{
					"device_id": "V0013701",
					"name": "冷冻1号",
					"model": "DX90601"
				},
				{
					"device_id": "V0013704",
					"name": "冷冻2号",
					"model": "DX90601"
				},
				{
					"device_id": "T0000006",
					"name": "TJJ35",
					"model": "DX0B801"
				},
				...
			]
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> devices </td>
<td> Array </td>
<td> 设备列表 </td>
</tr>
<tr>
<td> device_id </td>
<td> String </td>
<td> 设备序列号 </td>
</tr>
<tr>
<td> name </td>
<td> String </td>
<td> 设备名称 </td>
</tr>
<tr>
<td> model </td>
<td> String </td>
<td> 设备型号 </td>
</tr>
</tbody>
</table>

### 2. 获取企业名下所有的设备型号 ###

调用此API，用户可以知道自己正在使用哪几款产品型号。

> GET /company/products

	REQUEST:
	GET /company/products
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "products": [
				{
					"name": "冷库温湿度监控仪",
					"model": "DX90601"
				},
				{
					"name": "GPRS智能温度保温箱",
					"model": "DX0B801"
				}
				...
			]
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> products </td>
<td> Array </td>
<td> 产品型号列表 </td>
</tr>
<tr>
<td> device_id </td>
<td> String </td>
<td> 设备序列号 </td>
</tr>
<tr>
<td> name </td>
<td> String </td>
<td> 设备名称 </td>
</tr>
<tr>
<td> model </td>
<td> String </td>
<td> 设备型号 </td>
</tr>
</tbody>
</table>

### 3. 获取企业名下指定设备型号的设备列表 ###

调用此API，用户可以指定具体的产品型号，知道自己拥有这类产品的设备列表。

> GET /company/devices/:model

`model`为设备型号，比如`DX90601`、`DX0B801`...

	REQUEST:
	GET /company/devices/DX90601
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "devices": [
				{
					"device_id": "V0013701",
					"name": "冷冻1号",
					"model": "DX90601"
				},
				{
					"device_id": "V0013704",
					"name": "冷冻2号",
					"model": "DX90601"
				}
				...
			]
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> devices </td>
<td> Array </td>
<td> 设备列表 </td>
</tr>
<tr>
<td> device_id </td>
<td> String </td>
<td> 设备序列号 </td>
</tr>
<tr>
<td> name </td>
<td> String </td>
<td> 设备名称 </td>
</tr>
<tr>
<td> model </td>
<td> String </td>
<td> 设备型号 </td>
</tr>
</tbody>
</table>

## 通过设备的型号获取该设备所属的型号信息 ##

调用此API，用户可以指定设备型号，知道该类设备型号有哪些设备信息或属性。

> GET /company/models/info/:model

`model`为设备型号，比如`DX90601`、`DX0B801`...

	REQUEST:
	GET /company/models/info/DX0B801
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "model_info": {
			    "name": "GPRS智能温度保温箱",
			    "model": "DX0B801",
			    "type": "保温箱",
			    "remark":"GPRS智能温度保温箱",
			    "params":[
			        {
			            "alias": "温度",
			            "variable_name": "t",
			            "type": "STRING",
						"unit": "°C",
			            "required": true,
			            "chart": true
			        },
			        {
			            "alias": "开箱状态",
			            "variable_name": "lock",
			            "type": "STRING",
						"unit": "",
			            "required": false,
			            "chart": false
			        },
			        {
			            "alias": "基站定位",
			            "variable_name": "loc",
			            "type": "STRING",
						"unit": "",
			            "required": false,
			            "chart": false
			        },
			        {
			            "alias": "电量",
			            "variable_name": "v",
			            "type": "STRING",
						"unit": "mV",
			            "required": false,
			            "chart": false
			        }
			    ]
			}
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> model_info </td>
<td> Object </td>
<td> 设备型号信息 </td>
</tr>
<tr>
<td> name </td>
<td> String </td>
<td> 产品名称 </td>
</tr>
<tr>
<td> model </td>
<td> String </td>
<td> 设备型号 </td>
</tr>
<tr>
<td> type </td>
<td> String </td>
<td> 产品类别，属于什么类产品, 比如 保温箱、记录仪、冷藏库、冷藏柜、冷链车... </td>
</tr>
<tr>
<td> remark </td>
<td> String </td>
<td> 产品说明 </td>
</tr>
<tr>
<td> params </td>
<td> Array </td>
<td> 设备参数列表 </td>
</tr>
<tr>
<td> alias </td>
<td> String </td>
<td> 参数名称 </td>
</tr>
<tr>
<td> variable_name </td>
<td> String </td>
<td> 参数变量名 </td>
</tr>
<tr>
<td> type </td>
<td> String </td>
<td> 参数类型，有STRING（字符串型）、INTEGER（整型）、FLOAT（浮点型） 三种数据类型 </td>
</tr>
<tr>
<td> unit </td>
<td> String </td>
<td> 参数单位 </td>
</tr>
<tr>
<td> required </td>
<td> Boolean </td>
<td> 是否必选，如果true，则表示在一个**数据点**中此参数必须出现，否则可以不出现 </td>
</tr>
<tr>
<td> chart </td>
<td> Boolean </td>
<td> 是否图表展示，既该参数是否需要进行曲线图绘制 </td>
</tr>
</tbody>
</table>

## 对某一个具体设备相关操作接口 ##

### 1. 获取设备详情 ###

调用此API，用户可以指定具体的设备序列号，获得该设备的详情。

> GET /company/device/info/:sn

`sn`为设备序列号，比如`T0000001`、`T0000007`...，在设备的面贴上可以查看到。

	REQUEST:
	GET /company/device/info/T0000001
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
            "device_info": {
				"name": "SCC65",
				"serial": "T0000001",
				"remark": "保温箱测试#1号",
				"offline_interval": "700",
				"sampling_interval": "60",
				"real_sampling_interval": "30",
				"status": "standby",
				"loc": "120.190862-30.261341",
				"tags": "杭州,余杭"，
				"rules": [
					{
						"variable": "t",
						"period": "5",
						"relation": ">",
						"value": "10"
					},
					{
						"variable": "t",
						"period": "5",
						"relation": "<",
						"value": "-5"
					}
				]
			}
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> device_info </td>
<td> Object </td>
<td> 设备信息 </td>
</tr>
<tr>
<td> name </td>
<td> String </td>
<td> 设备名称 </td>
</tr>
<tr>
<td> serial </td>
<td> String </td>
<td> 设备序列号 </td>
</tr>
<tr>
<td> remark </td>
<td> String </td>
<td> 设备描述 </td>
</tr>
<tr>
<td> offline_interval </td>
<td> String </td>
<td> 离线时间间隔，单位秒，用于判断设备是否离线，如果此值为0，则表示该设备为实时设备，离线判断不通过该值进行判断，否则离线判断通过此值进行判断。 </td>
</tr>
<tr>
<td> sampling_interval </td>
<td> String </td>
<td> 采集时间间隔（应大于设备数据GPRS上传间隔，如何设置请参考下面的QA），单位秒，此值表示设备采集的频率，该值设置时应该大于设备实际的采集频率（**real_sampling_interval**），当离线时间间隔为0时，设备是否离线通过此值进行判断。 </td>
</tr>
<tr>
<td> real_sampling_interval </td>
<td> String </td>
<td> 设备实际的采集时间间隔，单位秒，此值表示设备采集的实际频率，设置该值时请确认是否和设备的实际配置一致。 </td>
</tr>
<tr>
<td> status </td>
<td> String </td>
<td> 设备运行状态，running(运行中), stop(停机), standby（待机）中的一种 </td>
</tr>
<tr>
<td> loc </td>
<td> String </td>
<td> 设备地理位置，格式为：经度-纬度，采用WGS-84坐标系，应用中需要使用其他坐标系，请自行转换</td>
</tr>
<tr>
<td> tags </td>
<td> String </td>
<td> 设备标签，多个标签使用逗号(,)分割 </td>
</tr>
<tr>
<td> rules </td>
<td> Array </td>
<td> 报警规则，默认为空，需要用户自己在管理员平台中设置 </td>
</tr>
<tr>
<td> variable </td>
<td> String </td>
<td> 报警规则中的监控项，该值为设备参数中的 **variable_name** </td>
</tr>
<tr>
<td> period </td>
<td> String </td>
<td> 报警规则中的统计周期，实际取所有监控项中的最小值 </td>
</tr>
<tr>
<td> relation </td>
<td> String </td>
<td> 报警规则中的关系，为 **>**, **>=**, **<**, **<=** 中的一个 </td>
</tr>
<tr>
<td> value </td>
<td> String </td>
<td> 报警规则中阀值 </td>
</tr>
</tbody>
</table>

注解： 

**`loc`取值规则为**：设备如果没有基带芯片或GPS模块，则使用用户在添加设备时设置的默认的地理位置坐标，如果未设置则返回空字符串，如果设备带基带芯片但未带GPS模块，则使用最新获取到的一次基站地理位置值，如果设备带GPS芯片，则优先使用GPS获取的地理位置坐标。优先级如下： `GPS` > `基站位置` > `用户设置的默认地理位置`。

**设备离线判断机制**： 所有设备分为实时设备和非实时设备，比如冷藏柜、冷藏库、冷链车等设备为实时设备，保温箱、蓝牙记录仪等设备为非实时设备。用户可以通过`offline_interval`离线时间间隔是否为0来判断该设备是否是实时设备还是非实时设备，当`offline_interval`为0时该设备为实时设备，非0时则为非实时设备。

**实时设备判断**： 如果当前时间与设备最后一条最新数据的时间的时间间隔大于`sampling_interval` 采集时间间隔，则为`离线`状态，否则为`运行中`状态。

**非实时设备判断**: 如果当前时间与设备最后一条最新数据的时间的时间间隔大于`offline_interval` 离线时间间隔，则为`待机`状态，否则为`运行中`状态。

**Q**: `离线时间间隔(offline_interval)` 和 `采集时间间隔(sampling_interval)` 该如何设置？

**A**: 如果为实时设备，`离线时间间隔` 请设置为0，`采集时间间隔` 应该设置成大于设备的数据上报时间间隔（该值为一个经验值，一般取该值的1.5~2.5倍为宜，当然由于GPRS信号强弱与实际环境有很大关系，也需要和实际的情况来设置最适宜的值）。如果为非实时设备，则`离线时间间隔`应该设置成大于设备的数据上报时间间隔（该值为一个经验值，一般取该值的1.5~2.5倍为宜，当然也需要和实际的情况来设置最适宜的值，比如该设备为蓝牙记录仪，因没有主动设备上报的功能，则可以设置为一个月的时间，一个月后系统会标识为离线，用户可大致判断蓝牙记录仪1个月内未使用过），`采集时间间隔` 设置为设备实际设置的数据采集间隔的1.1~1.5倍，大于设备`实际的采集时间间隔(real_sampling_interval)`。

**Q**: `采集时间间隔(sampling_interval)` 与 `实际的采集时间间隔(real_sampling_interval)` 有什么区别？

**A**: `采集时间间隔(sampling_interval)` 主要用于系统平台判断设备是否离线还是在线的策略机制中，在绘制曲线图时用于曲线连续还是断开中。而`实际的采集时间间隔(real_sampling_interval)`为硬件设备的实际传感器采集的时间间隔，用于打印数据报表中显示的数据采集间隔。用户实际设置时不要把这两个变量弄混淆了。

### 2. 获取设备当前实时数据 ###

调用此API，用户可以指定具体的设备序列号，获得该设备的实时数据，亦是该设备在云端的最新一条数据。

> GET /company/device/data/realtime/:sn

`sn`为设备序列号，比如`T0000001`、`T0000007`...，在设备的面贴上可以查看到。

	REQUEST:
	GET /company/device/data/realtime/T0000001
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
			"data": [
				{
					"at": 1444292630,
					"value": {
						"t": "8.5",
						"lock": "off",
						"loc": "120.190862-30.261341",
						"v": "3770"
					}
				}
			]
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> data </td>
<td> Array </td>
<td> 实时数据，这里为一个数组 </td>
</tr>
<tr>
<td> at </td>
<td> Number </td>
<td> 设备最新数据的时间戳（UTC时间，秒数） </td>
</tr>
<tr>
<td> value </td>
<td> Object </td>
<td> 设备最新数据内容，该对象中的值与该设备的属性一一对应，这里就不展开介绍 </td>
</tr>
</tbody>
</table>

注意: 在一些设备参数中如果有 `loc` 或 `gps`字段，不管是基站位置还是GPS获取的位置，这里都会使用经纬度展示。 另外如果有一些变量的值为 `err`， 说明传感器有损坏，该点的值无效。

### 3. 获取设备历史数据 ###

调用此API，用户可以指定具体的设备序列号，获得该设备的历史数据。

> GET /company/device/data/history/:sn

`sn`为设备序列号，比如`T0000001`、`T0000007`...，在设备的面贴上可以查看到。

在请求的 `HTTP HEADER`中需要指定查询历史记录中的起始时间`Time-Start`和结束时间 `Time-End`, 该值为UTC时间（秒数），两值之差不能大于7天（60x60x24x7=604800秒），否则会返回`时间间隔溢出错误`(204)，此接口受调用次数限制（205），勿频繁调用。

	REQUEST:
	GET /company/device/data/history/T0000001
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>
	Time-Start: 1447488500
	Time-End: 1447488600

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
			"data": [
				{
					"at": 1447488503,
					"value": {
						"t": "8.5",
						"lock": "on",
						"loc": "120.190862-30.261341",
						"v": "3770"
					}
				},
				{
					"at": 1447488536,
					"value": {
						"t": "8.3"
					}
				}
				...
			]
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> data </td>
<td> Array </td>
<td> 实时数据，这里为一个数组 </td>
</tr>
<tr>
<td> at </td>
<td> Number </td>
<td> 设备最新数据的时间戳（UTC时间） </td>
</tr>
<tr>
<td> value </td>
<td> Object </td>
<td> 设备最新数据内容，该对象中的值与该设备的属性一一对应，这里就不展开介绍 </td>
</tr>
</tbody>
</table>

注意： 这里的历史数据和实时数据有一定的区别，在实时数据接口中，`value`对象的内容所携带的参数是完整的，而在历史数据中可能不完整，取决于设备中的参数是否是必选的（`required` 为 true 表示该参数是必须的，在每一个时间点上都会有，而 `required` 为 false 的参数则是非必选的，在每个时间点上可能有，也可能没有），详见设备型号信息API部分。

### 4. 获取设备最近一次的启动时间 ###

调用此API，用户可以指定具体的设备序列号，获得该设备最近一次的启动时间`startlog`。

> GET /company/device/event/start/:sn

`sn`为设备序列号，比如`T0000001`、`T0000007`...，在设备的面贴上可以查看到。

	REQUEST:
	GET /company/device/event/start/T0000001
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
			 "event": {
				"at": 1444291636,
				"type": "startlog",
			}
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> event </td>
<td> Object </td>
<td> 事件的对象 </td>
</tr>
<tr>
<td> at </td>
<td> Number </td>
<td> 事件发生的时间，UTC时间(单位秒) </td>
</tr>
<tr>
<td> type </td>
<td> String </td>
<td> 事件的类型, 这里固定为 **startlog** </td>
</tr>
</tbody>
</table>

### 5. 获取设备最近一次的结束时间 ###

调用此API，用户可以指定具体的设备序列号，获得该设备最近一次的结束时间`endlog`。

> GET /company/device/event/end/:sn

`sn`为设备序列号，比如`T0000001`、`T0000007`...，在设备的面贴上可以查看到。

	REQUEST:
	GET /company/device/event/end/T0000001
	
	HEADER:
	Api-Key: <your api-key>
	Api-Domain: <your domain>

	RESPONSE:
    {
        "at": 1444292636,
        "code": 200,
        "result": {
			 "event": {
				"at": 1444290636,
				"type": "endlog",
			}
        }
    }

<table>
<tbody>
<tr>
<th> id </th>
<th> 类型 </th>
<th> 备注 </th>
</tr>
<tr>
<td> event </td>
<td> Object </td>
<td> 事件的对象 </td>
</tr>
<tr>
<td> at </td>
<td> Number </td>
<td> 事件发生的时间，UTC时间(单位秒) </td>
</tr>
<tr>
<td> type </td>
<td> String </td>
<td> 事件的类型, 这里固定为 **endlog** </td>
</tr>
</tbody>
</table>

**应用提示**：用户可以调用设备的4，5接口获取设备最近一次的开启时间`startlog`和结束时间`endlog`，但返回的对象为空时，说明该设备尚未使用过，如果获取的`startlog`时间大于`endlog`, 说明此时设备正常工作中，如果`startlog`小于`endlog`，说明此时设备已被关闭。用于可以使用`starlog`和`endlog`的时间结合历史数据获取接口获取设备最近一次的温度记录数据。

## 错误编号 ##
	
	200 OK
	201 illegal apikey     - apikey错误
	202 invalid format     - 格式错误
	203 service busy       - 服务忙，请稍后再试
	204 interval overflow  - 时间溢出错误
	205 Frequent operation - 操作频繁
	207 no result          - 操作无结果 
